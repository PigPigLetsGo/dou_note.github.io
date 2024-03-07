---
layout: post
title:  优先级队列-有序数组实现
categories: [java,数据结构与算法,算法,队列]
description: 优先级队列-有序数组介绍。
keywords: 算法
---

# 优先级队列-有序数组实现

见名知意，它是 将 优先级 越高的 元素 越往 队列顶部 排放，最高优先级的元素 就在 队列 的顶部。

![image-20240118183704215](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401181837284.png)

当出队列时就是将队列顶部的元素(优先级最高的元素)出队列

![image-20240118183825616](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401181838651.png)

出队 变得简单了，但是 入队就相对于比较复杂了，比如说 我们要 入队一个3 那么就需要 跟 队列中的 元素进行比较 然后找到自己适合的位置 入队就行了。

步骤如下：

1 需要先将 数组的 容量 进行扩增

![image-20240118184017914](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401181840942.png)

2 让 入栈元素的 优先级 跟 i 指针指向的元素的优先级 进行对比，8比3大 所以 将 8 向上移动一位

![image-20240118184143588](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401181841620.png)

3 然后 i 指向下一个元素 再进行比较，5 也比 3 大 所以 5 也要向上移动一位

![image-20240118184234332](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401181842364.png)

4 然后 i  指向下一个元素，4 跟 3 比，4 大，所以 4 向上移动一位

![image-20240118184317037](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401181843074.png)

5 最后 1 跟 3 比较，3 大。此时 1 就不用移动了 那么就将 3 插入到 指向的索引 1 的位置

![image-20240118184410512](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401181844545.png)

代码：

PriorityQueue1

```java
public class PriorityQueue1<E extends Priority> implements Queue<E>
{
    Priority[] array;
    int size;

    // 初始化 实例化 Priority 数组 并 设定 长度为 capacity
    public PriorityQueue1(int capacity)
    {
        array = new Priority[capacity];
    }

    // 添加元素
    @Override
    public boolean offer(E e)
    {
        if(isFull())
            return false;
        insert(e);
        // 增加容量
        size++;
        return true;
    }

    // 找到 合适的位置 并 插入 元素
    private void insert(E e)
    {
        int i = size - 1;
        // 如果 i 没有 到最后一个 和 i 指向的 元素的优先级 大于 e 元素优先级的话 就满足条件了
        while(i >= 0 && array[i].priority() > e.priority())
        {
            // 将 比 e 优先级高 的 元素 向后移动一位
            array[i + 1] = array[i];
            // 自减，继续比较下一个
            i--;
        }
        // 如果循环停止了说明找到了 e 合适的位置了  那么就将 e 插入到 i + 1 的位置
        array[i + 1] = e;
    }

    // 取出 优先级 高的 值
    @Override
    public E poll()
    {
        if (isEmpty())
        {
            return null;
        }
        E e = (E) array[size - 1];
        array[--size] = null; // help GC
        return e;
    }

    // 删除 poll 出去的值
    @Override
    public void remove(int index)
    {
        if (index < size - 1)
            System.arraycopy(array, index + 1, array, index, size - index - 1);
        size--;
    }

    // 获取 优先级最高 的值，但是不移除
    @Override
    public E peek()
    {
        if (isEmpty())
        {
            return null;
        }
        return (E) array[size - 1];
    }

    // 查看 队列是否 为 空
    @Override
    public boolean isEmpty()
    {
        return size == 0;
    }

    // 查看 队列是否 满了
    @Override
    public boolean isFull()
    {
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

测试代码：

```
public class TestDemo
{
    @Test
    public void testPoll()
    {
        PriorityQueue1<Entry> queue = new PriorityQueue1<>(5);
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
}
```

打印结果：√