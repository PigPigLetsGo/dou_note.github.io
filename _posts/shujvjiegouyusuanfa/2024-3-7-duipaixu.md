---
layout: post
title:  堆排序
categories: [java,数据结构与算法,算法,堆]
description: 堆排序介绍。
keywords: 算法
---

# 堆排序

使用堆 实现排序

### 算法描述：

1.  heapify 建立大顶堆
2.  将堆顶与堆低交换 (最大元素被交换到堆低)，缩小并下潜调整堆
3.  重复第二步直至堆里剩一个元素

### 第一步：heapify 建立大顶堆

如下图的数组元素

![image-20240122161244349](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221612453.png)

我们使用 heapify 进行一个 堆化，现在是大顶堆了，父节点比孩子节点都大

![image-20240122161301201](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221613300.png)

此时 第一步就完成了

### 第二步：将 堆顶元素 跟 堆低元素 交换位置 交换完位置后让堆的 大小 缩小 1 并且 下潜 调整符合 大顶堆的特性

下图就是 将 7 堆顶元素和 1 堆低元素交换位置 然后又 进行下潜 调整为符合 大顶堆的特性后的样子，操作完后 堆的大小 缩小 1 的范围 所以最后的 7 就不算了 ，只看 除了 7 以外 的元素 符合 大顶堆的特性就行了

![image-20240122161724889](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221617009.png)

我们重复这个过程 现在 堆顶是 6 堆低 是 4 。因为不算7 了 它们进行交换位置，并且 进行下潜调整为 符合大顶堆的特性

然后 堆的大小再次缩小 1 此时的 6 也就不算了

![image-20240122162105942](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221621032.png)

一直重复这个过程 直到 堆里仅剩 一个元素就说明 已经排好序了

![image-20240122162241374](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221622466.png)

最后一步 我们就可以退出循环了，此时数组中整个 元素的 顺序就排好了

## 代码实现

编写 大顶堆 类 内部 实现了 heapify 方法用于 堆化 一个 不符合大顶堆特性的数组元素的，内部还有 排序需要用到的  交换位置方法和 重新调整大顶堆的 下浮 方法

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
    public void down(int parent)
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
    public void swap(int i, int j)
    {
        int t = array[i];
        array[i]  = array[j];
        array[j] = t;
    }
}
```

### 排序代码：

```java
@Test
public void test()
{
   // 定义一个数组
   int array[] = {2, 3, 1, 7, 6, 4, 5};
   // 将数组传入 MaxHeap 对象的构造函数中，这个构造函数中 实现了 heapify 方法来对数组进行 堆化处理了
   MaxHeap maxHeap = new MaxHeap(array);
   System.out.println("排序前：" + Arrays.toString(maxHeap.array));
   // 循环到 堆 里 只有一个元素 还没做操作为止
   while(maxHeap.size > 1)
   {
      // 每次都让 堆顶元素 和 堆低元素进行交换位置
      maxHeap.swap(0, maxHeap.size - 1);
      // 每次 交换位置后 让 堆的 大小 缩小 1
      maxHeap.size--;
      // 让 堆顶元素 下潜 找到自己合适的位置 重新符合 大顶堆的特性
      maxHeap.down(0);
   }
   System.out.println("排序后：" + Arrays.toString(maxHeap.array));
}
```

打印结果：

```
排序前：[7, 6, 5, 3, 2, 4, 1]
排序后：[1, 2, 3, 4, 5, 6, 7]
```

