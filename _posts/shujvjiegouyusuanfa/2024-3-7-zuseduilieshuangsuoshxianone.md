---
layout: post
title:  阻塞队列-双锁实现1
categories: [java,数据结构与算法,算法,阻塞队列]
description: 阻塞队列-双锁实现1介绍。
keywords: 算法
---

# 阻塞队列-双锁实现1

## 回忆为什么加锁

之前在 阻塞队列-单锁实现中 我们使用 一把锁 保证了 队列 的线程安全，下面我们继续探讨下看看 这把锁有没有 优化的可能

我们先回忆我们会为什么要加如下这把锁在offer方法中呢？

```java
 	 @Override
    public void offer(E e) throws InterruptedException // poll 等待队列非空
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        ... [省略 代码]
```

就是因为在 多线程 下 如果我们不加这把锁那么方法内的代码在执行时就有可能受到其它线程的影响产生执行交错的情况，比如说两个线程都来执行offer方法的代码-如下图：

![image-20240120223221812](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401202232936.png)

第一个红色的线程它执行完了1 array[tail] = e 代码还没来得及执行 4 ++tail 它就切换到了 蓝色线程 去执行了 2 3 人家执行完了 CPU 才切换回去执行红色线程的 4 那这样的指令交错呢就可能导致最终的结果不正确，因此为了避免多线程下产生这种指令交错 我们需要加上锁 这样就可以保证多行代码执行的 原子性

上面讲述就是我们为什么要加锁

## 问题

但是加锁后呢会有新的问题

什么问题呢？

我们offer方法呢 它需要加锁 poll方法也需要加锁 它们使用的是 同一把锁

![image-20240120224140926](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401202241996.png)

那么这两个方法在多线程执行时会不会相互影响呢？

比如说有一个offer线程它要到队列的尾部添加元素，又有一个poll线程 它要到队列的 头部去 取走元素，那么offer 线程 添加元素的同时poll线程能执行吗？	

答： 不能执行，因为offer线程已经加了锁了，它除非等offer线程把代码执行完把锁释放了 poll线程才能获得锁

所以现在的效果就是 offer  和 poll  方法 它们是相互影响的，一个在执行的时候另一个就得阻塞

其实这样并不合理，因为offer 线程 它主要就是操作 队列的尾部 ，都是 tail 这个尾指针去读取 尾指针索引处的值 去让尾指针自增，它操作的都是队列的尾部

而poll 线程它操作的都是 队列的 头部 操作的都是head指针，按理来说 它们 应该互不干扰

但是我们现在使用一把锁它们就冲突了，一个执行的时候另一个就得阻塞，那怎么解决这个问题呢，下面就需要使用到双锁来实现了

## 分析

我们将offer线程和poll线程分别使用不同的锁 当offer线程上锁时并不会影响到 poll线程，所以就有如下代码：

```java
/**
 * 双锁实现
 *
 * @param <E> 元素类型
 */
@SuppressWarnings("all")
public class BlockingQueue2<E> implements BlockingQueueImpl<E>
{
	 // 定义数组
    private final E[] array;
    // 定义head和tail指针
    private int head;
    private int tail;
    // 队列中元素个数
    private int size;

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
        return size == 0;
    }

    private boolean isFull()
    {
        return size == array.length;
    }

    @Override
    public String toString()
    {
        return null;
    }

    @Override
    public void offer(E e) throws InterruptedException
    {
    }

    @Override
    public E poll() throws InterruptedException
    {
        return null;
    }

    @Override
    public boolean offer(E e, long timeout) throws InterruptedException
    {
        return false;
    }

    public static void main(String[] args) throws InterruptedException
    {
    }
}
```

实现了 offer 方法和 poll 方法的代码如下：

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
    private int size;

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
        return size == 0;
    }

    private boolean isFull()
    {
        return size == array.length;
    }

    @Override
    public String toString()
    {
        return null;
    }

    @Override
    public void offer(E e) throws InterruptedException
    {
        // 加锁(阻塞时可以随时打断)
        tailLock.lockInterruptibly();
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
            size++;

        }finally
        {
            // 释放锁
            tailLock.unlock();
        }
    }

    @Override
    public E poll() throws InterruptedException
    {
        // 加锁(阻塞时可以随时打断)
        headLock.lockInterruptibly();
        try
        {
            // 1. 队列为空则等待
            // 使用while来判断队列是否满了 防止 虚假唤醒
            while(isEmpty())
                headWaits.await();
            // 2. 非空则出队列
            E e = array[head];
            array[head] = null; // help GC
            if(++head == array.length)
                head = 0;
            // 元素个数 自减
            size--;
            return e;
        }finally
        {
            // 释放锁
            headLock.unlock();
        }
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
}
```

测试代码如下：

```java
public class Testdemo
{
    public static void main(String[] args) throws InterruptedException
    {
        BlockingQueue2<String> queue = new BlockingQueue2<>(3);
        queue.offer("任务1");
        new Thread(() ->
        {
            try
            {
                queue.offer("任务2");
            }
            catch (InterruptedException e)
            {
                throw new RuntimeException(e);
            }
        }, "offer").start();

        new Thread(() ->
        {
            try
            {
                System.out.println(queue.poll());
            }
            catch (InterruptedException e)
            {
                throw new RuntimeException(e);
            }
        }, "poll").start();
    }
}
```

下面我们debug 查看下 双锁的 线程执行情况是怎么样的：

debug启动

![image-20240121100456104](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211004171.png)

此时我们可以看到 队列中 存在了 任务1 元素，因为 在测试的类中 先存入了一个 任务1

那么offer 和 poll之间会互相干扰吗？ 先看下offer的线程执行情况

![image-20240121100648493](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211006551.png)

进入 offer 方法后 并获取 锁

那么这时 offer 线程 获取了 tailLock的锁 那么它会不会影响 poll 线程的锁呢？我们看下Poll 线程的情况

![image-20240121100948298](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211009349.png)

![image-20240121101014230](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211010280.png)

我们接着向下运行进入poll方法

![image-20240121101047533](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211010590.png)

它也同时 获取了 headLock 锁 执行下面代码操作，它们之间互不干扰 你放你的 任务2  我取我的 任务1

## 现有代码的问题

有什么问题呢？

问题就出现在 offer 和 poll的 修改size 那里，修改size 看去是 一行代码，但实际上它的底层是分成了 三步 才能完成 这个自增或者自减的操作

**哪三步呢？如下解释**:

1.  读取成员变量size的值
2.  自增
3.  结果写回成员变量size

既然size++和size- - 底层都是分成了多行代码才能完成的自增和自减操作，那么问题就来了，多线程下这些指令有没有可能出现交错的情况

答：可能的

比如下面举例：

假如 size 初始值为 5，那么size++底层 先是读取了 5

```
1.  读取成员变量size的值  5
2.  自增
3.  结果写回成员变量size
```

它还没来得及 进行 +1 操作 这时 CPU 就切换到了 poll中了而 poll线程 中的 size- - 也是 先要读取 当前size的值

因为 offer 线程没来得及自增 所以 poll 线程读取到的 size值 还是5

```
1.  读取成员变量size的值  5
# 接下来做了自减
2.  自减 4
# 然后把 4 写回到成员变量了
3.  结果写回成员变量size 4
```

当CPU切换到 offer线程后，由于offer 线程size++的底层第一步 读取了size的值也就是5 所以它还是从 5 的基础上 + 1 结果写回成员变量为 6

此时呢对照图如下：

![image-20240121103621860](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211036927.png)

这种情况对吗？两个线程一个 加了 一个元素 一个 取了一个元素 按道理 应该还是 5啊 怎么就多了一个呢？

这种情况显然是不对的，这就说明 有一个计算结果被不正确的覆盖掉了

### 解释这种情况产生的原因：

这是因为 由于offer线程使用的是 tailLock 锁，而poll线程使用的是 headLock 锁 两个锁不同 可以同时执行，它们都在修改size那么这时就是线程不安全的，因为 它们同时上锁执行时 一个正在做 + 的同时  另一个做了 - 这样就会产生指令的交错 导致 错误的结果

那怎么保证size的线程安全呢？

下面就讲述一个知识叫 ==原子变量==

## 原子变量

我们使用 原子 整数类 来保证 size的安全

如下代码改变了 size 成员变量的 方式 改为了 原子变量类型

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
            size.getAndIncrement(); // size++;
            /**
             * 1. 读取成员变量size的值
             * 2. 自增
             * 3. 结果写回成员变量size
             */
        }finally
        {
            // 释放锁
            tailLock.unlock();
        }
    }

    @Override
    public E poll() throws InterruptedException
    {
        // 加锁(阻塞时可以随时打断)
        headLock.lockInterruptibly();
        try
        {
            // 1. 队列为空则等待
            // 使用while来判断队列是否满了 防止 虚假唤醒
            while(isEmpty())
                headWaits.await();
            // 2. 非空则出队列
            E e = array[head];
            array[head] = null; // help GC
            if(++head == array.length)
                head = 0;
            // 元素个数 自减
            // getAndDecrement 效果就是 size-- 保证size的 原子性
            size.getAndDecrement(); // size--;
            /**
             * 1. 读取成员变量size的值
             * 2. 自减
             * 3. 结果写回成员变量size
             */
            return e;
        }finally
        {
            // 释放锁
            headLock.unlock();
        }
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
}
```

## 如何唤醒等待线程

我们先来考虑这么一个场景比如说 一开始队列是空的，现在有一个poll线程从队列中获取元素 此时不能获取到因为 队列是 空的，那么就会 进入 等待状态

```java
 while(isEmpty())
    headWaits.await(); // poll_1 线程进入等待
```

当然可能会有多个比如 poll_2线程也来获取元素发现还是空的就进入了等待

```java
 while(isEmpty())
    headWaits.await(); // poll_1 线程进入等待  poll_2 线程进入等待
```

那么 headWaits 中等待的线程我们什么时候去唤醒它们呢？

得等待队列中有元素了才能唤醒 也就是offer 线程添加了一个元素了

那么你可能会想到这么写：

```java
@Override
public void offer(E e) throws InterruptedException
{
   // 加锁(阻塞时可以随时打断)
   tailLock.lockInterruptibly();
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
      size.getAndIncrement(); // size++;
      /**
             * 1. 读取成员变量size的值
             * 2. 自增
             * 3. 结果写回成员变量size
             */
      headWaits.signal();
   }finally
   {
      // 释放锁
      tailLock.unlock();
   }
}
```

那么这么写 对不对呢

通过如下代码 进行测试看看对不对：

```java
    @Test
    public void test()
    {
        BlockingQueue2<String> queue = new BlockingQueue2<>(3);
        new Thread(() ->
        {
            try
            {
                String poll = queue.poll();
                System.out.println("poll1 线程: " + poll);
            }
            catch (InterruptedException e)
            {
                throw new RuntimeException(e);
            }
        }, "poll_1").start();

        new Thread(() ->
        {
            try
            {
                String poll = queue.poll();
                System.out.println("poll2 线程: " + poll);
            }
            catch (InterruptedException e)
            {
                throw new RuntimeException(e);
            }
        }, "poll_2").start();

        new Thread(() ->
        {
            try
            {
                queue.offer("任务");
            }
            catch (InterruptedException e)
            {
                throw new RuntimeException(e);
            }
        }, "offer").start();
    }
```

我们在queue.offer(“任务”) 处打上断点 让它先不进行添加，先让 前面两个poll线程进入阻塞，然后我们重点看下 添加一个元素后能不能唤醒 上面两个poll其中一个线程

debug执行后可以看到headWaits中加入了两个poll线程的阻塞

![image-20240121150845350](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211508417.png)

当我们运行后就会报错如下：

![image-20240121150330974](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211503052.png)

报错位置的代码如下：

offer方法中的 headWaits.signal 既然报错了

![image-20240121150940910](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211509981.png)

为什么它会出错呢？如下分析：

其实之前的文章提到过，不管是 await(阻塞) 方法还是 signal(唤醒) 方法 ，它们都必须配合锁一起使用，也就是 在调用这两个方法之前必须都加好锁，它俩调用完了后必须解锁否则将来就会报错

你可能会想说，这里报错的这个signal已经配合锁一起使用了呀 进入offer方法前 执行了`tailLock.lockInterruptibly();` 结束时必须执行 `tailLock.unlock();`，为什么会报错了

==注意==：我们不光是要让signal 配合锁一起使用 而且呢 与 signal一起使用的锁 必须配对

上面  锁 必须配对 是什么意思呢？

就是说 signal 必须是 headLock 这个锁对象一起配对使用，而 tailWaits 才能配对 tailLock 一起使用

到这里就清楚 为什么报错 会在 headWaits.signal 了吧

那这个问题怎么解决呢？

我们只需要在 headWaits.signal 的前后加上 与它配对的锁就行了，为了防止报错 没有释放锁 使用 try finally 进行包裹 headWaits.signal，代码如下：

```java
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
```

我们在 headLock.lockInterruptibly(); 代码处打上断点 进行查看当offer线程 添加元素时 是哪个 poll 线程 获取到的 offer添加的元素

![image-20240121152559807](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211525914.png)

当程序走到–释放锁时 此时就会 唤醒 headWaits 中的一个线程去 取元素

![image-20240121152650095](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211526159.png)

打印结果：

![image-20240121152904479](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211529532.png)
