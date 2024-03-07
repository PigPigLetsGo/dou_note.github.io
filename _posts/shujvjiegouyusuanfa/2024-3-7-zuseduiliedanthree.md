---
layout: post
title:  阻塞队列-单锁实现-2中代码的漏洞3
categories: [java,数据结构与算法,算法,阻塞队列]
description: 阻塞队列-单锁实现-2中代码的漏洞3介绍。
keywords: 算法
---

# 阻塞队列-单锁实现-2中代码的漏洞3

声明：只是做了一个很小的改动 但是 作用却非常的大，这是一个经验！！！

考虑代码中的==漏洞==

漏洞在哪里呢？其实就在 判断 isFull 的 if 判断语句 那段代码中

为什么呢？if 怎么会有错呢？它不就是判断 队列是否 满了 ，如果满了 添加代码 不能立刻 执行 我得先让代码阻塞 将来队列不满了，我将它唤醒，唤醒后继续执行后面的添加代码。if  能有什么错呢？

**答**：我们通过如下表格来进行分析，表格的 从上到下 顺序就是按照 时间顺序执行的

| 操作前  | offer(4)                  | offer(5)                     | poll()                    | 操作后    |
| ------- | ------------------------- | ---------------------------- | ------------------------- | --------- |
| [1 2 3] | 队列满，进入tailWaits等待 |                              |                           | [1 2 3]   |
| [1 2 3] |                           |                              | 取走1，队列不满，唤醒线程 | [2 3]     |
| [2 3]   |                           | 抢先获得锁，发现不满，放入 5 |                           | [2 3 5]   |
| [2 3 5] | 从上次等待处直接向下执行  |                              |                           | [2 3 5 ?] |

## 问题分析

假设 这个 队列 它的 容量是 3 已经放满了元素，放了 1 2 3 已经放满了

然后先来的是 offer 方法 它要添加一个 4 ，当然 这时队列已经满了 如下 if 判断就已经成立了

```java
if(isFull()) {
   // 满了该做的事, offer 线程阻塞
   tailWaits.await(); // 当前线程加入 tailWaits, 并且让此线程阻塞   tailWaits.signal()
}
```

此时 offer(4) 这个线程就会进入 tailWaits 等待就不能添加了 offer(4) 由于 后面 没有加进去所以 操作后的状态 还是 1 2 3

接下来在时间点2这里，又来了一个线程是 poll 线程 ，poll 线程 是从 队列中 取走一个元素，取走一个元素后 队列 此时就不满了 这时就由 poll 线程来唤醒 tailWaits 中等待的线程 操作后的状态是 被取走了一个元素所以是 2 3

接下来在时间点3这里，时间点1 线程已经被唤醒了，它要和 时间点3 争抢锁 。假设是 offer(5) 也就是时间点3 线程 争抢到了锁，此时 队列中 有 2 个元素不满 所以 offer(5) 就直接 添加到 队列里面去了

此时 offer(5) 这个线程 执行完就释放锁了，然后就轮到 线程 1 执行了 它会从 wait 函数 往下执行 因为它是被唤醒的，但是呢 此时 队列已经是 2 3 5 已经满了啊，这时就出现问题了！

## 问题关键

-  从tailWaits 中唤醒的线程，会与新来的offer的线程争抢锁，谁能抢到是不一定的，如果后者先抢到，就会导致条件又发生变化
-  这种情况称之为 ==虚假唤醒==，还行后应该重新检查条件，看是不是重新进入等待

那么怎么 重新检查呢？

我们只需要将 if 条件 换成 while 条件就可以了 

while的效果是什么？当线程 1 被唤醒之后 就会又回到 while 所处的代码行 重新 执行判断 看看 isFull是否成立 如果 满 就代表成立那么 线程 1 就会又被 阻塞 咯！

如果队列 没有 满 那么 线程 1 才能 得以执行 添加 offer(4) 的代码

## 代码：

```java
/*
    1. synchronized 关键字, 功能少
    2. ReentrantLock 可重入锁, 功能多
 */
public class TestThreadUnsafe {
    private final String[] array = new String[10];
    private int tail = 0;
    private int size = 0;
    ReentrantLock lock = new ReentrantLock(); // 锁对象
    Condition tailWaits = lock.newCondition(); // 条件变量对象 集合

    public void offer(String e) throws InterruptedException {
        lock.lockInterruptibly(); // 加锁(可以在阻塞状态随时打断)
        try {
            // 使用while解决 虚假唤醒 问题
            while(isFull()) {
                // 满了该做的事, offer 线程阻塞
                tailWaits.await(); // 当前线程加入 tailWaits, 并且让此线程阻塞   tailWaits.signal()
            }
            array[tail] = e;
            if(++tail == array.length) {
                tail = 0;
            }
            size++;
        } finally {
            lock.unlock(); // 解锁
        }
    }

    private boolean isFull() {
        return size == array.length;
    }

    @Override
    public String toString() {
        return Arrays.toString(array);
    }

    public static void main(String[] args) throws InterruptedException {
        TestThreadUnsafe queue = new TestThreadUnsafe();
        for (int i = 0; i < 10; i++) {
            queue.offer("e" + i);
        }
        new Thread(()->{
            try {
                System.out.println(Thread.currentThread().getName() + "添加元素之前");
                queue.offer("e10");
                System.out.println(Thread.currentThread().getName() + "添加元素成功");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }, "t1").start();

        new Thread(()->{
            System.out.println("开始唤醒");
            try {
                queue.lock.lockInterruptibly();
                queue.tailWaits.signal();
            }
            catch (InterruptedException e)
            {
                throw new RuntimeException(e);
            }
            finally {
                queue.lock.unlock();
            }
        }, "t2").start();
    }
}
```

