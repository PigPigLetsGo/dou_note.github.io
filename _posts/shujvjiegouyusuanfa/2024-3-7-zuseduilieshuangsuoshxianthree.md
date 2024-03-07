---
layout: post
title:  阻塞队列-双锁实现3-最后阶段
categories: [java,数据结构与算法,算法,阻塞队列]
description: 阻塞队列-双锁实现3-最后阶段介绍。
keywords: 算法
---

# 阻塞队列-双锁实现3-最后阶段

到此 我们还没有讲述完 双锁实现 因为 还有一段 最为 ==精华== 的代码没有加上，是什么呢？

我们当时为什么使用 双锁来实现？

使用双锁 它的性能 要比使用 单锁 高，因为 两个线程之间是可以同时 获取自己的锁 然后去同时工作 而不用等对方

我们接下来的优化就是 减少 唤醒时 加锁的次数 也就是 offer 方法 中唤醒时 我尽量要把它的加锁次数 降到最低 类似的 poll也是

我们考虑如下情况：

假设来了3个 poll 线程 poll_1 poll_2 poll_3 它们要去获取元素但是队列是空的，如下代码成立

```java
 while(isEmpty())
    headWaits.await();
```

它会都进入了 阻塞

而后 又来了 3 个 offer 线程，每个offer 线程都会向队列去添加一个元素

如果是按照原来的方式就是 offer_1 添加元素后 拿到headLock 锁 然后 唤醒 poll_1 线程 然后释放锁 让 poll_1 去 取元素 

offer_2 做类似的操作 offer_3 也做类似的操作

那么按照原来的方式呢 3 个 offer 线程 加了 3 次锁 做了 3 次 唤醒操作，但是我们现在想要 减少 offer 线程 使用 headLock 锁的机会(次数) 给它使用haedLock锁的次数尽量降低，所以我们想出了这样一个策略 如下：

offer 线程有3个 我们不让 3 个offer线程都去执行headLock加锁 ，headWaits.signal 唤醒操作，我们只让其中一个线程做这件事儿，比如说 offer_1 它去执行 headLock加锁，headWaits.signal唤醒 。其它两个线程offer_2 和 offer_3 它们只做添加元素操作就行了它们不用去做加锁唤醒，那么它只能 唤醒 poll_1 这时 我们再让 poll_1 去唤醒 poll_2 让poll_2 呢 再去唤醒 poll_3这样就可以了。

也就是让offer线程 它只做一次 唤醒 剩余唤醒的活儿 就交给poll线程这边来完成了，这个思想我们称之为 级联通知

## 实现代码如下：

在如下代码中 的offer 和 poll方法中 分别定义了 c 变量 用于表示 是否要唤醒 poll的 阻塞线程

offer中的c变量的解释：当我们第一次添加元素时那么 肯定 是先由于 队列是空的 poll 线程才会被阻塞 所以 我们用c变量来 判断 如果 size 的值也就是 元素个数为 0 那么就赋值给 c 变量 下面进行判断 如果c == 0说明我们需要去唤醒一下poll 线程 剩下的offer线程size肯定就不是0 了 那么就不需要去唤醒 poll 线程，就只做添加元素操作即可

poll中c变量的解释：通过size赋值给c 判断剩余的元素还有多少 如果 元素个数 大于1 那么就自己唤醒 poll 线程中阻塞的线程即可

如下是完整的代码：

```java
/**
 * 双锁实现
 *
 * @param <E> 元素类型
 */
@SuppressWarnings("all")
public class BlockingQueue2<E> implements BlockingQueueImpl<E>
{

    private final E[] array;
    private int head;
    private int tail;
    // 创建 原子变量 保证 size 在 多线程下 的线程安全问题
    private AtomicInteger size = new AtomicInteger();

    // 创建两个锁 用于保护不同的线程 并且 创建对应的 条件变量让 某个线程不满足某条件后就 阻塞线程进入等待状态
    // 保护尾指针 住要给offer 方法用
    private ReentrantLock tailLock = new ReentrantLock();
    // 队列满了就不能添加了 那就到 tailWaits 中等待
    Condition tailWaits = tailLock.newCondition();
    // 保护头指针 主要给poll 方法用
    private ReentrantLock headLock = new ReentrantLock();
    // 队列空了就不能取元素了 那就到 headWaits 中等待
    Condition headWaits = headLock.newCondition();

    public BlockingQueue2(int capacity)
    {
        this.array = (E[]) new Object[capacity];
    }

    private boolean isEmpty()
    {
        return size.get() == 0;
    }

    private boolean isFull()
    {
        return size.get() == array.length;
    }

    @Override
    public void offer(E e) throws InterruptedException
    {
        // 加锁(阻塞时可以随时打断)
        tailLock.lockInterruptibly();
        // 添加前的元素个数 通过这个变量 我们 判断是否 让某一个线程去 唤醒 poll 线程而不是都去唤醒
        int c;
        try
        {
            // 队列满就等待
            while(isFull())
                tailWaits.await();
            // 队列不满则将元素添加到尾部
            array[tail] = e;
            if(++tail == array.length)
                tail = 0;
            // 元素个数增加
            // getAndIncrement 效果就是 size++ 保证size的 原子性
            c = size.getAndIncrement(); // size++;
            /**
             * 1. 读取成员变量size的值
             * 2. 自增
             * 3. 结果写回成员变量size
             */
            // 判断如果 c + 1 小于 数组长度说明还有 添加空间 那么就 级联唤醒 offer 线程
            if(c + 1 < array.length)
                tailWaits.signal();
        }finally
        {
            // 释放锁
            tailLock.unlock();
        }

        // 如果从0变为非空，由offer这边唤醒等待非空的poll线程
        //    0 -> 1  1 -> 2  2 -> 3
        if(c == 0)
        {
            // 加上headWaits.signal();需要的 headLock锁
            headLock.lockInterruptibly();
            try
            {
                // 必须 配合 配对的锁一起使用否则会就报错
                headWaits.signal();
            }finally
            {
                // 释放 headLock的锁
                headLock.unlock();
            }
        }
    }

    @Override
    public E poll() throws InterruptedException
    {
        E e;
        // 加锁(阻塞时可以随时打断)
        headLock.lockInterruptibly();
        // 取走前的元素个数
        int c;
        try
        {
            // 1. 队列为空则等待
            // 使用while来判断队列是否满了 防止 虚假唤醒
            while(isEmpty())
                headWaits.await();
            // 2. 非空则出队列
            e = array[head];
            array[head] = null; // help GC
            if(++head == array.length)
                head = 0;
            // 元素个数 自减
            // getAndDecrement 效果就是 size-- 保证size的 原子性
            c = size.getAndDecrement(); // size--;
            /**
             * 1. 读取成员变量size的值
             * 2. 自减
             * 3. 结果写回成员变量size
             */
            // 判读 如果 c 也就是元素个数 不小于1 说明还有元素可以取 那么就 级联唤醒 poll 线程
            if(c > 1)
                headWaits.signal();
        }finally
        {
            // 释放锁
            headLock.unlock();
        }

        // 队列从满 -> 不满时 由poll唤醒等待不满的 offer 线程
        if(c == array.length)
        {
            // 加上tailWaits.signal();需要的 tailLock锁
            tailLock.lockInterruptibly();
            try
            {
                // 必须 配合 配对的锁一起使用否则会就报错
                tailWaits.signal();
            }finally
            {
                // 释放 headLock的锁
                tailLock.unlock();
            }
        }
        return e;
    }

    @Override
    public boolean offer(E e, long timeout) throws InterruptedException
    {
        return false;
    }

    @Override
    public String toString()
    {
        return "BlockingQueue2{" + "array=" + Arrays.toString(array) + '}';
    }

    public static void main(String[] args) throws InterruptedException
    {
        BlockingQueue2<String> queue = new BlockingQueue2<>(3);
        queue.offer("元素1");
        queue.offer("元素2");
        new Thread(()->{
            try {
                queue.offer("元素3");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }, "offer").start();

        new Thread(()->{
            try {
                queue.poll();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }, "poll").start();
    }
}
```

