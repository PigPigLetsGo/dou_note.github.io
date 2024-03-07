---
layout: post
title:  堆-heapify1
categories: [java,数据结构与算法,算法,堆]
description: 堆-heapify1介绍。
keywords: 算法
---

# 堆-heapify1

我们看一个堆里面相对比较重要的方法叫 heapify 翻译过来叫 (建堆，堆化)

这是什么意思呢？比如说我想做一个大顶堆 ，给了一个 初始的数组 

![image-20240122094815460](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401220948559.png)

但是上面这个数组并不符合 大顶堆 的定义，因为大顶堆 要求 任意一个父节点它的值要比 两个孩子节点大

我们要把上面这个数组 调整成 符合大顶堆的 定义 这个过程我们就叫做 heapify (建堆)

## 思路：

比如说准备了一个 空堆 我们向这个 空堆中添加元素， 如比我们先添加一个元素 2，如下图：

![image-20240122095314994](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401220953030.png)

如果我们再加入一个元素1 的话 那么跟之前一样 先跟父元素比较，如果比 父元素小那么就 符合条件 加入到下面即可

![image-20240122095425035](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401220954069.png)

如果我再加入一个 元素 3 呢，这个 3 它比 父元素大， 这时 我们就需要将 父元素向下调整

![image-20240122095510901](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401220955941.png)

![image-20240122095539700](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401220955738.png)

调整下来后 再跟它 上上层的父元素进行比较，而此时并没有 上上层元素了 那么原本 p 指向的位置就是我们新加入元素 正确的位置

![image-20240122095711050](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401220957083.png)

那么把元素 3 添加到 该位置就可以了

## Floyd 建堆算法作者 (也是之前龟兔赛跑判环作者)

### 算法描述:

1.  找到最后一个非叶子节点 (什么是非叶子节点呢？就是 有左右孩子的就是 非叶子节点，叶子节点就是 没有左右孩子的 就跟树上的叶子一样)
    1.  找到 最后 非叶子节点的公式：堆的大小 / 2 - 1 = 最后一个非叶子节点 。==注意==：适用于 数组索引从 0 为起点的
2.  从后向前，对每个节点执行下潜

第一步找到非叶子节点那么在如下图中的非叶子节点就是 2, 3, 1 它要找的是 最后一个 非叶子节点 那么就是 3

第二步 从后向前 也就是按 3， 2,  1 这个顺序从后向前然后为这每个节点执行下潜就可以了

![image-20240122103821313](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221038357.png)

### 演示过程：

第一步 ，我们在已经并不符合大顶堆特性的数组中找到最后一个非叶子节点 那么就是 3

![image-20240122104253273](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221042319.png)

第二步，执行下潜 ，3 这个元素 跟它 两个较大的 孩子 进行交换，在这个例子中 7 更大 那么就 跟3交换位置 (下潜)

![image-20240122104459279](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221044329.png)

第三步，交换完之后 3 就没有 更多的 左右孩子了 所以交换就停止了，这个下潜 也就完成了

重复这个过程找到下一个 最后的 非叶子节点 也就是 2 

![image-20240122104700311](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221047364.png)

2 跟左右孩子进行比较 跟 较大的孩子进行交换位置 (下潜)

![image-20240122104733738](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221047786.png)

2 节点没有更多的做优孩子 下潜就停止了

最后1 进行 跟左右孩子 比较 7 更大，跟 7 交换位置 (下潜)

![image-20240122104852653](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221048705.png)

但是 1 还有更多的左右孩子 ，那么继续跟 左右孩子 比较 较大的 然后交换位置 (继续下潜)

![image-20240122104947216](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221049263.png)

1 潜到底就停止

到现在为止 大顶堆 就调整好了

## 代码实现：

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
