---
layout: post
title: 二叉排序树
categories: [java,数据结构与算法,数据结构,算法]
description: 二叉排序树的介绍和实现
keywords: 算法
---

### 二叉排序树

**先看一个需求** 

给你一个数列 {7,3,10,12,5,1,9} ，要求能够高效的完成对数据的查询和添加。

**解决方案分析** 

1.  **数组未排序** 

    1.  优点：直接在数组尾添加，速度快。
    2.  缺点：查找速度慢

2.  **数组排序** 

    1.  优点：可以使用二分查找，查找速度快
    2.  缺点：为了保证数组有序在添加新数据时，找到插入位置后，后面的数据需要整体移动，速度慢

3.  **使用链式存储——链表** 

    不管链表是否有序，查找速度都慢，添加数据速度比数组快，不需要数据整体移动

**二叉排序树介绍** 

二叉排序树：**BST：(Binary Sort(Search)Tree)**，对于二叉排序树的任何一个**非叶子结点**，要求如下：

-  左子节点的值比当前节点^当前节点指的父节点^的值小。
-  右子节点的值比当前节点^^的值大。

==特别说明==：如果有相同的值，可以将该节点放在左子节点或右子节点。

比如针对前面的数据 {7,3,10,12,5,1,9}，对应的二叉排序树为：

![image-20230707160016773](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230707160016773.png)

1.  从根节点开始，发现比7小，直接往左子树查找，相当于直接折半了。
2.  比3小，再次折半
3.  比1大，直接挂在1的有节点

#### 二叉排序树==创建==和==遍历== 

一个数组创建成对应的二叉排序树，并使用中序遍历二叉排序树，比如：数组为Array {7,3,10,12,5,1,9} ，创建成对应的二叉排序树为：

![image-20230707185816753](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230707185816753.png)

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/719:25
 * @comment
 */
@SuppressWarnings("all")
public class BinarySortTree {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9};
        BinarySort binarySort = new BinarySort();
        for (int i = 0; i < arr.length; i++) {
            binarySort.add(new Node(arr[i]));
        }

        System.out.println("------------中序遍历二叉排序树------------");
        binarySort.infixOrder();
    }
}

@SuppressWarnings("all")
class BinarySort {
    private Node root;

    public void add(Node node) {
        //判断root根节点如果为null则将传入的当前赋值为根节点
        if (root == null) {
            root = node;
        } else {
            //调用add方法添加传入的节点
            root.add(node);
        }
    }


    public void infixOrder() {
        //如果根节点不为null则开始中序遍历二叉排序树
        if (root != null) {
            root.infixOrder();
        } else {
            System.out.println("二叉排序树为空");
        }
    }
}

@SuppressWarnings("all")
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    //添加传入节点
    public void add(Node node) {
        //判断传入的节点是否为空
        if (node == null) {
            return;
        }
        //判断传入的节点的值是否小于当前节点的值
        if (node.value < this.value) {
            //判断当前节点的左子节点是否为空
            if (this.left == null) {
                //为空则将传入的节点赋值到左子节点中
                this.left = node;
            } else {
                //向左递归
                this.left.add(node);
            }
        } else {
            //判断当前节点的右子节点是否为空
            if (this.right == null) {
                //为空将传入的节点赋值到当前节点的右子节点中
                this.right = node;
            } else {
                //向右递归
                this.right.add(node);
            }
        }
    }

    //二叉排序树中序遍历
    public void infixOrder() {
        //左
        if (this.left != null) {
            this.left.infixOrder();
        }
        //中
        System.out.println(this);
        //右
        if (this.right != null) {
            this.right.infixOrder();
        }
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
}
```

#### 二叉排序树的==删除== 

二叉排序树的删除情况比较复杂，有下面三种情况需要考虑

1.  删除==叶子节点== (比如：2,5,9,12)
2.  删除==只有一颗子树的节点== (比如：1)
3.  删除==有两颗子树的节点== (比如：7,3,10)

​																								**示意图**   

![image-20230707193738478](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230707193738478.png)

#### 删除==叶子节点== 

删除的节点是叶子节点，即该节点下没有左右子节点。

**思路分析**  

```
第一种情况:
删除叶子节点 (比如：2, 5, 9, 12)
思路
(1) 需求先去找到要删除的结点  targetNode
(2)  找到targetNode 的 父结点 parent 
(3)  确定 targetNode 是 parent的左子结点 还是右子结点
(4)  根据前面的情况来对应删除
左子结点 parent.left = null
右子结点 parent.right = null;
```

![image-20230707194108379](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230707194108379.png)

**代码实现** 

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/719:25
 * @comment
 */
@SuppressWarnings("all")
public class BinarySortTree {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9};
        BinarySort binarySort = new BinarySort();
        for (int i = 0; i < arr.length; i++) {
            binarySort.add(new Node(arr[i]));
        }

        System.out.println("------------中序遍历二叉排序树------------");
        binarySort.infixOrder();

        //测试删除叶子节点
        binarySort.delNode(10);//因为有子节点 所以删除无效
        System.out.println("------------删除节点后------------");
        binarySort.infixOrder();


        binarySort.delNode(1);//因为没有子节点 所以删除成功
        System.out.println("------------删除节点后------------");
        binarySort.infixOrder();
    }
}

@SuppressWarnings("all")
class BinarySort {
    private Node root;

    public void add(Node node) {
        //判断root根节点如果为null则将传入的当前赋值为根节点
        if (root == null) {
            root = node;
        } else {
            //调用add方法添加传入的节点
            root.add(node);
        }
    }


    public void infixOrder() {
        //如果根节点不为null则开始中序遍历二叉排序树
        if (root != null) {
            root.infixOrder();
        } else {
            System.out.println("二叉排序树为空");
        }
    }

    /**
     * 查找要删除的节点
     * @param value
     * @return
     */
    public Node search(int value) {
        if (root == null)
            return null;
        else
            return root.search(value);
    }

    /**
     * 查找要删除的节点的父节点
     * @param value
     * @return
     */
    public Node searchParent(int value){
        if(root == null)
            return null;
        else
            return root.searchParent(value);
    }

    /**
     * 删除节点
     * @param value
     */
    public void delNode(int value){
        if(root == null)
            return;
        else{
            //1.需求先去找到要删除的节点，targetNode
            Node targetNode = search(value);
           //如果没有找到要删除的节点
            if(targetNode == null)
                return;
            //如果我们发现当前这个二叉排序树只有一个节点则删除即可
            if(root.left == null && root.right == null)
                root = null;
            //去查找targetNode的父节点
            Node parent = searchParent(value);
            //如果删除的节点是叶子节点
            if(targetNode.left == null && targetNode.right == null){
               //判断targetNode节点是父节点的左子节点还是右子节点
                if(parent.left != null && parent.left.value == value)//是左子节点
                    parent.left = null;

                if(parent.right != null && parent.right.value == value)//是右子节点
                    parent.right = null;
            }
        }
    }
}

@SuppressWarnings("all")
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    /**
     * 查找要删除的节点
     *
     * @param value 希望删除的节点的值 [目标值]
     * @return 找到返回节点，否则返回null
     */
    public Node search(int value) {
        if (value == this.value) {
            return this;
        } else if (value < this.value) {
            if (this.left == null)
                return null;
            return this.left.search(value);
        } else {
            if (this.right == null)
                return null;
            return this.right.search(value);
        }
    }

    /**
     * 查找要删除节点的父节点
     *
     * @param value 要查找的节点值
     * @return 返回要删除节点的父节点，否则返回null
     */
    public Node searchParent(int value) {
        //如果当前节点就是要删除的节点的父节点则返回当前节点
        if (this.left != null && this.left.value == value ||
                this.right != null && this.right.value == value)
            return this;
        else {
            //如果查找的值小于当前节点的值，并且当前节点的左子节点不为空
            if (value < this.value && this.left != null)
                //向左子树递归查找
                return this.left.searchParent(value);
            else if (value >= this.value && this.right != null)
                //向右子树递归查找
                return this.right.searchParent(value);
            else
                //没有找到父节点
                return null;
        }
    }

    //添加传入节点
    public void add(Node node) {
        //判断传入的节点是否为空
        if (node == null) {
            return;
        }
        //判断传入的节点的值是否小于当前节点的值
        if (node.value < this.value) {
            //判断当前节点的左子节点是否为空
            if (this.left == null) {
                //为空则将传入的节点赋值到左子节点中
                this.left = node;
            } else {
                //向左递归
                this.left.add(node);
            }
        } else {
            //判断当前节点的右子节点是否为空
            if (this.right == null) {
                //为空将传入的节点赋值到当前节点的右子节点中
                this.right = node;
            } else {
                //向右递归
                this.right.add(node);
            }
        }
    }

    //二叉排序树中序遍历
    public void infixOrder() {
        //左
        if (this.left != null) {
            this.left.infixOrder();
        }
        //中
        System.out.println(this);
        //右
        if (this.right != null) {
            this.right.infixOrder();
        }
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
}
```

#### 删除==节点有一个子节点== 

删除的节点有一个子节点，该节点有左子节点或者右子节点。比如这里的 1

![image-20230709150143209](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230709150143209.png)

思路分析：

```
第二种情况: 删除只有一颗子树的节点 比如 1
思路
(1) 需求先去找到要删除的结点  targetNode
(2)  找到targetNode 的 父结点 parent 
(3) 确定targetNode 的子结点是左子结点还是右子结点
(4) targetNode 是 parent 的左子结点还是右子结点
(5) 如果targetNode 有左子结点
5. 1 如果 targetNode 是 parent 的左子结点
parent.left = targetNode.left;
5.2  如果 targetNode 是 parent 的右子结点
parent.right = targetNode.left;
(6) 如果targetNode 有右子结点
6.1 如果 targetNode 是 parent 的左子结点
parent.left = targetNode.right;
6.2 如果 targetNode 是 parent 的右子结点
parent.right = targetNode.right
```

![image-20230707194108379](./images/image-20230707194108379-1688886389817-1.png)

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/719:25
 * @comment
 */
@SuppressWarnings("all")
public class BinarySortTree {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9};
        BinarySort binarySort = new BinarySort();
        for (int i = 0; i < arr.length; i++) {
            binarySort.add(new Node(arr[i]));
        }

        System.out.println("------------中序遍历二叉排序树------------");
        binarySort.infixOrder();

        //测试删除叶子节点
        binarySort.delNode(1);//因为有子节点 所以删除无效
        System.out.println("------------删除节点后------------");
        binarySort.infixOrder();
    }
}

@SuppressWarnings("all")
class BinarySort {
    private Node root;

    public void add(Node node) {
        //判断root根节点如果为null则将传入的当前赋值为根节点
        if (root == null) {
            root = node;
        } else {
            //调用add方法添加传入的节点
            root.add(node);
        }
    }


    public void infixOrder() {
        //如果根节点不为null则开始中序遍历二叉排序树
        if (root != null) {
            root.infixOrder();
        } else {
            System.out.println("二叉排序树为空");
        }
    }

    /**
     * 查找要删除的节点
     * @param value
     * @return
     */
    public Node search(int value) {
        if (root == null)
            return null;
        else
            return root.search(value);
    }

    /**
     * 查找要删除的节点的父节点
     * @param value
     * @return
     */
    public Node searchParent(int value){
        if(root == null)
            return null;
        else
            return root.searchParent(value);
    }

    /**
     * 删除节点
     * @param value
     */
    public void delNode(int value){
        if(root == null)
            return;
        else{
            //1.需求先去找到要删除的节点，targetNode
            Node targetNode = search(value);
           //如果没有找到要删除的节点
            if(targetNode == null)
                return;
            //如果我们发现当前这个二叉排序树只有一个节点则删除即可
            if(root.left == null && root.right == null)
                root = null;
            //去查找targetNode的父节点
            Node parent = searchParent(value);
            //如果删除的节点是叶子节点
            if(targetNode.left == null && targetNode.right == null){
               //判断targetNode节点是父节点的左子节点还是右子节点
                if(parent.left != null && parent.left.value == value)//是左子节点
                    parent.left = null;

                if(parent.right != null && parent.right.value == value)//是右子节点
                    parent.right = null;

            }else //删除只有一个叶子节点的子树节点
                if(targetNode.left != null)//如果要删除的节点有左节点
                   if(parent != null)
                       if(parent.left.value == value)//如果targetNode是parent的左子节点
                           parent.left = targetNode.left;
                       else
                           parent.right = targetNode.left;
           				else 
                          root = targetNode.left;
                else//如果要删除的节点有右子节点
                   if(parent != null)
                       //如果targetNode是parent的左子节点
                       if(parent.left.value == value)
                           parent.left = targetNode.right;
                       else//如果targetNode是parent的右子节点
                           parent.right = targetNode.right;
           			 else
                       root = targetNode.left;
        }
    }
}

@SuppressWarnings("all")
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    /**
     * 查找要删除的节点
     *
     * @param value 希望删除的节点的值 [目标值]
     * @return 找到返回节点，否则返回null
     */
    public Node search(int value) {
        if (value == this.value) {
            return this;
        } else if (value < this.value) {
            if (this.left == null)
                return null;
            return this.left.search(value);
        } else {
            if (this.right == null)
                return null;
            return this.right.search(value);
        }
    }

    /**
     * 查找要删除节点的父节点
     *
     * @param value 要查找的节点值
     * @return 返回要删除节点的父节点，否则返回null
     */
    public Node searchParent(int value) {
        //如果当前节点就是要删除的节点的父节点则返回当前节点
        if (this.left != null && this.left.value == value ||
                this.right != null && this.right.value == value)
            return this;
        else {
            //如果查找的值小于当前节点的值，并且当前节点的左子节点不为空
            if (value < this.value && this.left != null)
                //向左子树递归查找
                return this.left.searchParent(value);
            else if (value >= this.value && this.right != null)
                //向右子树递归查找
                return this.right.searchParent(value);
            else
                //没有找到父节点
                return null;
        }
    }

    //添加传入节点
    public void add(Node node) {
        //判断传入的节点是否为空
        if (node == null) {
            return;
        }
        //判断传入的节点的值是否小于当前节点的值
        if (node.value < this.value) {
            //判断当前节点的左子节点是否为空
            if (this.left == null) {
                //为空则将传入的节点赋值到左子节点中
                this.left = node;
            } else {
                //向左递归
                this.left.add(node);
            }
        } else {
            //判断当前节点的右子节点是否为空
            if (this.right == null) {
                //为空将传入的节点赋值到当前节点的右子节点中
                this.right = node;
            } else {
                //向右递归
                this.right.add(node);
            }
        }
    }

    //二叉排序树中序遍历
    public void infixOrder() {
        //左
        if (this.left != null) {
            this.left.infixOrder();
        }
        //中
        System.out.println(this);
        //右
        if (this.right != null) {
            this.right.infixOrder();
        }
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
}
```

#### 删除==节点有两个子节点== 

删除的节点有两个子节点，即该节点有左子节点和右子节点。比如  7,3,10

![image-20230709150453717](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230709150453717.png)

思路分析：

```
情况三 ： 删除有两颗子树的节点. (比如：7, 3，10 )
思路
(1) 需求先去找到要删除的结点  targetNode
(2)  找到targetNode 的 父结点 parent 
(3)  从targetNode 的右子树找到最小的结点
(4) 用一个临时变量，将 最小结点的值保存 temp = 11
(5)  删除该最小结点
(6)  targetNode.value = temp
```

![image-20230707194108379](./images/image-20230707194108379-1688886398397-4.png)

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/719:25
 * @comment
 */
@SuppressWarnings("all")
public class BinarySortTree {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9};
        BinarySort binarySort = new BinarySort();
        for (int i = 0; i < arr.length; i++) {
            binarySort.add(new Node(arr[i]));
        }

        System.out.println("------------中序遍历二叉排序树------------");
        binarySort.infixOrder();

        //测试删除叶子节点
        binarySort.delNode(1);//因为有子节点 所以删除无效
        System.out.println("------------删除节点后------------");
        binarySort.infixOrder();

        binarySort.delNode(10);//因为有子节点 所以删除无效
        System.out.println("------------删除节点后------------");
        binarySort.infixOrder();
    }
}

@SuppressWarnings("all")
class BinarySort {
    private Node root;

    public void add(Node node) {
        //判断root根节点如果为null则将传入的当前赋值为根节点
        if (root == null) {
            root = node;
        } else {
            //调用add方法添加传入的节点
            root.add(node);
        }
    }


    public void infixOrder() {
        //如果根节点不为null则开始中序遍历二叉排序树
        if (root != null) {
            root.infixOrder();
        } else {
            System.out.println("二叉排序树为空");
        }
    }

    /**
     * 查找要删除的节点
     * @param value
     * @return
     */
    public Node search(int value) {
        if (root == null)
            return null;
        else
            return root.search(value);
    }

    /**
     * 查找要删除的节点的父节点
     * @param value
     * @return
     */
    public Node searchParent(int value){
        if(root == null)
            return null;
        else
            return root.searchParent(value);
    }

    /**
     * 删除节点
     * @param value
     */
    public void delNode(int value){
        if(root == null)
            return;
        else{
            //1.需求先去找到要删除的节点，targetNode
            Node targetNode = search(value);
           //如果没有找到要删除的节点
            if(targetNode == null)
                return;
            //如果我们发现当前这个二叉排序树只有一个节点则删除即可
            if(root.left == null && root.right == null)
                root = null;
            //去查找targetNode的父节点
            Node parent = searchParent(value);
            //如果删除的节点是叶子节点
            if(targetNode.left == null && targetNode.right == null){
               //判断targetNode节点是父节点的左子节点还是右子节点
                if(parent.left != null && parent.left.value == value)//是左子节点
                    parent.left = null;

                if(parent.right != null && parent.right.value == value)//是右子节点
                    parent.right = null;

            }else if(targetNode.left != null && targetNode.right != null){//删除有两个叶子节点的子树
                int min = delRightTreeMin(targetNode.right);
                targetNode.value = min;
            }else //删除只有一个叶子节点的子树节点
                if(targetNode.left != null)//如果要删除的节点有左节点
                   if(parent != null)
                       if(parent.left.value == value)//如果targetNode是parent的左子节点
                           parent.left = targetNode.left;
                       else
                           parent.right = targetNode.left;
           			 else
                       root = targetNode.left;
                else//如果要删除的节点有右子节点
                   if(parent != null)
                       //如果targetNode是parent的左子节点
                       if(parent.left.value == value)
                           parent.left = targetNode.right;
                       else//如果targetNode是parent的右子节点
                           parent.right = targetNode.right;
           		     else
                        root = targetNode.left;
        }
    }
   
    /**
     * 1.返回 以Node为根节点的二叉排序树的最小节点的值
     * 2.删除node，为根节点的二叉排序树的最小节点
     * @param node 传入的节点(当做二叉排序树的根节点)
     * @return 返回的以node 为根节点的二叉排序树的最小节点的值
     */
    public int delRightTreeMin(Node node){
        Node target = node;
        //循环左子节点到最底下就是最小值
        while(target.left != null)
            target = target.left;
       
		 //这时target就只指向了最小节点
        //删除最小节点
        delNode(target.value);
        return target.value;
    }
}

@SuppressWarnings("all")
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    /**
     * 查找要删除的节点
     *
     * @param value 希望删除的节点的值 [目标值]
     * @return 找到返回节点，否则返回null
     */
    public Node search(int value) {
        if (value == this.value) {
            return this;
        } else if (value < this.value) {
            if (this.left == null)
                return null;
            return this.left.search(value);
        } else {
            if (this.right == null)
                return null;
            return this.right.search(value);
        }
    }

    /**
     * 查找要删除节点的父节点
     *
     * @param value 要查找的节点值
     * @return 返回要删除节点的父节点，否则返回null
     */
    public Node searchParent(int value) {
        //如果当前节点就是要删除的节点的父节点则返回当前节点
        if (this.left != null && this.left.value == value ||
                this.right != null && this.right.value == value)
            return this;
        else {
            //如果查找的值小于当前节点的值，并且当前节点的左子节点不为空
            if (value < this.value && this.left != null)
                //向左子树递归查找
                return this.left.searchParent(value);
            else if (value >= this.value && this.right != null)
                //向右子树递归查找
                return this.right.searchParent(value);
            else
                //没有找到父节点
                return null;
        }
    }

    //添加传入节点
    public void add(Node node) {
        //判断传入的节点是否为空
        if (node == null) {
            return;
        }
        //判断传入的节点的值是否小于当前节点的值
        if (node.value < this.value) {
            //判断当前节点的左子节点是否为空
            if (this.left == null) {
                //为空则将传入的节点赋值到左子节点中
                this.left = node;
            } else {
                //向左递归
                this.left.add(node);
            }
        } else {
            //判断当前节点的右子节点是否为空
            if (this.right == null) {
                //为空将传入的节点赋值到当前节点的右子节点中
                this.right = node;
            } else {
                //向右递归
                this.right.add(node);
            }
        }
    }

    //二叉排序树中序遍历
    public void infixOrder() {
        //左
        if (this.left != null) {
            this.left.infixOrder();
        }
        //中
        System.out.println(this);
        //右
        if (this.right != null) {
            this.right.infixOrder();
        }
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
}
```
