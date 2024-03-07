---
layout: post
title:  堆-heapify2-实现其它方法
categories: [java,数据结构与算法,算法,堆]
description: 堆-heapify2-实现其它方法介绍。
keywords: 算法
---

# 堆-heapify2-实现其它方法

```java
/**
 * @author Dkx
 * @version 1.0
 * @2024/1/2211:08
 * @function
 * @comment
 * 大顶堆
 */
public class MaxHeap
{
    int array[];
    int size;

    public MaxHeap(int capacity)
    {
        this.array = new int[capacity];
    }

    /**
     * 如果实例化对象时传入的是一个普通数组那么我们就需要 进行 建堆 让这个数组符合 大顶堆的 特性
     * @param array
     */
    public MaxHeap(int array[])
    {
        // 将用户传入的 数组 赋值到 成员变量中的数组
        this.array = array;
        // 更新数组的大小
        this.size = array.length;
        // 调用 建堆 方法进行建堆
        heapify();
    }

    // 建堆
    private void heapify()
    {
        // 如何找到最后这个非叶子节点 size / 2 - 1
        for(var i = size / 2; i >= 0; i--)
            down(i);
    }

    /**
     * 删除堆顶元素
     * @return 堆顶元素
     */
    public int poll()
    {
        int top = array[0];
        swap(0, size - 1);
        size--;
        down(0);
        return top;
    }

    /**
     * 删除指定索引处元素
     * @param index 索引
     * @return 被删除元素
     */
    public int poll(int index)
    {
        // 获取 index索引处的元素
        int deleted = array[index];
        // 将 index索引的元素 跟 最后一个元素 进行交换位置
        swap(index, size - 1);
        // 移除最后被取出的元素
        size--;
        // 将index处的元素 下潜到自己 合适的位置
        down(index);
        return deleted;
    }

    /**
     * 获取堆顶元素(不移除元素)
     * @return 堆顶元素
     */
    public int peek()
    {
        return array[0];
    }

    /**
     * 替换堆顶元素
     * @param replaced 新元素
     */
    public void replace(int replaced)
    {
        // 将新元素赋值到 堆顶，但是直接赋值进入就会有可能破坏了 大顶堆的 特性 , 所以需要进行调整
        array[0] = replaced;
        // 跟左右孩子判断 进行 下潜 找到自己 合适的位置即可
        down(0);
    }

    /**
     * 堆的尾部添加元素
     * @param offered 被添加元素值
     * @return 是否添加成功
     */
    public boolean offer(int offered)
    {
          return false;
    }

    // 将inserted 元素上浮：直至offered 小于 父元素或到堆顶
    private void up(int offered)
    {
    }


    // 将parent索引处的元素下潜：与两个孩子较大者交换，直至没孩子或孩子没它大
    private void down(int parent)
    {
        // 通过公式 获取 当前节点的 左孩子
        int left = parent * 2 + 1;
        // 通过上面公式 + 1 获取当前节点的 右孩子
        int right = left + 1;
        // 假设 当前节点 是较大值
        int max = parent;
        // 在左孩子 在合法索引范围内 判断 父节点的值 是否小于 左孩子，如果小就将左孩子的索引值赋值给max记录
        if(left < size && array[max] < array[left])
            max = left;
        // 在右孩子 在合法索引范围内 判断 父节点的值 是否小于 右孩子，如果小就将右孩子的索引值赋值给max记录
        if(right < size && array[max] < array[right])
            max = right;
        // 如果上面两个判断中 有一个满足了 说明 父节点不是最大值
        // 如果max 被更改那么就不等于parent了 说明右需要交换位置的
        if(max != parent)
        {
            // 让 父节点 和 它 左右孩子 值 较大的 那个进行交换位置
            swap(max, parent);
            // 递归 传入max 也就是当前父节点的位置 继续下一轮的判断
            down(max);
        }
    }

    // 交换两个索引处的元素
    private void swap(int i, int j)
    {
        int t = array[i];
        array[i]  = array[j];
        array[j] = t;
    }
}
```

主要讲一下 offer 和 up 方法 其中offer方法从 队列尾部添加元素 为了这个方法实现的 清晰起见 我们将实现分成了两个方法 将来 offer 方法会间接调用 up 方法 也就是将 元素进行上浮的方法

这两篇文章中需要重点 掌握的有 down 下潜 ，up 上浮，还有 heapify 建堆

## 思路：

实现offer 方法 我们先来分析一下 往堆的尾部添加元素的思路：

我们通过公式：(数组长度 - 1) / 2 得到 添加新元素位置的父节点，也就是 完全二叉树 的添加方式 左 到 右

比如说 我们 新加 一个元素 8 ，那么我们根据下图的 堆 计算 这个元素 应该添加到的位置 (数组长度 - 1)  / 2 = 3 

![image-20240122154952661](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221549913.png)

然后让 新元素 跟 父元素进行比较 8 比 4 大 那么它们就需要交换位置 ，把 4 向下移动

![image-20240122155143776](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221551912.png)

然后继续这个过程 让 8 跟 5 比较，结果 5 小就 将 5 向下移动

![image-20240122155239469](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221552593.png)

最后跟7 进行比较 7小 需要向下移动

![image-20240122155314081](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221553190.png)

最后没有更上层的元素了那么 8  元素就被交换到了 堆顶位置，这样一个过程就是我们 提到的 上浮 过程

## 完整实现代码如下：

```java
/**
 * @author Dkx
 * @version 1.0
 * @2024/1/2211:08
 * @function
 * @comment
 * 大顶堆
 */
public class MaxHeap
{
    int array[];
    int size;

    public MaxHeap(int capacity)
    {
        this.array = new int[capacity];
    }

    /**
     * 如果实例化对象时传入的是一个普通数组那么我们就需要 进行 建堆 让这个数组符合 大顶堆的 特性
     * @param array
     */
    public MaxHeap(int array[])
    {
        // 将用户传入的 数组 赋值到 成员变量中的数组
        this.array = array;
        // 更新数组的大小
        this.size = array.length;
        // 调用 建堆 方法进行建堆
        heapify();
    }

    // 建堆
    private void heapify()
    {
        // 如何找到最后这个非叶子节点 size / 2 - 1
        for(var i = size / 2; i >= 0; i--)
            down(i);
    }

    /**
     * 删除堆顶元素
     * @return 堆顶元素
     */
    public int poll()
    {
        int top = array[0];
        swap(0, size - 1);
        size--;
        down(0);
        return top;
    }

    /**
     * 删除指定索引处元素
     * @param index 索引
     * @return 被删除元素
     */
    public int poll(int index)
    {
        // 获取 index索引处的元素
        int deleted = array[index];
        // 将 index索引的元素 跟 最后一个元素 进行交换位置
        swap(index, size - 1);
        // 移除最后被取出的元素
        size--;
        // 将index处的元素 下潜到自己 合适的位置
        down(index);
        return deleted;
    }

    /**
     * 获取堆顶元素(不移除元素)
     * @return 堆顶元素
     */
    public int peek()
    {
        return array[0];
    }

    /**
     * 替换堆顶元素
     * @param replaced 新元素
     */
    public void replace(int replaced)
    {
        // 将新元素赋值到 堆顶，但是直接赋值进入就会有可能破坏了 大顶堆的 特性 , 所以需要进行调整
        array[0] = replaced;
        // 跟左右孩子判断 进行 下潜 找到自己 合适的位置即可
        down(0);
    }

    /**
     * 堆的尾部添加元素
     * @param offered 被添加元素值
     * @return 是否添加成功
     */
    public boolean offer(int offered)
    {
        // 判断 队列 是否 满了
        if(size == array.length)
            return false;
        // 调用up 上浮 方法 添加元素
        up(offered);
        // 增加元素个数
        size++;
        return true;
    }

    // 将inserted 元素上浮：直至offered 小于 父元素或到堆顶
    private void up(int offered)
    {
        // child 是最后 位置的指针 每次 判断 比 新元素小了 就需要 将child 位置的父节点 向下移动
        // 再让child 赋值为 父节点位置 继续 上上层的 比较
        int child = size;
        // 判断 只要 child 没有走到 堆顶 那么就 继续 比较 父节点的值
        while(child > 0)
        {
            // 通过公式 child - 1 / 2 获取当前child位置的父节点 索引
            int parent = (child - 1) / 2;
            // 通过 parent索引位置的值 比较 当前offered 值
            if(offered > array[parent])
                // 如果 新元素的值 比 父节点的值 大，那么就需要 将 父节点的值 向下移动 移动到child处
                array[child] = array[parent];
            else
                // 如果不大 那么就 跳出 直接 加入即可
                break;
            // 将child 位置 赋值为 parent 位置 继续向上做比较
            child = parent;
        }
        // 如果 上面循环停止了 说明 offered找到了它合适的位置 进行赋值即可
        array[child] = offered;
    }


    // 将parent索引处的元素下潜：与两个孩子较大者交换，直至没孩子或孩子没它大
    private void down(int parent)
    {
        // 通过公式 获取 当前节点的 左孩子
        int left = parent * 2 + 1;
        // 通过上面公式 + 1 获取当前节点的 右孩子
        int right = left + 1;
        // 假设 当前节点 是较大值
        int max = parent;
        // 在左孩子 在合法索引范围内 判断 父节点的值 是否小于 左孩子，如果小就将左孩子的索引值赋值给max记录
        if(left < size && array[max] < array[left])
            max = left;
        // 在右孩子 在合法索引范围内 判断 父节点的值 是否小于 右孩子，如果小就将右孩子的索引值赋值给max记录
        if(right < size && array[max] < array[right])
            max = right;
        // 如果上面两个判断中 有一个满足了 说明 父节点不是最大值
        // 如果max 被更改那么就不等于parent了 说明右需要交换位置的
        if(max != parent)
        {
            // 让 父节点 和 它 左右孩子 值 较大的 那个进行交换位置
            swap(max, parent);
            // 递归 传入max 也就是当前父节点的位置 继续下一轮的判断
            down(max);
        }
    }

    // 交换两个索引处的元素
    private void swap(int i, int j)
    {
        int t = array[i];
        array[i]  = array[j];
        array[j] = t;
    }
}
```

