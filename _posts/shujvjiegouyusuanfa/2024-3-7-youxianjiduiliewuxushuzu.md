---
layout: post
title:  优先级队列-无序数组的实现
categories: [java,数据结构与算法,算法,队列]
description: 优先级队列-无序数组的实现介绍。
keywords: 算法
---

# 优先级队列-无序数组的实现

## 介绍：

### 普通队列

一端进 一端出 ，FIFO ==先进先出==

### 优先级队列

一端进 一端出 ，它会==按照队列中的优先级 出栈==， 优先级高的 先 出栈

## 代码：

写了 三个类 主要是 PriorityQueue 中的逻辑代码

PriorityQueue

```java
public class PriorityQueue<E extends Priority> implements Queue<E>
{
    Priority[] array;
    int size;

    // 初始化 实例化 Priority 数组 并 设定 长度为 capacity
    public PriorityQueue(int capacity)
    {
        array = new Priority[capacity];
    }

    // 添加元素
    @Override
    public boolean offer(E e)
    {
        // 判断 队列中是否为 满了
        if(isFull())
        {
            return false;
        }
        // 向size处添加元素 并将size 自增
        array[size++] = e;
        return true;
    }

    // 返回优先级最高的索引值
    private int selectMax()
    {
        int max = 0;
        for(var i = 1; i < size; i++)
            // 找到 优先级 最高的 索引 赋值给 max 然后返回
            if(array[i].priority() > array[max].priority())
                max = i;
        return max;
    }

    // 取出 优先级 高的 值
    @Override
    public E poll()
    {
        // 判断 队列是否为 空
        if(isEmpty())
            return null;
        // 获取 优先级最高的 索引位置
        int i = selectMax();
        // 通过 i 获取 优先级 最高的 值
        E e = (E) array[i];
        // 根据i删除这个值
        remove(i);
        // 返回 优先级 最高的 值
        return e;
    }

    // 删除 poll 出去的值
    @Override
    public void remove(int index)
    {
        if (index < size - 1)
            // 将 array 的 index + 1 的值开始 拷贝到 array 的 index 处 拷贝 size - index - 1 个值
            System.arraycopy(array, index + 1, array, index, size - index - 1);
        // 移除后 size 自减
        size--;
    }

    // 获取 优先级最高 的值，但是不移除
    @Override
    public E peek()
    {
        // 判断 队列 是否为 空
        if(isEmpty())
            return null;
        // 获取优先级最高的索引
        int i = selectMax();
        // 通过i索引返回优先级最高的值
        return (E) array[i];
    }

    // 查看 队列是否 为 空
    @Override
    public boolean isEmpty()
    {
        // 判断 如果 size 为 0 说明 队列中没有任何值
        return size == 0;
    }

    // 查看 队列是否 满了
    @Override
    public boolean isFull()
    {
        // 判断 size 如果 等于 队列的长度说明 队列已经满了
        return size == array.length;
    }
}
```

Priority

```java
public interface Priority
{
    // 返回对象的优先级，约定数字越大，优先级越高
    // return: 优先级
    int priority();
}
```

Entry

```java
class Entry implements Priority {

    String value;
    int priority;

    public Entry(int priority) {
        this.priority = priority;
    }

    public Entry(String value, int priority) {
        this.value = value;
        this.priority = priority;
    }

    @Override
    public int priority() {
        return priority;
    }

    @Override
    public String toString() {
        return "(" + value + " priority=" + priority + ")";
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Entry entry = (Entry) o;
        return priority == entry.priority;
    }

    @Override
    public int hashCode() {
        return priority;
    }
}
```

## 测试代码：

```java
@Test
public void testPoll()
{
   PriorityQueue<Entry> queue = new PriorityQueue<>(5);
   queue.offer(new Entry("stack1", 4));
   queue.offer(new Entry("stack2", 3));
   queue.offer(new Entry("stack3", 2));
   queue.offer(new Entry("stack4", 5));
   queue.offer(new Entry("stack5", 1));
   assertFalse(queue.offer(new Entry("stack6", 7)));
   assertEquals(5, queue.poll().priority());
   assertEquals(4, queue.poll().priority());
   assertEquals(3, queue.poll().priority());
   assertEquals(2, queue.poll().priority());
   assertEquals(1, queue.poll().priority());
}
```

执行结果： √