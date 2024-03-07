---
layout: post
title:  优先级队列-无序数组的实现
categories: [java,数据结构与算法,算法,队列]
description: 优先级队列-无序数组的实现介绍。
keywords: 算法
---

# 优先级队列-堆实现-小顶堆实现

如下有三个链表 分别 使用指针p指向 头部

```
p
1 -> 4 -> 5 -> null
p
1 -> 3 -> 4 -> null
p
2 -> 6 -> null
```

我们分别把 各个链表 的头部 元素 放到 小顶堆中

```
p
1 -> 4 -> 5 -> null
p
1 -> 3 -> 4 -> null
p
2 -> 6 -> null
# 先将 三个链表的头部元素 放到 小顶堆中，由于 1 和 1 相同所以不用 交换位置
小顶堆  1 1 2
空链表
# 移除 小顶堆的 堆顶元素 (最小的元素) 放到 链表中
小顶堆  1 2
空链表  1
```

将第一个链表的 p 指针向后移动一步，将当前指向 的 元素放到 小顶堆中

```
     p
1 -> 4 -> 5 -> null
p
1 -> 3 -> 4 -> null
p
2 -> 6 -> null
# 将 小顶堆的 堆顶元素 (最小的元素) 放到 链表中
小顶堆  2 4
空链表  1 -> 1
```

将第二个链表的 p 指针向后移动一步，将当前指向的 元素放到 小顶堆中

```
     p
1 -> 4 -> 5 -> null
     p
1 -> 3 -> 4 -> null
p
2 -> 6 -> null
# 将 小顶堆的 堆顶元素 (最小的元素) 放到 链表中
小顶堆  4  3
空链表  1 -> 1 -> 2
```

将第三个链表的 p 指针向后移动一步，将当前指向的 元素放到 小顶堆中

```
     p
1 -> 4 -> 5 -> null
     p
1 -> 3 -> 4 -> null
     p
2 -> 6 -> null
# 将 小顶堆的 堆顶元素 (最小的元素) 放到 链表中
小顶堆  4  6
空链表  1 -> 1 -> 2 -> 3
```

小顶堆中 3 被移除了 那么就将 第二个链表的 p 指针向后移动一步，将当前指向的 元素  放到小顶堆中

```
     p
1 -> 4 -> 5 -> null
          p
1 -> 3 -> 4 -> null
     p
2 -> 6 -> null
# 将 小顶堆的 堆顶元素 (最小的元素) 放到 链表中 被移除的 4 是 从第一个链表中 取出的，因为这次也是4 并不交换位置
小顶堆  6  4
空链表  1 -> 1 -> 2 -> 3 -> 4
```

小顶堆中第一个链表的 4 被移除了 那么就将 第一个链表的 p 指针向后移动一步，将当前指向的 元素  放到小顶堆中

```
          p
1 -> 4 -> 5 -> null
          p
1 -> 3 -> 4 -> null
     p
2 -> 6 -> null
# 将 小顶堆的 堆顶元素 (最小的元素) 放到 链表中
小顶堆  6  5
空链表  1 -> 1 -> 2 -> 3 -> 4 -> 4
```

现在 三个链表的 p 指针 都指向了null那么就将小顶堆中 剩余的 按照 小的开始放到链表中

```
# 将 小顶堆的 堆顶元素 (最小的元素) 放到 链表中
小顶堆 
空链表  1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6
```

### 演示图：

![recording](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191524365.gif)

会了大顶堆 在看 小顶堆 代码就会很轻松

## 代码：

MinHeap

```java
// 小顶堆
public class MinHeap
{
    ListNode[] array;
    int size;

    public MinHeap(int capacity)
    {
        array = new ListNode[capacity];
    }

    public boolean offer(ListNode e)
    {
        // 判断堆中是否 满了
        if(isFull())
            return false;
        // 获取 最后一个元素
        int child = size++;
        // 通过公式 得出 当前元素的 父节点
        int parent = (child - 1) / 2;
        // 如果 不是 root 节点 和 当前的元素值 小于 父节点的元素值 就满足条件
        while(child > 0 && e.val < array[parent].val)
        {
            // 将 父节点的 元素值 赋值给 当前元素
            array[child] = array[parent];
            // 将 父节点的索引位置 赋值到 当前索引
            child = parent;
            // 再次计算 当前 节点的 父节点的索引位置
            parent = (child - 1) / 2;
        }
        // 将 当前的 元素赋值 到 当前节点中
        array[child] = e;
        return true;
    }

    public ListNode poll()
    {
        // 判断 堆中是否为空
        if(isEmpty())
            return null;
        // 将 root 节点 与 最后一个节点 交换位置 ，方便移除 提高效率
        swap(0, size - 1);
        // 大小 减一
        size--;
        // 拿到 最后 的值
        ListNode e = array[size];
        // 将最后的 值赋值为 null 交给 GC 垃圾回收处理
        array[size] = null;
        // 将 当前的 root 节点 向下比较 找到自己合适的位置
        down(0);
        return e;
    }

    private void swap(int i, int j)
    {
        // 经典的位置交换
        ListNode t = array[i];
        array[i] = array[j];
        array[j] = t;
    }

    private void down(int parent)
    {
        // 通过公式 获取 当前节点的 左孩子
        int left = 2 * parent + 1;
        // 通过公式 获取 当前节点的 右孩子
        int right = left + 1;
        // 将当前 节点 假定为 最小的 元素 赋值给 min
        int min = parent;
        // 判断 如果 left 没有超出 索引范围 并且 left 所在的 值 比 min 所在的值 还要小
        if(left < size && array[left].val < array[min].val)
            // 就将 left 索引赋值给min
            min = left;
        // 与上面判断同理
        if(right < size && array[right].val < array[min].val)
            min = right;
        // 判断如果min不等于parent说明上面是有符合交换条件的 就执行if方法体
        if(min != parent)
        {
            // 交换位置
            swap(min, parent);
            // 将当前的min 进行递归调用 重复这个 工作 直到 该 值 找到自己合适的位置即可
            down(min);
        }
    }

    public boolean isEmpty()
    {
        return size == 0;
    }

    public boolean isFull()
    {
        return size == array.length;
    }
}
```

ListNode

```java
/**
 * Leetcode 很多链表题目用到的节点类
 */
public class ListNode {
    public int val;
    public ListNode next;

    public ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder(64);
        sb.append("[");
        ListNode p = this;
        while (p != null) {
            sb.append(p.val);
            if (p.next != null) {
                sb.append(",");
            }
            p = p.next;
        }
        sb.append("]");
        return sb.toString();
//        return String.valueOf(this.val);
    }

    public static ListNode of(int... elements) {
        if (elements.length == 0) {
            return null;
        }
        ListNode p = null;
        for (int i = elements.length - 1; i >= 0; i--) {
            p = new ListNode(elements[i], p);
        }
        return p;
    }
}
```

测试代码：

```java
public class DemoTest
{
    // 合并多个有序链表
    private ListNode merge(ListNode[] list)
    {
        MinHeap heap = new MinHeap(list.length);
        // 1. 将链表的头节点 添加到 小顶堆中
        for (ListNode h : list)
        {
            if(h != null)
            {
                heap.offer(h);
            }
        }
        // 2. 不断从堆顶移除最小元素，加入新链表
        ListNode s = new ListNode(-1, null);
        // 定义辅助变量
        ListNode t = s;
        // 判断如果 heap 对象不为空
        while(! heap.isEmpty())
        {
            // 从heap 堆中 弹出 顶部 元素
            ListNode min = heap.poll();
            // 将 min 节点 添加到 t 链表中
            t.next = min;
            // 更新t 节点
            t = min;
            if(min.next != null)
                // 将 上面poll 处的min 的next节点 在 添加到 heap 对象中这样就起到了 向后指向的 作用
                heap.offer(min.next);
        }
        // s.next返回 除了 哨兵节点外的 其它元素
        return s.next;
    }

    public static void main(String[] args)
    {
        ListNode[] list = {
                ListNode.of(1, 4, 5),
                ListNode.of(1, 3, 5),
                ListNode.of(2, 6),
                null,
        };
        System.out.println(new DemoTest().merge(list));
    }
}
```

运行结果：

```
[1,1,2,3,4,5,5,6]
```

