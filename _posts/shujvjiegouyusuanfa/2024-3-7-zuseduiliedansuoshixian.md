---
layout: post
title:  阻塞队列-单锁实现-防止交错执行1
categories: [java,数据结构与算法,算法,阻塞队列]
description: 阻塞队列-单锁实现-防止交错执行1介绍。
keywords: 算法
---

# 阻塞队列-单锁实现-防止交错执行1

在java中要防止 代码 多线程下的 交错执行，我们就需要用到锁

在java中 锁 有两种选择：

1.  一种锁的实现是 sychronized 关键字，功能较少
2.  ReentrantLock 翻译：entrant (进入) Reentrant (重新进入) Lock (锁) ReentrantLock (可重入锁) 。JDK 提供的工具类 而不是 关键字，相对功能比较多

## 代码：

```java
public class TestThreadUnsafe
{
    private final String[] array = new String[11];
    private int tail = 0;
   
    // 锁对象
    ReentrantLock lock = new ReentrantLock();

    public void offer(String e) throws InterruptedException
    {
        // 加锁 阻塞 时 可以随时被 打断
        lock.lockInterruptibly();
        try
        {
            // 需要受到 锁 保护的 代码块
            array[tail] = e;
            tail++;
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
        return Arrays.toString(array);
    }

    public static void main(String[] args)
    {
        TestThreadUnsafe queue = new TestThreadUnsafe();
        new Thread(() ->
        {
            try
            {
                queue.offer("e1");
            }
            catch (InterruptedException e)
            {
                throw new RuntimeException(e);
            }
        }, "t1").start();
        new Thread(() ->
        {
            try
            {
                queue.offer("e2");
            }
            catch (InterruptedException e)
            {
                throw new RuntimeException(e);
            }
        }, "t2").start();
    }
}
```

