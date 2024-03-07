---
layout: post
title:  目前队列存在的问题
categories: [java,数据结构与算法,算法,阻塞队列]
description: 目前队列存在的问题介绍。
keywords: 算法
---

## 目前队列存在的问题

1.  很多场景要求分离==生产者==，==消费者==两个角色，它们得由不同的线程来担当，而之前的实现根本没有考虑线程安全问题
2.  队列为空，那么在之前的实现里会返回null，如果就是硬要拿到一个元素呢？只能不断循环尝试
3.  队列为满，那么再之前的实现里会返回false，如果就是硬要塞入一个元素呢？只能不断循环尝试

所谓的生产者就是一段代码它主要调用的是offer方法，为队列提供元素的这个称之为 ==生产者==

所谓的消费者就是另一段代码它主要调用的是poll方法，从队列中获取元素的这个称之为 ==消费者==

### 案例代码：

```java
public class TestThreadUnsafe
{
    private final String[] array = new String[10];
    private int tail = 0;
    public void offer(String e)
    {
        array[tail] = e;
        tail++;
    }

    @Override
    public String toString()
    {
        return Arrays.toString(array);
    }

    public static void main(String[] args)
    {
        TestThreadUnsafe queue = new TestThreadUnsafe();
        // 如果将 两个添加 代码 换成 在 两个线程中去执行看看会有什么问题
        new Thread(() -> queue.offer("e1"), "t1").start();
        new Thread(() -> queue.offer("e2"), "t2").start();
    }
}
```

### 分析代码：

t1 和 t2 都是要执行如下代码，但是 它们在执行时代码次序 可能会发生 交错的情况

```
tail
[null null null null null null null null null null]
  0    1     2    3    4    5   6    7    8    9
  ============t1 和 t2两个线程的执行次序=============
t1                          t2
array[tail] = e1;
tail++;
                            array[tail] = e2;
                            tail++;
```

上面的话什么意思呢？就是说 当 CPU 刚开始 指向 t1 的时候它先 指向了 赋值语句 array[tail] = e; 但是它还没来得及执行后面的自增操作：tail++; CPU的执行权就会切换到 t2 切换到 t2 它可能会把 t2 这两行代码执行完了 它再切换回 t1 执行 自增操作 tail++;

这就是两个线程代码产生 交错的 情况

```
tail
[null null null null null null null null null null]
  0    1     2    3    4    5   6    7    8    9
  ============t1 和 t2两个线程的执行次序=============
t1                          t2
array[tail] = e1;

                            array[tail] = e2;
                            tail++;
tail++;
```

交错 就会导致我们的结果 出乎意料，导致一个不正确的结果

### 分析交错代码执行流程：

t1 执行中…

```
tail
[null null null null null null null null null null]
  0    1     2    3    4    5   6    7    8    9
  ============t1 和 t2两个线程的执行次序=============
当 t1 将索引0的位置赋值为 e1 的时候 还没来得及执行 tail++ CPU就切换到了 t2
t1                          t2
array[0] = e1;

                            array[tail] = e2;
                            tail++;
tail++;
```

t2执行中…

```
此时由于上面t1执行的时候将 索引0位置赋值为 了e1
tail
[e1 null null null null null null null null null]
  0    1     2    3    4    5   6    7    8    9
  ============t1 和 t2两个线程的执行次序=============
t1                          t2
array[0] = e1;
                            执行t2到赋值语句此时 tail还处于索引0的位置，赋值后 并进行 tail++;
                            array[tail] = e2;
                            tail++;
tail++;
```

t1执行中…

```
由于t1执行了赋值语句为e1时 还没有 tail++ CPU就切换到了t2 而t2赋值为了 e2 并将tail++了
    tail
[e2 null null null null null null null null null]
  0    1     2    3    4    5   6    7    8    9
  ============t1 和 t2两个线程的执行次序=============
t1                          t2
array[0] = e1;
CPU切换到t1 将剩余代码执行完毕 tail++
                            array[tail] = e2;
                            tail++;
tail++;
```

t2执行中…

```
CPU切换到t1 继续执行下面代码tail++
         tail
[e2 null null null null null null null null null]
  0    1     2    3    4    5   6    7    8    9
  ============t1 和 t2两个线程的执行次序=============
t1                          t2
array[0] = e1;
                            CPU 切换到t2 执行赋值代码 并 tail++
                            array[tail] = e2;
                            tail++;
tail++;
```

当现在其实也不必在深究下去了，因为已经可以看出 出错了

当然这种 交错的情况 也只是 有可能而已 ，它也有可能是正常的 比如执行完了 t1 在 执行 t2 但是也有可能 t2 只执行了赋值语句没来得及执行tail++CPU就切换到了t1了