---
layout: post
title:  插入排序
categories: [java,数据结构与算法,算法,排序]
description: 插入排序的详细介绍。
keywords: 算法
---

# 实现动态数组

简单的动态数组类 `DynamicArray`，以下是对其概述思想的介绍：

1. **初始状态和基本参数：**
   -  `size`：表示动态数组的逻辑大小，即当前元素的个数。
   -  `capacity`：表示动态数组的容量，即数组的实际大小。
   -  `array`：是用于存储元素的数组。
2. **添加元素：**
   -  `addLast(int element)` 方法用于向数组的最后位置添加元素。它实际上调用了 `add(int index, int element)` 方法，并将 `index` 参数设置为当前的逻辑大小 `size`。
   -  `add(int index, int element)` 方法用于在指定位置添加元素。在添加之前，会检查是否需要进行数组扩容（通过 `checkAndGrow()` 方法）。如果需要扩容，则创建一个新的数组，并将旧数组中的元素复制到新数组中。然后，通过 `System.arraycopy` 将从指定位置开始的元素整体向后移动一位，腾出位置插入新元素。
3. **数组扩容：**
   -  `checkAndGrow()` 方法用于检查是否需要进行数组扩容。如果当前数组的大小等于容量 `capacity`，则进行扩容。新的容量是当前容量的 1.5 倍，然后创建一个新的数组，将旧数组的元素复制到新数组中。
4. **查询元素：**
   -  `get(int index)` 方法用于查询指定位置的元素。
5. **遍历元素：**
   -  `foreach(Consumer<Integer> consumer)` 方法用于遍历数组的元素，并对每个元素执行指定的操作。
6. **迭代器和流：**
   -  实现了 `Iterator<Integer>` 接口，通过 `iterator()` 方法返回一个迭代器，可以使用迭代器遍历数组元素。
   -  提供了 `stream()` 方法，返回一个 `IntStream`，可以使用流的方式操作数组。
7. **删除元素：**
   -  `remove(int index)` 方法用于删除指定位置的元素。通过 `System.arraycopy` 将从指定位置开始的元素整体向前移动一位，然后逻辑大小 `size` 减一。
8. **示例：**
   -  在 `main` 方法中创建了一个 `DynamicArray` 实例，并进行了一些元素的添加和流式输出。

总体来说，这个动态数组的实现遵循了动态数组的基本思想，通过在必要时进行扩容，支持在任意位置添加和删除元素，同时提供了迭代器和流的方式遍历和操作数组。这种动态数组的实现方式在实际编程中常见，并提供了一种灵活、高效的数据结构

代码：

```java
public class DynamicArray implements Iterable<Integer>
{
    private int size = 0; // 逻辑大小
    private int capacity = 8; // 容量
    private int array[] = {};

    /**
     * 向最后位置[size]添加元素
     * @param element 待添加元素
     */
    public void addLast(int element)
    {
        add(size, element);
    }

    /**
     * 向[0 ... size] 位置添加元素
     * @param index 索引位置
     * @param element 待添加元素
     */
    private void add(int index, int element)
    {
        // 检查是否需要扩容
        checkAndGrow();
        // 如果index 小于等于 0 或 小于 size 就满足条件
        // 如果是 直接调用 add 想要在 某个元素后面添加元素就会满足条件
        if(index >= 0 && index < size)
        {
            // 从 index 索引开始 拷贝 到 index + 1 ，需要的长度为 size - index
            // 从 某个元素的位置开始 整体向后移动一位 下面再对当前索引位置进行赋值并扩大容量
            System.arraycopy(array, index, array, index + 1, size - index);
        }
        // 将值赋值到index处
        array[index] = element;
        // 数组容量扩大
        size++;
    }

    private void checkAndGrow()
    {
        if(size == 0)
        {
            array = new int[capacity];
        }else
        // 判断如果size 等于了 capacity 就进行扩容
        if(size == capacity)
        {
            capacity += capacity >> 1;
            // 创建扩容后的数组
            int newArray[] = new int[capacity];
            // 将旧数组中的元素拷贝到新数组中
            System.arraycopy(array, 0, newArray, 0, size);
            array = newArray;
        }
    }

    /**
     * 查询元素
     * @param index 索引位置，在[0 ... size] 区间内
     * @return 该索引位置的元素
     */
    public int get(int index)
    {
        return array[index];
    }

    public void foreach(Consumer<Integer> consumer)
    {
        for(int i = 0; i < size; i++)
        {
            consumer.accept(array[i]);
        }
    }

    @Override
    public Iterator<Integer> iterator()
    {
        return new Iterator<Integer>()
        {
            int i = 0;
            @Override
            public boolean hasNext() // 有没有下一个元素
            {
                return i < size;
            }

            @Override
            public Integer next() // 返回当前元素, 并移动到下一个元素
            {
                return array[i++];
            }
        };
    }

    public IntStream stream()
    {
        return IntStream.of(Arrays.copyOfRange(array, 0, size));
    }

    public int remove(int index)
    {
        int i = array[index];
        if(index < size - 1)
        {
            System.arraycopy(array, index + 1, array, index, size - index - 1);
        }
        size--;
        return i;
    }

    public static void main(String[] args)
    {
        DynamicArray dynamicArray = new DynamicArray();
        dynamicArray.addLast(1);
        dynamicArray.addLast(2);
        dynamicArray.addLast(3);
        dynamicArray.addLast(4);
        dynamicArray.stream().forEach(e -> {
            System.out.println(e);
        });
    }
}
```

