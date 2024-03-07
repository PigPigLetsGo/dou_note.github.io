---
layout: post
title:  优先级队列-堆实现-大顶堆实现
categories: [java,数据结构与算法,算法,队列]
description: 优先级队列-堆实现-大顶堆实现介绍。
keywords: 算法
---

# 优先级队列-堆实现-大顶堆实现

## 堆实现

堆本质上就是一种树，堆的方式也有很多种，通常使用的是完全二叉树实现。

计算机科学中，堆是一种基于数的数据结构，==通常用完全二叉树实现==。堆的特性如下：

-  在大顶堆中，任意节点C与它的父节点P符合 P.value$$\geq$$ C.value
-  而小顶堆中，任意节点与C与它的父节点P符号P.value $$\leq$$ C.value
-  最顶层的节点 (没有父节点) 称之为 root 根节点

## 那到底什么叫完全二叉树呢？

![image-20240119084035205](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190840270.png)

我们对照上面图进行解释：

首先我们看什么叫 **二叉** ，所谓的 二叉 就是 树中==每个节点 最多 有两个分支==，如下图的 1 号节点 和 3 号节点 都是 有两个 分支的 就是 二叉

![image-20240119084229296](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190842343.png)

## 那什么又叫完全二叉树呢？

==树的将来每一层，除了最后一层，其它层都是填满的==，==当然最后一层 填满它也是 完全二叉树==

根据下图解释：只有 除了 1 根节点除外 ， 2 节点 和 3 节点 有两个 分支 是填满的，而它们下面的 左右子树 也是 各有两个分支 同样 也是填满的 ，最后一层 没有填满 。符合 完全二叉树

![image-20240119084512445](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190845489.png)

## 完全二叉树特点

如果再往树中加一个节点的话，那么必须==靠左添加==，也就是 必须 先将 ==以 左子树 为目标 依次 向 右子树 填满==

比如我要加一个 11 节点的话那么 添加后如下：

![image-20240119084923633](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190849680.png)

**回过头来我们继续看堆** 

## 大顶堆

**大顶堆**：这个树中任意一个节点 跟它的 父节点比，符合一个规则 就是 父节点的值 更大 ，大于子节点的 值

![image-20240119085256025](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190852069.png)

## 小顶堆

**小顶堆**：这个树中任意一个节点 跟它的 父节点比，符合一个规则 就是 父节点的 值 更小，小于子节点的 值

![image-20240119085436036](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190854078.png)

## 特点

==完全二叉树 是 非线性的数据结构，但是我们可以使用数组来存储 节点数据== 

但是这种 完全二叉树 比较特殊，==虽然它是非线性的==数据结构，但是它存储的时候==可以使用线性的数组结构来存储它的数据==。

![image-20240119090049007](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190900064.png)

如比说 上图，完全二叉树是一个 大顶堆，大顶堆的根节点 我们可以存储到 数组的索引 0 的位置，根节点两个孩子 分别可以存储到 索引 1 ，索引 2 的位置，这样一次类推

## 特征

正是因为，堆的数据可以存储在一维的数组里 所以 前人 给我们总结出了 一些 规则，这些规则可以在已知子节点的情况下快速找到父节点，或者反过来 已知父节点的情况下快速找到 子节点 (左子树或者右子树)

-  如果从索引0开始存储节点数据
   -  节点i的父节点为floor((i - 1) / 2) ，当 i > 0 时
      -  当 i > 0时我们才能根据 该 公式 找父节点
   -  节点i的左子节点为2  *  i + 1，右子节点为2  *  i + 2，当然它们得 < size
      -  2 是固定的，i 是 当前索引值，所以是 2 乘以当前 索引 加 1= 左子树。加 2 = 右子树 
      -  根据 父节点 找 子节点 不能超出 数组索引 范围，比如说 下图中 的 节点 3 找 子节点的话就是：2 * 4 + 1 = 9 这显然超出了 数组的 索引范围了，因为 3 节点 没有 子树
-  如果从索引i开始存储节点数据
   -  节点i的父节点为floor(i / 2)，当i > 1 时
   -  节点i的左子节点为 2i ，右子节点为 2i + 1，同样的 < size

上面讲述了 堆呢 它的底层是 完全二叉树，而完全二叉树 显然 是一个 非线性的数据结构，因为树 它都分叉了 不想之前 数组 ，链表 它们都是一条线，所以称之为 ==非线性数据结构==。

### 根据 孩子 找 父节点

floor((索引 - 1) / 2) = 父节点

![image-20240119091032317](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190910408.png)

### 根据 父节点 找 左孩子

2 * 索引 + 1 = 左孩子

![image-20240119091441618](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190914706.png)

### 根据父节点找到 右孩子

2 * 索引 + 2 = 右孩子

![image-20240119091645223](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401190916312.png)

# 大顶堆实现-优先级队列

## 分析添加节点的情况：

假如我要添加一个节点4 那么我该如何添加呢？

如果我直接添加到 节点 3 的下面 那么就 破坏了 大顶堆 的规则了，我们需要对大顶堆进行调整让它重新符合 大顶堆的 特性

![image-20240119101253419](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191012569.png)

### 步骤：

比如说 我们入堆 一个 4 ，它应该在 索引 9 的位置

使用了两个指针 C 代表 孩子， P 代表 父节点

![image-20240119101519869](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191015974.png)

让 C 的优先级 与 P 的优先级 进行比较，C 是 4 ，P 是 3 。孩子 的优先级 比 父节点的高 那么我们应该把 父节点 向下移动 C 指向的位置

![image-20240119101741925](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191017991.png)

然后 将两个指针 向上各移动一位，重复 刚才的过程 进行比较

![image-20240119101844995](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191018086.png)

现在 C 是 4 再跟它的父节点 19 进行比较，现在 父节点 19 比 现在 孩子节点 4 要大了。就不用做调整了

我们就把 4 放入 C 指向的位置

![image-20240119102008781](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191020890.png)

这样就重新符合了 大顶堆的 特性

### 添加节点的情况的代码实现部分：

```java
/**
     * 1. 入堆新元素，加入到数组末尾 (索引位置child)
     * 2. 不断比较新加元素与它父节点 (parent) 优先级
     *    - 如果父节点优先级低，则向下移动，并找到下一个 parent
     *    - 直至父节点优先级更高 或 child = 0 为止
     * @param e
     * @return
     */
@Override
public boolean offer(E e)
{
   // 判断是否 满了
   if(isFull())
      return false;
   // child 最后的 完全二叉树 节点位置 是个空的
   int child = size++;
   // 通过公式 找到 当前 位置的 父节点
   int parent = (child - 1) / 2;
   // 判断 如果 child 不是 根节点或者 当前e 的优先级 高于 父节点的 优先级 满足条件
   while(child > 0 && e.priority() > array[parent].priority())
   {
      // 将 父节点向下移动
      array[child] = array[parent];
      // 将 child 的 索引位置 赋值为 父节点 位置 也就是 向上指向 向上比较
      child = parent;
      // 再将 parent 赋值为 当前索引位置的 父节点位置
      parent = (child - 1) / 2;
   }
   // 最后将 当前要添加的 节点 添加到 合适的位置 就 符合 大顶堆的 特性了
   array[child] = e;
   return true;
}
```

## 分析取出 堆 中 优先级 最高的元素

如下图 优先级最高的 元素 显然是 堆 顶的元素，我们要移除它之后仍然要保证 它还符合 大顶堆的 特性

![image-20240119103337792](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191033848.png)

**第一步**：将要取出的元素跟最后元素交换位置 提高 移除效率

我们将 堆顶的 元素 跟 最后一个元素 进行一个交换

为什么交换？

ps：是为了让它移除的效率更高，因为它本质上还是数组，数组从尾部移除效率最高

![image-20240119103515036](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191035271.png)

那么交换 位置后 将来只需要 size - 1 就可以把它给移除了

![image-20240119103652949](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191036010.png)

**第二步**：重新调整 堆，让它重新符合 大顶堆的 特性

现在经过上面交换位置之后 它就不符合 大顶堆的 特性了。

现在的调整堆 的思路和 入堆的 思路是相反的：入堆是 元素 经过比较 向上浮，然现在 7 要不断向下浮 直到符合堆的 特性

### 步骤：

7 跟 当前 两个孩子进行比较跟 优先级较大的 孩子 交换位置

![image-20240119104210792](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191042850.png)

![image-20240119104243164](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191042218.png)

然后 P 指向当前交换位置的 子孩子位置，L 指向 左孩子，R 指向右孩子 再重复刚才的过程

这回更大的 子孩子是 25 那么就跟R 交换位置

![image-20240119104408330](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191044392.png)

直到 没有孩子了 或者 下次的两个孩子都没有它大 就可以结束 下浮的操作了

![image-20240119104511747](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401191045805.png)

### 取出 堆 中 优先级 最高的元素代码实现部分：

```java
/**
 * 1. 交换堆顶和尾部元素，让尾部元素出队
 * 2. (下滑)
 *    - 从堆顶开始，将父元素与两个孩子较大者交换
 *    - 直到父元素大于两个孩子，或者没有孩子为止
 * @return
 */
@Override
public E poll()
{
   // 判断 堆 是否为空
   if(isEmpty())
      return null;
   // 让 优先级 最高的 也就是 跟节点 跟 最后一个节点进行交换 [为了方便移除，并且效率高点]
   swap(0, size - 1);
   // size-- 大小 减一
   size--;
   // 获取 最后 元素的值
   Priority e = array[size];
   // 将最后元素 赋值为 null 等待 GC 垃圾回收即可
   array[size] = null; // help GC
   // 下潜
   down(0);
   return (E) e;
}

private void down(int parent)
{
   // 通过公式 获取 左孩子 索引
   int left = 2 * parent + 1;
   // int right = 2 * parent + 2;
   int right = left + 1; // 等价于 上面的公式
   // 父节点 为 优先级 最高的 节点
   int max = parent;
   // 判断 在有左孩子的情况下 并且 左孩子的 优先级 比 父节点 优先级高 那么将 父节点 向 左孩子 移动
   // 当然了 max 只是辅助 变量 只是记录了 parent 要跟 谁 进行 交换 ，而 并没有真正 的 赋值过去
   if(left < size && array[left].priority() > array[max].priority())
      max = left;
   // 判断 在有右孩子的情况下 并且 右孩子的 优先级 比 父节点 优先级高 那么将 父节点 向 右孩子 移动
   if(right < size && array[right].priority() > array[max].priority())
      max = right;
   if(max != parent)
   {
      // 将 比 parent 优先级 还高的 孩子节点 进行交换位置
      swap(max, parent);
      // 将 parent 节点 更新为 max 节点 继续下一轮的 比较和交换位置
      down(max);
   }
}

private void swap(int i, int j)
{
   // 典型的 交换位置 写法
   Priority t = array[i];
   array[i] = array[j];
   array[j] = t;
}
```

## 完整实现代码：

priorityQueue

```java
public class PriorityQueue2<E extends Priority> implements Queue<E>
{
    Priority[] array;
    int size;

    // 初始化 实例化 Priority 数组 并 设定 长度为 capacity
    public PriorityQueue2(int capacity)
    {
        array = new Priority[capacity];
    }

    /**
     * 1. 入堆新元素，加入到数组末尾 (索引位置child)
     * 2. 不断比较新加元素与它父节点 (parent) 优先级
     *    - 如果父节点优先级低，则向下移动，并找到下一个 parent
     *    - 直至父节点优先级更高 或 child = 0 为止
     * @param e
     * @return
     */
    @Override
    public boolean offer(E e)
    {
        // 判断是否 满了
        if(isFull())
            return false;
        // child 最后的 完全二叉树 节点位置 是个空的
        int child = size++;
        // 通过公式 找到 当前 位置的 父节点
        int parent = (child - 1) / 2;
        // 判断 如果 child 不是 根节点或者 当前e 的优先级 高于 父节点的 优先级 满足条件
        while(child > 0 && e.priority() > array[parent].priority())
        {
            // 将 父节点向下移动
            array[child] = array[parent];
            // 将 child 的 索引位置 赋值为 父节点 位置 也就是 向上指向 向上比较
            child = parent;
            // 再将 parent 赋值为 当前索引位置的 父节点位置
            parent = (child - 1) / 2;
        }
        // 最后将 当前要添加的 节点 添加到 合适的位置 就 符合 大顶堆的 特性了
        array[child] = e;
        return true;
    }

    /**
     * 1. 交换堆顶和尾部元素，让尾部元素出队
     * 2. (下滑)
     *    - 从堆顶开始，将父元素与两个孩子较大者交换
     *    - 直到父元素大于两个孩子，或者没有孩子为止
     * @return
     */
    @Override
    public E poll()
    {
        // 判断 堆 是否为空
        if(isEmpty())
            return null;
        // 让 优先级 最高的 也就是 跟节点 跟 最后一个节点进行交换 [为了方便移除，并且效率高点]
        swap(0, size - 1);
        // size-- 大小 减一
        size--;
        // 获取 最后 元素的值
        Priority e = array[size];
        // 将最后元素 赋值为 null 等待 GC 垃圾回收即可
        array[size] = null; // help GC
        // 下潜
        down(0);
        return (E) e;
    }

    @Override
    public void remove(int index)
    {
    }

    private void down(int parent)
    {
        // 通过公式 获取 左孩子 索引
        int left = 2 * parent + 1;
        // int right = 2 * parent + 2;
        int right = left + 1; // 等价于 上面的公式
        // 父节点 为 优先级 最高的 节点
        int max = parent;
        // 判断 在有左孩子的情况下 并且 左孩子的 优先级 比 父节点 优先级高 那么将 父节点 向 左孩子 移动
        // 当然了 max 只是辅助 变量 只是记录了 parent 要跟 谁 进行 交换 ，而 并没有真正 的 赋值过去
        if(left < size && array[left].priority() > array[max].priority())
            max = left;
        // 判断 在有右孩子的情况下 并且 右孩子的 优先级 比 父节点 优先级高 那么将 父节点 向 右孩子 移动
        if(right < size && array[right].priority() > array[max].priority())
            max = right;
        if(max != parent)
        {
            // 将 比 parent 优先级 还高的 孩子节点 进行交换位置
            swap(max, parent);
            // 将 parent 节点 更新为 max 节点 继续下一轮的 比较和交换位置
            down(max);
        }
    }

    private void swap(int i, int j)
    {
        // 典型的 交换位置 写法
        Priority t = array[i];
        array[i] = array[j];
        array[j] = t;
    }

    @Override
    public E peek()
    {
        if(isEmpty())
            return null;
        return (E) array[0];
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

```java
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
```

测试结果：√