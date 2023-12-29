---
layout: post
title: 平衡二叉树
categories: [java,数据结构与算法,算法，数据结构]
description: 平衡二叉树的介绍和实现
keywords: 算法
---

### 平衡二叉树(AVL树)

看一个案例(==说明二叉排序树可能的问题==)

给你一个数列 {1,2,3,4,5,6}，要求创建一颗二叉排序树(BST)，并分析问题所在。

![image-20230709181533895](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230709181533895.png)

**左边BST存在的问题分析**：

1.  ==左子树全部为空==，从形式上看，更像一个==单链表==。
2.  ==插入速度没有影响==。
3.  ==查询速度明显降低==(因为==需要依次比较==)，不能发挥BST的优势，因为==每次还需要比较左子树==，其==查询速度比单链表还慢== 
4.  解决方案——**平衡二叉树(AVL)** 

**基本介绍** 

1.  **平衡二叉树**也叫==平衡二叉搜索树==(**Self-balancing binary search tree**) 又被称为 ==AVL树==，可以==保证查询效率较高== 
2.  具有以下**特点**：它是一颗==空树==或它的==左右两个子树的高度差<strong style="color:red">^只看高度不能看左右节点^</strong>的绝对值不超过1并且左右两个子树都是一颗平衡二叉树==。平衡二叉树的常用实现方法有==红黑树==，==AVL<strong style="color:red">^指定是算法而不是树^</strong>==，==替罪羊树==，==Treap==，==伸展树等==。
3.  举例说明，看看下面哪些是AVL树，为什么？

​							**图一**									  				**图二**								 								**图三**

![image-20230710150129892](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230710150129892.png)

**分析**：

图一：左子树高度：2，右子树高度：1 。差值：1

图二：左子树高度：2，右子树高度：2 。差值：0

图三：左子树高度：3，右子树高度：1 。差值：2

![image-20230710151042079](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230710151042079.png)

#### 单旋转(左旋转)

如果是左旋转就属于是单旋转

**要求**：给你一个数列，创建出对应的平衡二叉树，数列 {4,3,6,5,7,8}

**左旋转的目的**：降低右子树的高度

**思路分析(示意图)** 

下图红线部分是调整流程。

![image-20230710153838284](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230710153838284.png)

按照规则调整完成之后，形成了下面这样一棵树

![image-20230710153954253](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230710153954253.png)

完整流程如下图所示：

![image-20230710154031528](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230710154031528.png)

插入8时，发现左右子树高度相差大于1，则进行左旋转；

1.  创建一个新的节点`newNode`，值等于当前 **根节点** 的值(以 4 创建)
2.  把新节点的 左子树 设置为当前节点的 **左子树** 

```java
newNode.left = left;
```

3.  把新节点的 **右子树** 设置为==当前节点== ==右子树==的==左子树== 

```java
newNode.right = right.left;
```

4.  把 ==当前节点== 的==值换为== ==右子节点 的值== 

```java
value = right.value;
```

5.  把 ==当前节点== 的==右子树设置为== ==右子树的右子树== 

```java
right = right.right;
```

6.  把 ==当前节点== 的==左子树设置为新节点== 

```java
left = newNode;
```

<strong style="color:red">注</strong>：图左边是调整期，右边是调整后。注意调整期的 6 那个节点，**调整之后，没有节点指向它了**。也就是说，遍历的时候它是不可达的。那么将会自动的被垃圾回收掉。

##### 树高度计算

前面说过，平衡二叉树是为了解决二叉排序树中可能出现的查找效率问题，那么基本上的代码都可以在之前的二叉排序树上进行优化。那么下面只给出当前主题相关的代码，最后放出一份完整的代码。

树的高度计算，我们需要得到 3 个高度：

1.  这棵树的整体高度
2.  左子树的高度
3.  右子树的高度

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1015:49
 * @comment
 */
@SuppressWarnings("all")
public class AVLTreeDemo {
    public static void main(String[] args) {
        AVLTree tree = new AVLTree();
        int[] arr = {4, 3, 6, 5, 7, 8};
        for (int i = 0; i < arr.length; i++)
            tree.add(new Node(arr[i]));
        tree.infixOrder();
        System.out.printf("树高度：%d\n", tree.root.height());//4
        System.out.printf("左树高度：%d\n", tree.root.leftHeight());//1
        System.out.printf("右数高度：%d\n", tree.root.rightHeight());//3
    }
}

class AVLTree {
    public Node root;

    /**
     * @return 获取root节点
     */
    public Node getRoot() {
        return root;
    }

    /**
     * 添加节点
     *
     * @param node 传入要添加的节点
     */
    public void add(Node node) {
        if (root == null)
            root = node;
        else
            root.add(node);
    }

    /**
     * 中序遍历排序二叉树
     */
    public void infixOrder() {
        if (root == null)
            return;
        else
            root.infixOrder();
    }
}

@SuppressWarnings("all")
class Node {
    //节点的值
    public int value;
    //左子节点
    public Node left;
    //右子节点
    public Node right;

    //构造器初始化节点的值
    public Node(int value) {
        this.value = value;
    }

    //返回左子树的高度
    public int leftHeight() {
        if (left == null)
            return 0;
        else
            return left.height();
    }

    //返回右子树的高度
    public int rightHeight() {
        if (right == null)
            return 0;
        else
            return right.height();
    }

    //返回当前节点的高度,以当前节点为跟节点的树的高度
    public int height() {
        /**
         * 这里使用了递归：返回了左右子树，最高的那一个数值。
         * 递归原理： 第一个开始统计的时候，一定是一个叶子节点。
         *          根据这个逻辑：叶子节点的Math.max(0,0) = 0
         *          因为当前节点算一层，所以 +1;
         *          返回到上一层的时候，至少是这样：Math.max(1,0) = 1
         *          然后把自己本身的层+1.以此类推，返回到根节点的时候，就拿到了从包含根节点的树的高度
         */
        //+1；因为本身节点也要算上
        return Math.max(left == null ? 0 : left.height(),
                right == null ? 0 : right.height()) + 1;
    }

    /**
     * 添加节点
     *
     * @param node 传入要添加的节点进行添加
     */
    public void add(Node node) {
        if (node == null)
            return;
        if (node.value < this.value)
            if (this.left == null)
                this.left = node;
            else
                this.left.add(node);
        else if (this.right == null)
            this.right = node;
        else
            this.right.add(node);
    }

    /**
     * 中序遍历排序二叉树
     */
    public void infixOrder() {
        if (this.left != null)
            this.left.infixOrder();
        System.out.println(this);
        if (this.right != null)
            this.right.infixOrder();
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
}
```

##### 旋转

**说下旋转的时机**：也就是什么时候采取做旋转的操作

**当然是**：当 ==右子树高度 - 左子树高度 > 1== 时，才执行左旋转。

这里就得到一些信息：

1.  每次添加完一个节点后，就需要检查树的高度

2.  满足 ==右子树高度 - 左子树高度 > 1==，那么一定满足下面的条件：

    1.  左子树高度为1

    2.  右子树高度为3

        也就是符合以下这张图：

    ![image-20230711110051128](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711110051128.png)

    也就是有如上的信息逻辑，在实现旋转的时候，只要按照思路分析写就可以了，不需要进行边界判断了。

3.  代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1015:49
 * @comment
 */
@SuppressWarnings("all")
public class AVLTreeDemo {
    public static void main(String[] args) {
        AVLTree tree = new AVLTree();
        int[] arr = {4, 3, 6, 5, 7, 8};
        for (int i = 0; i < arr.length; i++)
            tree.add(new Node(arr[i]));
        tree.infixOrder();
        System.out.printf("树高度：%d\n", tree.root.height());//3
        System.out.printf("左树高度：%d\n", tree.root.leftHeight());//2
        System.out.printf("右数高度：%d\n", tree.root.rightHeight());//2
        System.out.println("当前Root节点为: "+tree.getRoot());//6
    }
}

class AVLTree {
    public Node root;

    /**
     * @return 获取root节点
     */
    public Node getRoot() {
        return root;
    }

    /**
     * 添加节点
     *
     * @param node 传入要添加的节点
     */
    public void add(Node node) {
        if (root == null)
            root = node;
        else
            root.add(node);
    }

    /**
     * 中序遍历排序二叉树
     */
    public void infixOrder() {
        if (root == null)
            return;
        else
            root.infixOrder();
    }
}

@SuppressWarnings("all")
class Node {
    //节点的值
    public int value;
    //左子节点
    public Node left;
    //右子节点
    public Node right;

    //构造器初始化节点的值
    public Node(int value) {
        this.value = value;
    }

    //返回左子树的高度
    public int leftHeight() {
        if (left == null)
            return 0;
        else
            return left.height();
    }

    //返回右子树的高度
    public int rightHeight() {
        if (right == null)
            return 0;
        else
            return right.height();
    }

    //返回当前节点的高度,以当前节点为跟节点的树的高度
    public int height() {
        /**
         * 这里使用了递归：返回了左右子树，最高的那一个数值。
         * 递归原理： 第一个开始统计的时候，一定是一个叶子节点。
         *          根据这个逻辑：叶子节点的Math.max(0,0) = 0
         *          因为当前节点算一层，所以 +1;
         *          返回到上一层的时候，至少是这样：Math.max(1,0) = 1
         *          然后把自己本身的层+1.以此类推，返回到根节点的时候，就拿到了从包含根节点的树的高度
         */
        //+1；因为本身节点也要算上
        return Math.max(left == null ? 0 : left.height(),
                right == null ? 0 : right.height()) + 1;
    }

    /**
     * 创建左旋转方法
     */
    public void leftRotate() {
        //1.创建一个新的节点newNode(以4这个值创建),创建一个新的节点，值等于当前根节点的值
        Node newNode = new Node(value);
        //2.把新节点的左子树设置为当前节点的左子树
        newNode.left = this.left;
        //3.把新节点的右子树设置为当前节点的右子树的左子树
        newNode.right = this.right.left;
        //4.把当前节点的值换为右子节点的值
        this.value = this.right.value;
        //5.把当前节点的右子树设置为右子树的右子树
        this.right = this.right.right;
        //6.把当前节点的左子树设置为新节点
        this.left = newNode;
    }

    /**
     * 添加节点
     *
     * @param node 传入要添加的节点进行添加
     */
    public void add(Node node) {
        if (node == null)
            return;
        if (node.value < this.value)
            if (this.left == null)
                this.left = node;
            else
                this.left.add(node);
        else if (this.right == null)
            this.right = node;
        else
            this.right.add(node);

        /**
         * 旋转的时候有以下规则
         * 没添加一个节点后：检查树的高度是否平衡
         *         如果右子树高度 - 左子树高度 > 1 ，则左旋转
         *         也就是说：每次旋转的层只涉及到4层(对照笔记上的图示理解)
         */
        if (rightHeight() - leftHeight() > 1)
            leftRotate();
    }

    /**
     * 中序遍历排序二叉树
     */
    public void infixOrder() {
        if (this.left != null)
            this.left.infixOrder();
        System.out.println(this);
        if (this.right != null)
            this.right.infixOrder();
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
}
```

运行结果：

```
Node{value=3}
Node{value=4}
Node{value=5}
Node{value=6}
Node{value=7}
Node{value=8}
树高度：3
左树高度：2
右数高度：2
当前Root节点为: Node{value=6}
```

看完代码之后，它的旋转其实就是，将root节点，往下沉到了 root.right 节点下面。

![image-20230711110535115](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711110535115.png)

看着上图，是否有想过，貌似根本就可以不用前面讲解的6个步骤来旋转：

1.  不用创建新节点
2.  直接将node节点下沉
3.  更改node的right节点为right.left
4.  更改right.left = node

其实就已经完成了旋转。但是你仔细想一想，旋转逻辑是写在node里面的，avlTree中的引用如何改变？除非把旋转逻辑移动到avlTree中去，就可以省略掉新节点的步骤来完成。

#### 右旋转

-  左旋转：右 - 左 > 1 ，把右边的往左边旋转一层
-  右旋转：左 - 右 > 1 ，把左边的往右边旋转一层

它们是反着来得思路如下：

1.  创建一个新的节点newNode，值等于当前 **根节点** 的值(以 10 创建)
2.  把新节点的  **右子树**  设置为当前节点的 **右子树** 

```java
newNode.right = this.right;
```

3.  把新节点的  **左子树**  设置为当前节点的  **左子树的右子树** 

```java
newNode.left = this.left.right;
```

4.  把 **当前节点** 的**值**换为  **左子节点**  的值

```java
this.value = this.left.value;
```

5.  把  **当前节点** 的**左子树**设置为 **左子树的左子树** 

```java
this.left = this.left.left;
```

6.  把 **当前节点** 的**右子树**设置为**新节点** 

```java
this.right = newNode;
```

上述步骤就是对应下图的描述：查看图示更清楚

![image-20230711114545279](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711114545279.png)

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1015:49
 * @comment
 */
@SuppressWarnings("all")
public class AVLTreeDemo {
    public static void main(String[] args) {
        AVLTree tree = new AVLTree();
//        int[] arr = {4, 3, 6, 5, 7, 8};
        int[] arr = {10, 12, 8, 9, 7, 6};
        for (int i = 0; i < arr.length; i++)
            tree.add(new Node(arr[i]));
        tree.infixOrder();
        System.out.printf("树高度：%d\n", tree.root.height());//3
        System.out.printf("左树高度：%d\n", tree.root.leftHeight());//2
        System.out.printf("右数高度：%d\n", tree.root.rightHeight());//2
        System.out.println("当前Root节点为: "+tree.getRoot());//8
    }
}

class AVLTree {
    public Node root;

    /**
     * @return 获取root节点
     */
    public Node getRoot() {
        return root;
    }

    /**
     * 添加节点
     *
     * @param node 传入要添加的节点
     */
    public void add(Node node) {
        if (root == null)
            root = node;
        else
            root.add(node);
    }

    /**
     * 中序遍历排序二叉树
     */
    public void infixOrder() {
        if (root == null)
            return;
        else
            root.infixOrder();
    }
}

@SuppressWarnings("all")
class Node {
    //节点的值
    public int value;
    //左子节点
    public Node left;
    //右子节点
    public Node right;

    //构造器初始化节点的值
    public Node(int value) {
        this.value = value;
    }

    //返回左子树的高度
    public int leftHeight() {
        if (left == null)
            return 0;
        else
            return left.height();
    }

    //返回右子树的高度
    public int rightHeight() {
        if (right == null)
            return 0;
        else
            return right.height();
    }

    //返回当前节点的高度,以当前节点为跟节点的树的高度
    public int height() {
        /**
         * 这里使用了递归：返回了左右子树，最高的那一个数值。
         * 递归原理： 第一个开始统计的时候，一定是一个叶子节点。
         *          根据这个逻辑：叶子节点的Math.max(0,0) = 0
         *          因为当前节点算一层，所以 +1;
         *          返回到上一层的时候，至少是这样：Math.max(1,0) = 1
         *          然后把自己本身的层+1.以此类推，返回到根节点的时候，就拿到了从包含根节点的树的高度
         */
        //+1；因为本身节点也要算上
        return Math.max(left == null ? 0 : left.height(),
                right == null ? 0 : right.height()) + 1;
    }

    /**
     * 创建右旋转方法
     */
    public void rightRotate() {
        //1.创建一个新的节点newNode(以10这个值创建)
        Node newNode = new Node(value);
        //2.把新节点的右子树设置为当前节点的右子树
        newNode.right = this.right;
        //3.把新节点的左子树设置为当前节点的左子树的右子树
        newNode.left = this.left.right;
        //4.把当前节点的值换为左子树的值
        this.value = this.left.value;
        //5.把当前节点的左子树设置为左子树的左子树
        this.left = this.left.left;
        //6.把当前节点的右子树设置为新节点
        this.right = newNode;
    }

    /**
     * 创建左旋转方法
     */
    public void leftRotate() {
        //1.创建一个新的节点newNode(以4这个值创建),创建一个新的节点，值等于当前根节点的值
        Node newNode = new Node(value);
        //2.把新节点的左子树设置为当前节点的左子树
        newNode.left = this.left;
        //3.把新节点的右子树设置为当前节点的右子树的左子树
        newNode.right = this.right.left;
        //4.把当前节点的值换为右子节点的值
        this.value = this.right.value;
        //5.把当前节点的右子树设置为右子树的右子树
        this.right = this.right.right;
        //6.把当前节点的左子树设置为新节点
        this.left = newNode;
    }

    /**
     * 添加节点
     *
     * @param node 传入要添加的节点进行添加
     */
    public void add(Node node) {
        if (node == null)
            return;
        if (node.value < this.value)
            if (this.left == null)
                this.left = node;
            else
                this.left.add(node);
        else if (this.right == null)
            this.right = node;
        else
            this.right.add(node);

        /**
         * 旋转的时候有以下规则
         * 没添加一个节点后：检查树的高度是否平衡
         *         如果右子树高度 - 左子树高度 > 1 ，则左旋转
         *         也就是说：每次旋转的层只涉及到4层(对照笔记上的图示理解)
         */
        if (rightHeight() - leftHeight() > 1)
            leftRotate();
        if (leftHeight() - rightHeight() > 1)
            rightRotate();
    }

    /**
     * 中序遍历排序二叉树
     */
    public void infixOrder() {
        if (this.left != null)
            this.left.infixOrder();
        System.out.println(this);
        if (this.right != null)
            this.right.infixOrder();
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
}
```

结果：

```
Node{value=6}
Node{value=7}
Node{value=8}
Node{value=9}
Node{value=10}
Node{value=12}
树高度：3
左树高度：2
右数高度：2
当前Root节点为: Node{value=8}
```

### ==单旋转==的<strong style="color:red">注意事项</strong> 

有些情况下，<strong style="color:red">单旋转不能完成平衡二叉树的转换如比数列</strong> {10,11,7,6,8,9} 或者 {2,1,6,5,7,3}; 运行原来的代码可以看到，并没有转成AVL树。

![问题分析](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/%E9%97%AE%E9%A2%98%E5%88%86%E6%9E%90.png)

![image-20230711145636238](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711145636238.png)

**问题分析**：

左侧这个树满足 `leftHeight - rightHeight > 1`，也就是满足右旋转，旋转之后，树结构变化了。但是还是一个非平衡二叉树

它的主要原因是：root 左子树的 左子树高度 小于 右子树的高度。即：节点 7 的左子树高度小于右子树的高度。

### 解决办法——双旋转

1.  先将 7 这个节点为 root 节点，进行左旋转
2.  再将原始的root节点进行右旋转

![123](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711145017212.png)

**其实可以参考下面两个单旋转的例子，它有这样一个特点**：

1.  **右旋转**：
    -  root 的 left 左子树高度 大于 右子树高度
    -  右旋转的时候，会将 left.right 旋转到 right.left 节点上个
2.  **左旋转**：
    -  root 的 right 右子树高度 大于 左子树高度
    -  左旋转的时候，会将 right.left 旋转到 left.right 上

如果不满足这个要求，在第二个操作的时候，就会导致 2 层的高度被旋转到 1 层的节点下面，导致不平衡了。

**解决代码如下**：

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1015:49
 * @comment
 */
@SuppressWarnings("all")
public class AVLTreeDemo {
    public static void main(String[] args) {
        AVLTree tree = new AVLTree();
//        int[] arr = {4, 3, 6, 5, 7, 8};
//        int[] arr = {10, 12, 8, 9, 7, 6};
       //下面两个数组是导致单旋转失败的例子
//        int[] arr = {10, 11, 7, 6, 8, 9};
        int[] arr = {2, 1, 6, 5, 7, 3};
        for (int i = 0; i < arr.length; i++)
            tree.add(new Node(arr[i]));
        tree.infixOrder();
        System.out.printf("树高度：%d\n", tree.root.height());//3
        System.out.printf("左树高度：%d\n", tree.root.leftHeight());//2
        System.out.printf("右数高度：%d\n", tree.root.rightHeight());//2
        System.out.println("当前Root节点为: " + tree.getRoot());//5
    }
}

class AVLTree {
    public Node root;

    /**
     * @return 获取root节点
     */
    public Node getRoot() {
        return root;
    }

    /**
     * 添加节点
     *
     * @param node 传入要添加的节点
     */
    public void add(Node node) {
        if (root == null)
            root = node;
        else
            root.add(node);
    }

    /**
     * 中序遍历排序二叉树
     */
    public void infixOrder() {
        if (root == null)
            return;
        else
            root.infixOrder();
    }
}

@SuppressWarnings("all")
class Node {
    //节点的值
    public int value;
    //左子节点
    public Node left;
    //右子节点
    public Node right;

    //构造器初始化节点的值
    public Node(int value) {
        this.value = value;
    }

    //返回左子树的高度
    public int leftHeight() {
        if (left == null)
            return 0;
        else
            return left.height();
    }

    //返回右子树的高度
    public int rightHeight() {
        if (right == null)
            return 0;
        else
            return right.height();
    }

    //返回当前节点的高度,以当前节点为跟节点的树的高度
    public int height() {
        /**
         * 这里使用了递归：返回了左右子树，最高的那一个数值。
         * 递归原理： 第一个开始统计的时候，一定是一个叶子节点。
         *          根据这个逻辑：叶子节点的Math.max(0,0) = 0
         *          因为当前节点算一层，所以 +1;
         *          返回到上一层的时候，至少是这样：Math.max(1,0) = 1
         *          然后把自己本身的层+1.以此类推，返回到根节点的时候，就拿到了从包含根节点的树的高度
         */
        //+1；因为本身节点也要算上
        return Math.max(left == null ? 0 : left.height(),
                right == null ? 0 : right.height()) + 1;
    }

    /**
     * 创建有旋转方法
     */
    public void rightRotate() {
        //1.创建一个新的节点newNode(以10这个值创建)
        Node newNode = new Node(value);
        //2.把新节点的右子树设置为当前节点的右子树
        newNode.right = this.right;
        //3.把新节点的左子树设置为当前节点的左子树的右子树
        newNode.left = this.left.right;
        //4.把当前节点的值换为左子树的值
        this.value = this.left.value;
        //5.把当前节点的左子树设置为左子树的左子树
        this.left = this.left.left;
        //6.把当前节点的右子树设置为新节点
        this.right = newNode;
    }

    /**
     * 创建左旋转方法
     */
    public void leftRotate() {
        //1.创建一个新的节点newNode(以4这个值创建),创建一个新的节点，值等于当前根节点的值
        Node newNode = new Node(value);
        //2.把新节点的左子树设置为当前节点的左子树
        newNode.left = this.left;
        //3.把新节点的右子树设置为当前节点的右子树的左子树
        newNode.right = this.right.left;
        //4.把当前节点的值换为右子节点的值
        this.value = this.right.value;
        //5.把当前节点的右子树设置为右子树的右子树
        this.right = this.right.right;
        //6.把当前节点的左子树设置为新节点
        this.left = newNode;
    }

    /**
     * 添加节点
     *
     * @param node 传入要添加的节点进行添加
     */
    public void add(Node node) {
        if (node == null)
            return;
        if (node.value < this.value)
            if (this.left == null)
                this.left = node;
            else
                this.left.add(node);
        else if (this.right == null)
            this.right = node;
        else
            this.right.add(node);

        /**
         * 旋转的时候有以下规则
         * 没添加一个节点后：检查树的高度是否平衡
         *         如果右子树高度 - 左子树高度 > 1 ，则左旋转
         *         也就是说：每次旋转的层只涉及到4层(对照笔记上的图示理解)
         */
        if (rightHeight() - leftHeight() > 1) {
            //如果它的右子树的左子树的高度大于它的右子树的右子树的高度
            if (right != null && right.leftHeight() > right.rightHeight()) {
                //先对右子节点进行右旋转
                right.rightRotate();
                //然后再对当前节点进行左旋转
                leftRotate();
            } else
                //直接进行左旋转
                leftRotate();
            //旋转好后阻止程序的向后执行否则就会产生错误的操作
            return;
        }
        if (leftHeight() - rightHeight() > 1)
            //如果它的左子树的右子树高度大于它的左子树的高度
            if (left != null && left.rightHeight() > left.leftHeight()) {
                //先对当前节点的左节点(左子树) -> 左旋转
                left.leftRotate();
                //再对当前节点进行右旋转
                rightRotate();
            } else
                //直接进行右旋转
                rightRotate();
    }

    /**
     * 中序遍历排序二叉树
     */
    public void infixOrder() {
        if (this.left != null)
            this.left.infixOrder();
        System.out.println(this);
        if (this.right != null)
            this.right.infixOrder();
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
}
```

结果：

```
Node{value=1}
Node{value=2}
Node{value=3}
Node{value=5}
Node{value=6}
Node{value=7}
树高度：3
左树高度：2
右数高度：2
当前Root节点为: Node{value=5}
```
