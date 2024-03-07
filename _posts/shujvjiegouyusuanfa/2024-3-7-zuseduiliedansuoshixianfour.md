---
layout: post
title:  阻塞队列-单锁实现3-完整实现
categories: [java,数据结构与算法,算法,阻塞队列]
description: 阻塞队列-单锁实现3-完整实现介绍。
keywords: 算法
---

# 阻塞队列-单锁实现3-完整实现

阻塞队列的1篇和2篇 都看完了 下面就是完整的实现代码了

先编写一个 阻塞队列接口 如下：

```java
/**
 目前队列存在的问题
 <ol>
    <li>很多场景要求<b>分离</b>生产者、消费者两个<b>角色</b>、它们得由不同的线程来担当，而之前的实现根本没有考虑线程安全问题</li>
    <li>队列为空，那么在之前的实现里会返回 null，如果就是硬要拿到一个元素呢？只能不断循环尝试</li>
    <li>队列为满，那么再之前的实现里会返回 false，如果就是硬要塞入一个元素呢？只能不断循环尝试</li>
 </ol>

 解决方法
 <ol>
     <li>用锁保证线程安全</li>
     <li>用条件变量让 poll 或 offer 线程进入<b>等待</b>状态，而不是不断循环尝试，让 CPU 空转</li>
 </ol>
 */

public interface BlockingQueueImpl<E> { // 阻塞队列

    void offer(E e) throws InterruptedException;

    boolean offer(E e, long timeout) throws InterruptedException;

    E poll() throws InterruptedException;
}
```

我们先来讲解一下 阻塞队列接口中的 offer 和 poll 方法

首先它们内部都会用到 条件变量 ，条件变量会 抛出 检查异常所以 函数声明 中 都进行了 throws 抛出异常

offer：使用了 void 返回值 类型，因为 如果 队列满了就让当前线程阻塞不需要返回状态了

实现代码：

```java
public class BlockingQueue<E> implements BlockingQueueImpl<E>
{
    // 定义数组
    private final E[] array;
    // 定义head和tail指针
    private int head;
    private int tail;
    // 队列中元素个数
    private int size;
    // 保证 队列的 线程安全 创建一个 锁对象
    private ReentrantLock lock = new ReentrantLock();
    /**
     * 两个条件变量
     * 在我们将来offer 以及poll 这两个方法 暂时没有办法向队列添加/取出元素时让线程进入阻塞状态
     */
    // 配合poll使用，在队列为空时poll方法没办法继续执行了，那么就放到headWaits中等待
    Condition headWaits = lock.newCondition();
    // 配合offer使用，在队列满时offer方法没办法继续执行了，那么就放到tailWaits中等待
    Condition tailWaits = lock.newCondition();

    // 通过构造函数 给数组 赋 初始值
    public BlockingQueue(int capacity)
    {
        array = (E[]) new Object[capacity];
    }

    // 判断 队列是否为空
    private boolean isEmpty()
    {
        return size == 0;
    }

    // 判断 队列是否满了
    private boolean isFull()
    {
        return size == array.length;
    }

    @Override
    public void offer(E e) throws InterruptedException // poll 等待队列非空
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        try
        {
            // 使用while来进行 重复检查 队列是否满了, 如果队列满了就阻塞线程
            while(isFull())
                tailWaits.wait();
            // 将 要添加的元素 添加到 array[tail] 位置
            array[tail] = e;
            // 判断tail 是否 指向了 数组长度 也就是是否满了 如果满了 就将tail 重置为0
            if(++tail == array.length)
                tail = 0;
            // 自增 增加 队列中元素个数(记录)
            size++;
            // 由于 队列中 添加了一个元素 队列部位空了 就唤醒 headWaits 中的阻塞线程让其进行取出元素
            headWaits.signal();
        }finally
        {
            // 释放锁
            lock.unlock();
        }
    }

    @Override
    public boolean offer(E e, long timeout) throws InterruptedException
    {
        return false;
    }

    @Override
    public E poll() throws InterruptedException // offer等待线程为空
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        try
        {
            // 判断队列是否为空，如果队列为空就将当前取出元素的线程进行阻塞
            while(isEmpty())
            {
                headWaits.wait();
            }
            // 取出head处的元素
            E e = array[head];
            // 将当前取出的元素位置赋值为null 让GC垃圾回收机制进行处理
            array[head] = null; // help GC
            // 判断head是否指向了 数组长度，如果是那么就重置head
            if(++head == array.length)
                head = 0;
            // 每次取出元素就将 元素个数 自减
            size--;
            // 取出元素后就表示 队列中被取出一个元素 那么此时队列肯定是不满了 那么就唤醒 tailWaits 中的阻塞对象让其进行添加元素
            tailWaits.signal();
            // 返回 取出的元素
            return e;
        }
        finally
        {
            // 释放锁
            lock.unlock();
        }
    }
}
```

上面代码中确实是实现了 对于 队列的线程安全的需求了，但是呢 如果offer方法 线程等待后迟迟没有人 唤醒它呢 ，所以我们要实现一个可以给等待加上时间上限的offer方法

代码如下：

```java
public class BlockingQueue<E> implements BlockingQueueImpl<E>
{
    // 定义数组
    private final E[] array;
    // 定义head和tail指针
    private int head;
    private int tail;
    // 队列中元素个数
    private int size;
    // 保证 队列的 线程安全 创建一个 锁对象
    private ReentrantLock lock = new ReentrantLock();
    /**
     * 两个条件变量
     * 在我们将来offer 以及poll 这两个方法 暂时没有办法向队列添加/取出元素时让线程进入阻塞状态
     */
    // 配合poll使用，在队列为空时poll方法没办法继续执行了，那么就放到headWaits中等待
    Condition headWaits = lock.newCondition();
    // 配合offer使用，在队列满时offer方法没办法继续执行了，那么就放到tailWaits中等待
    Condition tailWaits = lock.newCondition();

    // 通过构造函数 给数组 赋 初始值
    public BlockingQueue(int capacity)
    {
        array = (E[]) new Object[capacity];
    }

    // 判断 队列是否为空
    private boolean isEmpty()
    {
        return size == 0;
    }

    // 判断 队列是否满了
    private boolean isFull()
    {
        return size == array.length;
    }

    @Override
    public void offer(E e) throws InterruptedException // poll 等待队列非空
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        try
        {
            // 使用while来进行 重复检查 队列是否满了, 如果队列满了就阻塞线程
            while(isFull())
                tailWaits.wait();
            // 将 要添加的元素 添加到 array[tail] 位置
            array[tail] = e;
            // 判断tail 是否 指向了 数组长度 也就是是否满了 如果满了 就将tail 重置为0
            if(++tail == array.length)
                tail = 0;
            // 自增 增加 队列中元素个数(记录)
            size++;
            // 由于 队列中 添加了一个元素 队列部位空了 就唤醒 headWaits 中的阻塞线程让其进行取出元素
            headWaits.signal();
        }finally
        {
            // 释放锁
            lock.unlock();
        }
    }

    // 在上面原有的offer的基础上多了一个选择

    /**
     * 原有的offer方法当 队列 满了之后就会进入等待，那么问题就来了 它会等多久呢？
     * 它需要等到有其它的线程来唤醒它 比如说 下面的 poll 线程
     * 如果我不想等这么久，比如 迟迟等不到 有线程来 唤醒，我想给它加一个等待的时间上限呢?
     * 这时我们就需要用到 下面新的 offer 方法了
     * @param e 要添加的元素
     * @param timeout 等待的时间上限 (超时 时间)
     * @return 如果超时没有添加成功就返回false 添加成功返回true
     * @throws InterruptedException
     */
    @Override
    public boolean offer(E e, long timeout) throws InterruptedException
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        try
        {
            // 将毫秒转换为纳秒 方便 传值
            // 因为下面的awaitNanos需要的是 纳秒 而我们传入 纳秒是不方便的 所以我们使用上面的API将用户传入的毫秒转换为纳秒使用
            long nanos = TimeUnit.MILLISECONDS.toNanos(timeout);
            // 使用while来进行 重复检查 队列是否满了, 如果队列满了就阻塞线程
            while(isFull())
                // awaitNanos 接收一个 时间上限的等待
                // 时间单位是什么呢？ 就是 它本身后半段语句 Nanos 就是 纳秒的意思
                // 纳秒使用呢 用户传入是不方便的，我们通过上面的TimeUnit.MILLISECONDS.toNanos来将 用户传入的毫秒转换为纳秒使用
                tailWaits.awaitNanos(nanos);
            // 将 要添加的元素 添加到 array[tail] 位置
            array[tail] = e;
            // 判断tail 是否 指向了 数组长度 也就是是否满了 如果满了 就将tail 重置为0
            if(++tail == array.length)
                tail = 0;
            // 自增 增加 队列中元素个数(记录)
            size++;
            // 由于 队列中 添加了一个元素 队列部位空了 就唤醒 headWaits 中的阻塞线程让其进行取出元素
            headWaits.signal();
        }finally
        {
            // 释放锁
            lock.unlock();
        }
        return false;
    }

    @Override
    public E poll() throws InterruptedException // offer等待线程为空
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        try
        {
            // 判断队列是否为空，如果队列为空就将当前取出元素的线程进行阻塞
            while(isEmpty())
            {
                headWaits.wait();
            }
            // 取出head处的元素
            E e = array[head];
            // 将当前取出的元素位置赋值为null 让GC垃圾回收机制进行处理
            array[head] = null; // help GC
            // 判断head是否指向了 数组长度，如果是那么就重置head
            if(++head == array.length)
                head = 0;
            // 每次取出元素就将 元素个数 自减
            size--;
            // 取出元素后就表示 队列中被取出一个元素 那么此时队列肯定是不满了 那么就唤醒 tailWaits 中的阻塞对象让其进行添加元素
            tailWaits.signal();
            // 返回 取出的元素
            return e;
        }
        finally
        {
            // 释放锁
            lock.unlock();
        }
    }
}
```

注意：我们使用的 给等待加上时间上限的 offer 方法是有问题的，它有 虚假唤醒的问题。

比如 timeout参数 用户传入了 时间上限为 5秒，5秒之内如果队列还是满的那么应该返回false表示没有添加成功

我们考虑虚假唤醒 比如时间已经过去了 1秒了，过去1 秒之后 poll线程 取走一个元素此时 队列布满了 poll线程把 tailWaits 中等待的线程唤醒，然后到了tailWaits.awaitNanos(nanos); 唤醒，但是这次唤醒是个虚假唤醒 也就是并发还有另外一个offer线程，人家又把队列放满了 当 while(isFull) 再次循环的时候 isFull 还是 true那 还得接着等待，正常 等待是 从 4秒开始等待就行了，因为已经过去了1秒了，但是按照现在的代码它是从参数传过来的时间值，它是每次循环的时候都会等待 5秒，所以我们需要对等待时间做一个调整

上面的问题其实很好解决，因为 人家API已经帮我们考虑好了这个tailWaits.awaitNanos(nanos)会有一个返回值，这个返回值就代表等待的剩余时间，比如说已经 等了 1秒 剩下还有 4秒，那么返回值就是 剩下的 4秒时间

所以我们只需要判断 nanos时间是否 等完了 也就是 是否小于等于 0了 表示没有剩余时间了，这个队列还是满的 那么就放弃等待 返回 false表示添加失败了

对应代码：

```java
// nanos 小于等于表示没有等待时间了 超时了 返回false表示 添加失败了
if(nanos <= 0)
	return false;	
```

如果 nanos 是大于 0 的说明 遭遇了虚假唤醒，还有一些时间需要继续等待 那么我就继续等 上次更新后的nanos时间

对应代码：

```java
// 每次都将 nanos 时间更新为 当前剩余的时间
nanos = tailWaits.awaitNanos(nanos);
```

如果 队列不满 并且 元素添加 成功了 那么就返回 true 表示添加元素成功了

offer方法完整代码如下：

```java
// 在上面原有的offer的基础上多了一个选择
/**
     * 原有的offer方法当 队列 满了之后就会进入等待，那么问题就来了 它会等多久呢？
     * 它需要等到有其它的线程来唤醒它 比如说 下面的 poll 线程
     * 如果我不想等这么久，比如 迟迟等不到 有线程来 唤醒，我想给它加一个等待的时间上限呢?
     * 这时我们就需要用到 下面新的 offer 方法了
     * @param e 要添加的元素
     * @param timeout 等待的时间上限 (超时 时间)
     * @return 如果超时没有添加成功就返回false 添加成功返回true
     * @throws InterruptedException
     */
@Override
public boolean offer(E e, long timeout) throws InterruptedException
{
   // 加锁(阻塞时可以随时打断)
   lock.lockInterruptibly();
   try
   {
      // 将毫秒转换为纳秒 方便 传值
      // 因为下面的awaitNanos需要的是 纳秒 而我们传入 纳秒是不方便的 所以我们使用上面的API将用户传入的毫秒转换为纳秒使用
      long nanos = TimeUnit.MILLISECONDS.toNanos(timeout);
      // 使用while来进行 重复检查 队列是否满了, 如果队列满了就阻塞线程
      while(isFull())
      {
         // awaitNanos 接收一个 时间上限的等待
         // 时间单位是什么呢？ 就是 它本身后半段语句 Nanos 就是 纳秒的意思
         // 纳秒使用呢 用户传入是不方便的，我们通过上面的TimeUnit.MILLISECONDS.toNanos来将 用户传入的毫秒转换为纳秒使用
         // nanos 小于等于表示没有等待时间了 超时了 返回false表示 添加失败了
         if(nanos <= 0)
            return false;
         // 每次都将 nanos 时间更新为 当前剩余的时间
         nanos = tailWaits.awaitNanos(nanos);
      }
      // 将 要添加的元素 添加到 array[tail] 位置
      array[tail] = e;
      // 判断tail 是否 指向了 数组长度 也就是是否满了 如果满了 就将tail 重置为0
      if(++tail == array.length)
         tail = 0;
      // 自增 增加 队列中元素个数(记录)
      size++;
      // 由于 队列中 添加了一个元素 队列部位空了 就唤醒 headWaits 中的阻塞线程让其进行取出元素
      headWaits.signal();
      return true;
   }finally
   {
      // 释放锁
      lock.unlock();
   }
}
```

完整的代码：

```java
public class BlockingQueue<E> implements BlockingQueueImpl<E>
{
    // 定义数组
    private final E[] array;
    // 定义head和tail指针
    private int head;
    private int tail;
    // 队列中元素个数
    private int size;
    // 保证 队列的 线程安全 创建一个 锁对象
    private ReentrantLock lock = new ReentrantLock();
    /**
     * 两个条件变量
     * 在我们将来offer 以及poll 这两个方法 暂时没有办法向队列添加/取出元素时让线程进入阻塞状态
     */
    // 配合poll使用，在队列为空时poll方法没办法继续执行了，那么就放到headWaits中等待
    Condition headWaits = lock.newCondition();
    // 配合offer使用，在队列满时offer方法没办法继续执行了，那么就放到tailWaits中等待
    Condition tailWaits = lock.newCondition();

    // 通过构造函数 给数组 赋 初始值
    public BlockingQueue(int capacity)
    {
        array = (E[]) new Object[capacity];
    }

    // 判断 队列是否为空
    private boolean isEmpty()
    {
        return size == 0;
    }

    // 判断 队列是否满了
    private boolean isFull()
    {
        return size == array.length;
    }

    @Override
    public void offer(E e) throws InterruptedException // poll 等待队列非空
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        try
        {
            // 使用while来进行 重复检查 队列是否满了, 如果队列满了就阻塞线程
            while(isFull())
                tailWaits.wait();
            // 将 要添加的元素 添加到 array[tail] 位置
            array[tail] = e;
            // 判断tail 是否 指向了 数组长度 也就是是否满了 如果满了 就将tail 重置为0
            if(++tail == array.length)
                tail = 0;
            // 自增 增加 队列中元素个数(记录)
            size++;
            // 由于 队列中 添加了一个元素 队列部位空了 就唤醒 headWaits 中的阻塞线程让其进行取出元素
            headWaits.signal();
        }finally
        {
            // 释放锁
            lock.unlock();
        }
    }

    // 在上面原有的offer的基础上多了一个选择

    /**
     * 原有的offer方法当 队列 满了之后就会进入等待，那么问题就来了 它会等多久呢？
     * 它需要等到有其它的线程来唤醒它 比如说 下面的 poll 线程
     * 如果我不想等这么久，比如 迟迟等不到 有线程来 唤醒，我想给它加一个等待的时间上限呢?
     * 这时我们就需要用到 下面新的 offer 方法了
     * @param e 要添加的元素
     * @param timeout 等待的时间上限 (超时 时间)
     * @return 如果超时没有添加成功就返回false 添加成功返回true
     * @throws InterruptedException
     */
    @Override
    public boolean offer(E e, long timeout) throws InterruptedException
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        try
        {
            // 将毫秒转换为纳秒 方便 传值
            // 因为下面的awaitNanos需要的是 纳秒 而我们传入 纳秒是不方便的 所以我们使用上面的API将用户传入的毫秒转换为纳秒使用
            long nanos = TimeUnit.MILLISECONDS.toNanos(timeout);
            // 使用while来进行 重复检查 队列是否满了, 如果队列满了就阻塞线程
            while(isFull())
            {
                // awaitNanos 接收一个 时间上限的等待
                // 时间单位是什么呢？ 就是 它本身后半段语句 Nanos 就是 纳秒的意思
                // 纳秒使用呢 用户传入是不方便的，我们通过上面的TimeUnit.MILLISECONDS.toNanos来将 用户传入的毫秒转换为纳秒使用
                // nanos 小于等于表示没有等待时间了 超时了 返回false表示 添加失败了
                if(nanos <= 0)
                    return false;
                // 每次都将 nanos 时间更新为 当前剩余的时间
                nanos = tailWaits.awaitNanos(nanos);
            }
            // 将 要添加的元素 添加到 array[tail] 位置
            array[tail] = e;
            // 判断tail 是否 指向了 数组长度 也就是是否满了 如果满了 就将tail 重置为0
            if(++tail == array.length)
                tail = 0;
            // 自增 增加 队列中元素个数(记录)
            size++;
            // 由于 队列中 添加了一个元素 队列部位空了 就唤醒 headWaits 中的阻塞线程让其进行取出元素
            headWaits.signal();
            return true;
        }finally
        {
            // 释放锁
            lock.unlock();
        }
    }

    @Override
    public E poll() throws InterruptedException // offer等待线程为空
    {
        // 加锁(阻塞时可以随时打断)
        lock.lockInterruptibly();
        try
        {
            // 判断队列是否为空，如果队列为空就将当前取出元素的线程进行阻塞
            while(isEmpty())
            {
                headWaits.wait();
            }
            // 取出head处的元素
            E e = array[head];
            // 将当前取出的元素位置赋值为null 让GC垃圾回收机制进行处理
            array[head] = null; // help GC
            // 判断head是否指向了 数组长度，如果是那么就重置head
            if(++head == array.length)
                head = 0;
            // 每次取出元素就将 元素个数 自减
            size--;
            // 取出元素后就表示 队列中被取出一个元素 那么此时队列肯定是不满了 那么就唤醒 tailWaits 中的阻塞对象让其进行添加元素
            tailWaits.signal();
            // 返回 取出的元素
            return e;
        }
        finally
        {
            // 释放锁
            lock.unlock();
        }
    }
   
    @Override
    public String toString()
    {
        return "BlockingQueue{" + "array=" + Arrays.toString(array) + '}';
    }
}
```

测试代码：

```java
public class TestBlockingQueue1 {
    public static void main(String[] args) throws InterruptedException {
        BlockingQueue<String> queue = new BlockingQueue<>(3);

        Thread t1 = new Thread(()->{
            try {
                System.out.println(System.currentTimeMillis() + " begin");
                queue.offer("任务1");
                System.out.println(queue);
                queue.offer("任务2");
                System.out.println(queue);
                queue.offer("任务3");
                System.out.println(queue);
                queue.offer("任务4", 5000);
                System.out.println(queue);
                System.out.println(System.currentTimeMillis() + " end");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        },"生产者");
        t1.start();

        Thread.sleep(2000);
        queue.poll();
    }
}
```

打印结果：

```
1705760500582 begin
BlockingQueue{array=[任务1, null, null]}
BlockingQueue{array=[任务1, 任务2, null]}
BlockingQueue{array=[任务1, 任务2, 任务3]}
# 等待2秒后 poll 线程从队列中取出了一个元素 offer(e, timeout) 方法得以执行 添加了 任务4
BlockingQueue{array=[任务4, 任务2, 任务3]}
1705760502589 end
```
