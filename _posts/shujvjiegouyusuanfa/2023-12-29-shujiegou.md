---
layout: post
title: 树结构
categories: [java,数据结构与算法,数据结构]
description: 树结构的实现
keywords: 数据结构
---

## 树结构基础部分

### 二叉树

**为什么需要树这种数据结构** 

1. 数组存储方式的分析 
**优点**：通过==下标方式访问元素==，速度快。对于有序数组，还可使用==二分查找提高检索速度==。 
**缺点**：如果要检索具体某个值，或者==插入值(按一定顺序)会整体移动==，效率较低 [示意图]。

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903674.png)

2. 链式存储方式的分析 
**优点**：在一定程度上对数组存储方式有优化(比如：插入一个数值节点，只需要将插入节点，链接到链表中即可， 删除效率也很好)。 
**缺点**：在进行检索时，效率仍然较低，比如(检索某个值，需要从头节点开始遍历) 
3. 【示意图】

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903376.png)

3. 树存储方式的分析 
能提高数据存储，读取的效率, 比如利用 二叉排序树(Binary Sort Tree)，既可以保证数据的检索速度，同时也可以保证数据的插入，删除，修改的速度。【示意图,后面详讲】 
**案例**: [7, 3, 10, 1, 5, 9, 12]

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903631.png)

#### 树的常用术语：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903841.png)

树的常用术语(结合示意图理解):

1. 节点
2. 根节点
3. 父节点
4. 子节点
5. 叶子节点 (没有子节点的节点)
6. 节点的权(节点值)
7. 路径(从root节点找到该节点的路线）
8. 层
9. 子树
10. 树的高度(最大层数)
11. 森林 :多颗子树构成森林 

**二叉树的概念** 

1. 树有很多种，==每个节点最多只能有两个子节点==的一种形式==称为二叉树==。
2. 二叉树的==子节点==分为==左节点==和==右节点==。

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903760.png)

**二叉树的概念** 

3. 如果该二叉树的所有==叶子节点都在最后一层==，并且结点总数 = 2^n -1 , n 为层数，则我们称为==满二叉树==。
4. 如果该二叉树的所有==叶子节点都在最后一层==或者==倒数第二层==，==而且====最后一层的叶子节点在左边连续==，==倒数第二层的叶子节点在右边连续==，我们称为==完全二叉树==。
我们重点讲解一下二叉树的**前序遍历**，**中序遍历**和**后序遍历**。

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903921.png)

#### 二叉树遍历应用实例(前序,中序,后序)

**二叉树遍历的说明** 

使用前序,中序和后序对下面的二叉树进行遍历.

==前序遍历==: 先输出父节点，再遍历左子树和右子树

==中序遍历==: 先遍历左子树，再输出父节点，再遍历右子树

==后序遍历==: 先遍历左子树，再遍历右子树，最后输出父节点

==小结==:  看输出父节点的顺序，就确定是前序，中序还是后序

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903028.png)

**要求如下**：

1. 前上图的 3号节点 "卢俊"  , 增加一个左子节点 [5, 关胜]
2. 使用前序，中序，后序遍历，请写出各自输出的顺序是什么?

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903018.png)

**输出顺序思路**：

>  前序：中，左，右 -> 父节点(1) > 左 (2)[左节点没有右节点就到下一个旁边的] > 右(3)[先输出父节点，然后左节点和右节点] > 左(5) > 右(4)
>  最终结果：1，2，3，5，4
>
>  中序：左，中，右 -> 左节点(2)[没有左右节点] > 父节点(1) > 左节点(5)[3是父节点它的左节点是3] > 父节点(3) > 右节点 (4)
>  最终结果：2，1，5，3，4
>
>  后序：左，右，中 -> 左节点(2)[没有左右节点到旁边节点] > 3节点的左节点(5)[节点3是父节点先输出左节点5] > 3节点的有节点(4) > 父节点(3) > 父节点(1)
>  最终结果：2，5，4，3，1

代码实现：

测试前序，中序，后序的输出顺序的代码案例

```java
@SuppressWarnings("all")
public class BinaryTreeDemo {
    public static void main(String[] args) {
        //创建一个二叉树
        BinaryTree binaryTree = new BinaryTree();
        //创建节点
        HeroNode root = new HeroNode(1,"宋江");
        HeroNode node = new HeroNode(2,"吴用");
        HeroNode node1 = new HeroNode(3,"卢俊义");
        HeroNode node2 = new HeroNode(4,"林冲");
        HeroNode node3 = new HeroNode(5,"关胜");

        //手动创建二叉树(后面使用递归方式创建二叉树)
        root.setLeft(node);
        root.setRight(node1);
        node1.setRight(node2);
        node1.setLeft(node3);

        binaryTree.setRoot(root);

        //测试
        System.out.println("前序遍历");//1,2,3,5,4
        binaryTree.preOrder();
        //测试
        System.out.println("中序遍历");
        binaryTree.infixOrder();//2,1,3,5,4
        //测试
        System.out.println("后序遍历");
        binaryTree.postOrder();//5,4,3,2,1
    }
}
@SuppressWarnings("all")
//定义BinaryTree二叉树
class BinaryTree {
    private HeroNode root;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    //前序遍历，遍历是从根节点(root)开始的
    public void preOrder(){
        if(this.root != null){
            this.root.preOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }

    //中序遍历
    public void infixOrder(){
        if(this.root != null){
            this.root.infixOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }

    //后序遍历
    public void postOrder(){
        if(this.root != null){
            this.root.postOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }
}
@SuppressWarnings("all")
//创建HeroNode节点
class HeroNode {
    private int no;
    private String name;
    private HeroNode left;
    private HeroNode right;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }

    //前序遍历方法
    public void preOrder(){
        //先输出父节点
        System.out.println(this);
        //递归向左子树前序遍历
        if(this.left != null){
            this.left.preOrder();
        }
        //递归向右子树前序遍历
        if(this.right != null){
            this.right.preOrder();
        }
    }
    //中序遍历方法
    public void infixOrder(){
        //递归向左子树中序遍历
        if(this.left != null){
            this.left.infixOrder();
        }
        //输出父节点
        System.out.println(this);
        //递归向右子树中序遍历
        if(this.right != null){
            this.right.infixOrder();
        }
    }
    //后序遍历方法
    public void postOrder(){
        //递归向左子树后序遍历
        if(this.left != null){
            this.left.postOrder();
        }
        //递归向右子树后序遍历
        if(this.right != null){
            this.right.postOrder();
        }
        //输出父节点
        System.out.println(this);
    }
}
```

#### 二叉树-==查找指定节点== 

要求

1.  请编写**前序查找**，**中序查找**和**后序查找**的**方法** 
2.  并分别使用**三种查找方式**，**查找heroNode = 5**的节点
3.  并**分析各种查找方式**，分别**比较了多少次** 

代码演示：

```java
@SuppressWarnings("all")
public class BinaryTreeDemo {
    public static void main(String[] args) {
        //创建一个二叉树
        BinaryTree binaryTree = new BinaryTree();
        //创建节点
        HeroNode root = new HeroNode(1,"宋江");
        HeroNode node = new HeroNode(2,"吴用");
        HeroNode node1 = new HeroNode(3,"卢俊义");
        HeroNode node2 = new HeroNode(4,"林冲");
        HeroNode node3 = new HeroNode(5,"关胜");

        //手动创建二叉树(后面使用递归方式创建二叉树)
        root.setLeft(node);
        root.setRight(node1);
        node1.setRight(node2);
        node1.setLeft(node3);

        binaryTree.setRoot(root);

        //测试
        System.out.println("前序遍历");//1,2,3,5,4
//        binaryTree.preOrder();
        //测试
        System.out.println("中序遍历");
//        binaryTree.infixOrder();//2,1,3,5,4
        //测试
        System.out.println("后序遍历");
//        binaryTree.postOrder();//5,4,3,2,1

        System.out.println("-----------------前序遍历方式-----------------");
        HeroNode resNode = binaryTree.preOrderSearch(5);
        if(resNode != null){
            System.out.printf("查找到的信息为 [↓]\n>>>\t\tid:\t%d\n>>>\t\tname:\t%s\n",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d的信息",resNode.getNo());
        }

        System.out.println("-----------------中序遍历方式-----------------");
        HeroNode resNode1 = binaryTree.infixOrderSearch(5);
        if(resNode1 != null){
            System.out.printf("查找到的信息为 [↓]\n>>>\t\tid:\t%d\n>>>\t\tname:\t%s\n",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d的信息",resNode.getNo());
        }

        System.out.println("-----------------后序遍历方式-----------------");
        HeroNode resNode2 = binaryTree.postOrderSearch(5);
        if(resNode2 != null){
            System.out.printf("查找到的信息为 [↓]\n>>>\t\tid:\t%d\n>>>\t\tname:\t%s\n",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d的信息",resNode.getNo());
        }
    }
}
@SuppressWarnings("all")
//定义BinaryTree二叉树
class BinaryTree {
    private HeroNode root;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    //前序遍历，遍历是从根节点(root)开始的
    public void preOrder(){
        if(this.root != null){
            this.root.preOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }

    //中序遍历
    public void infixOrder(){
        if(this.root != null){
            this.root.infixOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }

    //后序遍历
    public void postOrder(){
        if(this.root != null){
            this.root.postOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }

    //前序遍历查找
    public HeroNode preOrderSearch(int no){
        if(root != null){
            return root.preOrderSearch(no);
        }else{
            return null;
        }
    }

    //中序遍历查找
    public HeroNode infixOrderSearch(int no){
        if(root != null){
            return root.infixOrderSearch(no);
        }else{
            return null;
        }
    }

    //后序遍历查找
    public HeroNode postOrderSearch(int no){
        if(root != null){
            return root.postOrderSearch(no);
        }else{
            return null;
        }
    }
}
@SuppressWarnings("all")
//创建HeroNode节点
class HeroNode {
    private int no;
    private String name;
    private HeroNode left;
    private HeroNode right;

    public static int countPre = 0;
    public static int countInfix = 0;
    public static int countPost = 0;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }

    //前序遍历方法
    public void preOrder(){
        //先输出父节点
        System.out.println(this);
        //递归向左子树前序遍历
        if(this.left != null){
            this.left.preOrder();
        }
        //递归向右子树前序遍历
        if(this.right != null){
            this.right.preOrder();
        }
    }

    //中序遍历方法
    public void infixOrder(){
        //递归向左子树中序遍历
        if(this.left != null){
            this.left.infixOrder();
        }
        //输出父节点
        System.out.println(this);
        //递归向右子树中序遍历
        if(this.right != null){
            this.right.infixOrder();
        }
    }

    //后序遍历方法
    public void postOrder(){
        //递归向左子树后序遍历
        if(this.left != null){
            this.left.postOrder();
        }
        //递归向右子树后序遍历
        if(this.right != null){
            this.right.postOrder();
        }
        //输出父节点
        System.out.println(this);
    }

    /**
     * 前序遍历查找
     * @param no 查找no
     * @return 如果找到就返回该Node，如果没有找到返回null
     */
    public HeroNode preOrderSearch(int no){
        countPre++;
        System.out.printf("进入前序遍历~~~\t一共遍历了>\t%d\t<次\n",countPre);
        //比较当前节点是不是要查找的节点
        if(this.no == no){
            return this;
        }
        //则判断当前节点的左子节点是否为空，如果不为空，则递归前序查找
        HeroNode heroNode = null;
        /**
         * 1.判断当前节点的左子节点是否为空，如果不为空，则递归前序查找
         * 2.如果做递归前序查找，找到节点，则返回
         */
        if(this.left != null){
            heroNode = this.left.preOrderSearch(no);
        }
        //说明左子树找到
        if(heroNode != null){
            return heroNode;
        }
        if(this.right != null){
            heroNode = this.right.preOrderSearch(no);
        }
        return heroNode;
    }

    //中序遍历查找
    public HeroNode infixOrderSearch(int no){
        HeroNode node = null;
        //判断左子节点是否为空，如果不为空则递归向左子节点遍历查找
        if(this.left != null){
            node = this.left.infixOrderSearch(no);
        }

        if(node != null){
            return node;
        }

        countInfix ++;
        System.out.printf("进入中序遍历~~~\t一共遍历了>\t%d\t<次\n",countInfix);
        //判断当前节点是否 为需要查找的
        if(this.no == no){
            return this;
        }

        //判断右子节点是否为空，如果不为空则递归向右子节点遍历查找
        if(this.right != null){
            node = this.right.infixOrderSearch(no);
        }

        //无论是否为空 都要返回递归节点
        return node;
    }

    //后序遍历查找
    public HeroNode postOrderSearch(int no){
        HeroNode node = null;
        //判断左子节点是否为空
        if(this.left != null){
            node = this.left.postOrderSearch(no);
        }

        //判断节点不为空则返回
        if(node != null){
            return node;
        }

        //判断右子节点是否为空
        if(this.right != null){
            node = this.right.postOrderSearch(no);
        }

        if(node != null){
            return node;
        }

        countPost ++;
        System.out.printf("进入后序遍历~~~\t一共遍历了>\t%d\t<次\n",countPost);

        //判断是否为需要查找的节点
        if(this.no == no){
            return this;
        }
        return node;
    }
}
```

运行结果：

```
-----------------前序遍历方式-----------------
进入前序遍历~~~	一共遍历了>	1	<次
进入前序遍历~~~	一共遍历了>	2	<次
进入前序遍历~~~	一共遍历了>	3	<次
进入前序遍历~~~	一共遍历了>	4	<次
查找到的信息为 [↓]
>>>		id:	5
>>>		name:	关胜
-----------------中序遍历方式-----------------
进入中序遍历~~~	一共遍历了>	1	<次
进入中序遍历~~~	一共遍历了>	2	<次
进入中序遍历~~~	一共遍历了>	3	<次
查找到的信息为 [↓]
>>>		id:	5
>>>		name:	关胜
-----------------后序遍历方式-----------------
进入后序遍历~~~	一共遍历了>	1	<次
进入后序遍历~~~	一共遍历了>	2	<次
查找到的信息为 [↓]
>>>		id:	5
>>>		name:	关胜
```

**分析结果**：前序查找次数：4 ，中序查找次数：3 ，后序查找次数：2

#### 二叉树-==删除节点== 

要求：

1.  如果删除的节点是叶子节点，则删除该节点。
2.  如果删除的节点是非叶子节点，则删除该子树。
3.  测试，删除掉5号叶子节点和3号子树。

![image-20230630120206935](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230630120206935.png)

完成删除的思路分析：

二叉树-删除节点

思考题(课后练习)

如果要删除的节点是非叶子节点，现在我们不希望将该非叶子节点为根节点的子树删除，**需要指定规则**,**假如规定如下**:

1.  如果该非叶子节点A只有一个子节点B，则子节点B替代节点A
2.  如果该非叶子节点A有左子节点B和右子节点C，则让左子节点B替代节点A。

思路：

>  首先先处理：
>
>  考虑如果树是空树root，如果只有一个root节点，则等价将二叉树置空

然后进行下面步骤：

1.  因为我们的二叉树是单向的，所以我们是判断当前节点的子节点是否需要删除节点，而不能去判断当前这个节点是不是需要删除节点。
2.  如果当前节点的左子节点不为空，并且左子节点就是要删除节点，就将`this.left = null;`并且就返回^`return;`^(结束递归删除)
3.  如果当前节点的右子节点不为空，并且右子节点就是要删除节点，就将`this.right = null;`并且就返回^`return;`^(结束递归删除)
4.  如果第2和第3步没有删除节点，那么我们就需要向左子树进行递归删除
5.  如果第4步也没有删除节点，则应当向右子树进行递归删除

代码实现：

```java
@SuppressWarnings("all")
public class BinaryTreeDemo {
    public static void main(String[] args) {
        //创建一个二叉树
        BinaryTree binaryTree = new BinaryTree();
        //创建节点
        HeroNode root = new HeroNode(1,"宋江");
        HeroNode node = new HeroNode(2,"吴用");
        HeroNode node1 = new HeroNode(3,"卢俊义");
        HeroNode node2 = new HeroNode(4,"林冲");
        HeroNode node3 = new HeroNode(5,"关胜");

        //手动创建二叉树(后面使用递归方式创建二叉树)
        root.setLeft(node);
        root.setRight(node1);
        node1.setRight(node2);
        node1.setLeft(node3);

        binaryTree.setRoot(root);
        /* TODO
        //测试
        System.out.println("前序遍历");//1,2,3,5,4
        binaryTree.preOrder();
        //测试
        System.out.println("中序遍历");
        binaryTree.infixOrder();//2,1,3,5,4
        //测试
        System.out.println("后序遍历");
        binaryTree.postOrder();//5,4,3,2,1
        TODO */

        /* TODO
        System.out.println("-----------------前序遍历方式-----------------");
        HeroNode resNode = binaryTree.preOrderSearch(5);
        if(resNode != null){
            System.out.printf("查找到的信息为 [↓]\n>>>\t\tid:\t%d\n>>>\t\tname:\t%s\n",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d的信息",resNode.getNo());
        }

        System.out.println("-----------------中序遍历方式-----------------");
        HeroNode resNode1 = binaryTree.infixOrderSearch(5);
        if(resNode1 != null){
            System.out.printf("查找到的信息为 [↓]\n>>>\t\tid:\t%d\n>>>\t\tname:\t%s\n",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d的信息",resNode.getNo());
        }

        System.out.println("-----------------后序遍历方式-----------------");
        HeroNode resNode2 = binaryTree.postOrderSearch(5);
        if(resNode2 != null){
            System.out.printf("查找到的信息为 [↓]\n>>>\t\tid:\t%d\n>>>\t\tname:\t%s\n",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d的信息",resNode.getNo());
        }
        TODO */

        /* TODO
        //测试删除节点
        System.out.println("-----------------删除前，前序遍历情况-----------------");
        binaryTree.preOrder();//1,2,3,5,4
        binaryTree.delNode(5);
        System.out.println("-----------------删除后，前序遍历情况-----------------");
        binaryTree.preOrder();//1,2,3,4
        TODO */

        System.out.println("-----------------删除前，前序遍历情况-----------------");
        binaryTree.preOrder();//1,2,3,5,4
        binaryTree.delNode(3);
        System.out.println("-----------------删除后，前序遍历情况-----------------");
        binaryTree.preOrder();//1,2
    }
}
@SuppressWarnings("all")
//定义BinaryTree二叉树
class BinaryTree {
    private HeroNode root;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    //删除节点
    public void delNode(int no){
        if(this.root != null){
            //如果只有一个root节点，这里立即判断root是不是就是要删除节点
            if(this.root.getNo() == no){
                this.root = null;
                return;
            }else{
                //递归删除
                this.root.delNode(no);
            }
        }else{
            System.out.println("二叉树为空");
        }
    }

    //前序遍历，遍历是从根节点(root)开始的
    public void preOrder(){
        if(this.root != null){
            this.root.preOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }

    //中序遍历
    public void infixOrder(){
        if(this.root != null){
            this.root.infixOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }

    //后序遍历
    public void postOrder(){
        if(this.root != null){
            this.root.postOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }

    //前序遍历查找
    public HeroNode preOrderSearch(int no){
        if(root != null){
            return root.preOrderSearch(no);
        }else{
            return null;
        }
    }

    //中序遍历查找
    public HeroNode infixOrderSearch(int no){
        if(root != null){
            return root.infixOrderSearch(no);
        }else{
            return null;
        }
    }

    //后序遍历查找
    public HeroNode postOrderSearch(int no){
        if(root != null){
            return root.postOrderSearch(no);
        }else{
            return null;
        }
    }
}
@SuppressWarnings("all")
//创建HeroNode节点
class HeroNode {
    private int no;
    private String name;
    private HeroNode left;
    private HeroNode right;

    public static int countPre = 0;
    public static int countInfix = 0;
    public static int countPost = 0;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }

    //前序遍历方法
    public void preOrder(){
        //先输出父节点
        System.out.println(this);
        //递归向左子树前序遍历
        if(this.left != null){
            this.left.preOrder();
        }
        //递归向右子树前序遍历
        if(this.right != null){
            this.right.preOrder();
        }
    }

    //中序遍历方法
    public void infixOrder(){
        //递归向左子树中序遍历
        if(this.left != null){
            this.left.infixOrder();
        }
        //输出父节点
        System.out.println(this);
        //递归向右子树中序遍历
        if(this.right != null){
            this.right.infixOrder();
        }
    }

    //后序遍历方法
    public void postOrder(){
        //递归向左子树后序遍历
        if(this.left != null){
            this.left.postOrder();
        }
        //递归向右子树后序遍历
        if(this.right != null){
            this.right.postOrder();
        }
        //输出父节点
        System.out.println(this);
    }

    /**
     * 前序遍历查找
     * @param no 查找no
     * @return 如果找到就返回该Node，如果没有找到返回null
     */
    public HeroNode preOrderSearch(int no){
        countPre++;
        System.out.printf("进入前序遍历~~~\t一共遍历了>\t%d\t<次\n",countPre);
        //比较当前节点是不是要查找的节点
        if(this.no == no){
            return this;
        }
        //则判断当前节点的左子节点是否为空，如果不为空，则递归前序查找
        HeroNode heroNode = null;
        /**
         * 1.判断当前节点的左子节点是否为空，如果不为空，则递归前序查找
         * 2.如果做递归前序查找，找到节点，则返回
         */
        if(this.left != null){
            heroNode = this.left.preOrderSearch(no);
        }
        //说明左子树找到
        if(heroNode != null){
            return heroNode;
        }
        if(this.right != null){
            heroNode = this.right.preOrderSearch(no);
        }
        return heroNode;
    }

    //中序遍历查找
    public HeroNode infixOrderSearch(int no){
        HeroNode node = null;
        //判断左子节点是否为空，如果不为空则递归向左子节点遍历查找
        if(this.left != null){
            node = this.left.infixOrderSearch(no);
        }

        if(node != null){
            return node;
        }

        countInfix ++;
        System.out.printf("进入中序遍历~~~\t一共遍历了>\t%d\t<次\n",countInfix);
        //判断当前节点是否 为需要查找的
        if(this.no == no){
            return this;
        }

        //判断右子节点是否为空，如果不为空则递归向右子节点遍历查找
        if(this.right != null){
            node = this.right.infixOrderSearch(no);
        }

        //无论是否为空 都要返回递归节点
        return node;
    }

    //后序遍历查找
    public HeroNode postOrderSearch(int no){
        HeroNode node = null;
        //判断左子节点是否为空
        if(this.left != null){
            node = this.left.postOrderSearch(no);
        }

        //判断节点不为空则返回
        if(node != null){
            return node;
        }

        //判断右子节点是否为空
        if(this.right != null){
            node = this.right.postOrderSearch(no);
        }

        if(node != null){
            return node;
        }

        countPost ++;
        System.out.printf("进入后序遍历~~~\t一共遍历了>\t%d\t<次\n",countPost);

        //判断是否为需要查找的节点
        if(this.no == no){
            return this;
        }
        return node;
    }

    /**
     * 递归删除节点
     * 1.如果删除的节点是叶子节点，则删除该节点
     * 2.如果删除的节点是非叶子结点，则删除该子树
     * @param no
     */
    public void delNode(int no){
        /**
         * 思路：
         * 1.因为我们的二叉树是单向的，所以我们是判断当前节点的子节点是否需要删除节点，而不能去判断当前这个节点是不是需要删除节点
         * 2.如果当前节点的左子节点不为空，并且左子节点就是要删除节点，就将this.left = null;并且就返回(结束递归删除)
         * 3.如果当前节点的右子节点不为空，并且右子节点就是要删除节点，就将this.right = null;并且就返回(结束递归删除)
         * 4.如果第2和第3步没有删除节点，那么我们就需要向左子树进行递归删除
         * 5.如果第4步也没有删除节点，则应当向右子树进行递归删除
         */
         // 2.如果当前节点的左子节点不为空，并且左子节点就是要删除节点，就将this.left = null;并且就返回(结束递归删除)
        if(this.left != null && this.left.no == no){
            this.left = null;
            return;
        }
        // 3.如果当前节点的右子节点不为空，并且右子节点就是要删除节点，就将this.right = null;并且就返回(结束递归删除)
        if(this.right != null && this.right.no == no){
            this.right = null;
            return;
        }
        // 4.如果第2和第3步没有删除节点，那么我们就需要向左子树进行递归删除
         if(this.left != null){
             this.left.delNode(no);
         }
         // 5.如果第4步也没有删除节点，则应当向右子树进行递归删除
         if(this.right != null){
             this.right.delNode(no);
         }
    }
}
```

代码讲解：

![image-20230630174738464](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230630174738464.png)

### ==顺序存储==二叉树

**基本说明**：

从数据存储来看，==数组存储方式==和==树存储方式==可以==相互转换==，即==数组可以转换成树==，==树也可以转换成数组==，看下图：

![image-20230630192358216](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230630192358216.png)

**概述**：

顺序存储使用==数组的形式实现==；由于==非完全二叉树会导致数组中出现空缺==，==有的位置不能填上数字==，所以顺序存储二叉树通常情况下只考虑==完全二叉树==。

**原理**：

==顺序存储在数组中==是按照==第一层第二层依次往下存储==的，遍历方式也有==前序遍历==，==中序遍历==，==后序遍历==。

**要求**：

1.  上图的二叉树的节点，要求以数组的方式来存放arr:[1,2,3,4,5,6]
2.  要求在遍历数组arr时，仍然可以以  ==前序遍历==，==中序遍历==，==后序遍历==的方式完成节点的遍历

**顺序存储二叉树的特点**：

1.  <strong style="color:red">顺序二叉树通常只考虑</strong>==完全二叉树== 
2.  第n个元素的左子节点为`2*n+1` 
3.  第n个元素的右子节点为`2*n+2` 
4.  第n个元素的父节点为`(n-1)/2` 
5.  n：表示二叉树中的第几个元素的==下标==（==按0开始编号如下图所示==）

![image-20230630192711961](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230630192711961.png)

对上述的详细解释：

![image-20230630193107449](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230630193107449.png)

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/6/3019:39
 * @comment
 */

@SuppressWarnings("all")
public class ArrayBinaryTreeDemo {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5,6,7};
        ArrayBinaryTree arrayBinaryTree = new ArrayBinaryTree(arr);
        System.out.println("前序");
        arrayBinaryTree.preOrder();//1,2,4,5,3,6,7
        System.out.println();
        System.out.println("中序");
        arrayBinaryTree.infixOrder();
        System.out.println();
        System.out.println("后序");
        arrayBinaryTree.postOrder();
    }
}
//实现顺序存储二叉树遍历
class ArrayBinaryTree {
    private int[] arr;//存储数据节点的数据

    public ArrayBinaryTree(int[] arr) {
        this.arr = arr;
    }

    /**
     * 重载preOrder方法为了调用方便
     */
    public void preOrder(){
        this.preOrder(0);
    }

    public void infixOrder(){
        this.infixOrder(0);
    }

    public void postOrder(){
        this.postOrder(0);
    }

    /**
     * 编写方法，完成顺序存储二叉树的前序遍历
     * @param index 表示数组的下标
     */
    public void preOrder(int index){
        if(arr == null || arr.length == 0){
            System.out.println("二叉树为空");
            return;
        }

        System.out.printf("%d\t",arr[index]);

        //向左递归遍历
        if((2 * index + 1) < arr.length){
            preOrder(2 * index + 1);
        }

        //向右递归遍历
        if((2 * index + 2) < arr.length){
            preOrder(2 * index + 2);
        }

    }
	
    //顺序二叉树中序遍历
    public void infixOrder(int index){
        if(arr == null || arr.length == 0){
            System.out.println("二叉树为空");
            return;
        }
       
		 //向左递归遍历
        if((2 * index + 1) < arr.length){
            infixOrder((2 * index + 1));
        }
 
       //输出节点
        System.out.printf("%d\t",arr[index]);

       //向右递归遍历
        if((2 * index + 2) < arr.length){
            infixOrder(2 * index + 2);
        }
    }

    //顺序二叉树后序遍历
    public void postOrder(int index){
        if(arr == null || arr.length == 0){
            System.out.println("二叉树为空");
            return;
        }

        //向左递归遍历
        if((2 * index + 1) < arr.length){
            postOrder(2 * index + 1);
        }

        //向右递归遍历
        if((2 * index + 2) < arr.length){
            postOrder(2 * index + 2);
        }

        //输出节点
        System.out.printf("%d\t",arr[index]);
    }
}
```

运行结果：

无论是 前序遍历，中序遍历，后序遍历，它们的索引都对应那个值

```
---------------------前序遍历---------------------
>>> 当前的下标为0	元素为:1
>>> 当前的下标为1	元素为:2
>>> 当前的下标为3	元素为:4
>>> 当前的下标为4	元素为:5
>>> 当前的下标为2	元素为:3
>>> 当前的下标为5	元素为:6
>>> 当前的下标为6	元素为:7
---------------------中序遍历---------------------
>>> 当前的下标为3	元素为:4
>>> 当前的下标为1	元素为:2
>>> 当前的下标为4	元素为:5
>>> 当前的下标为0	元素为:1
>>> 当前的下标为5	元素为:6
>>> 当前的下标为2	元素为:3
>>> 当前的下标为6	元素为:7
---------------------后序遍历---------------------
>>> 当前的下标为3	元素为:4
>>> 当前的下标为4	元素为:5
>>> 当前的下标为1	元素为:2
>>> 当前的下标为5	元素为:6
>>> 当前的下标为6	元素为:7
>>> 当前的下标为2	元素为:3
>>> 当前的下标为0	元素为:1
```

### ==线索化==二叉树

**先看一个问题**：

将数列{1,3,6,8,10,14}构建成一颗二叉树，n^数组长度^+1=7

![image-20230630211156278](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230630211156278.png)

**问题分析**：

1.  当我们对上面的二叉树进行中序遍历时，数列为{8,3,10,1,14,6}
2.  但是6,8,10,14这几个节点的 左右指针，并没有完全的利用上
3.  如果我们希望充分的利用各个节点的左右指针，让各个节点可以指向自己的前后节点，怎么办？
4.  解决方案 - ==线索化二叉树== 

**线索化二叉树基本介绍** 

1.  n个节点的二叉链表中含有n^数组元素个数^+1 [公式2n-(n-1)=n+1] 个==空指针域==。利用==二叉链表中==的==空指针域==，存放指向<font style="color:skyblue">该节点</font>在<font style="color:red">某种遍历次序</font>下的==前驱==和==后续==节点的指针(这种附加的指针称为 "==线索==")

    ![image-20230701100223283](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230701100223283.png)

2.  这种加上了线索的二叉链表称为**线索链表**，相应的二叉树称为**线索二叉树**(Threaded BinaryTree)。根据线索性质的不同，线索二叉树可分为==前序线索二叉树==，==中序线索二叉树==，==后序线索二叉树== **三种**。
3.  一个节点的==前==一个节点，称为==前驱==节点
4.  一个节点的==后==一个节点，称为==后继==节点

**线索二叉树引用案例** 

**应用案例说明**：将下面的二叉树，进行**中序线索二叉树**。中序遍历的数列为 {8,3,10,1,14,6}

![image-20230701101834786](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230701101834786.png)

**思路分析**：

中序遍历的结果：{8,3,10,1,14,6}

![image-20230701101932874](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230701101932874.png)

-  8 的后继节点为 3
-  3 由于 **左右节点都有元素，不能线索化** 
-  10 的前驱节点为 3 ，后继节点为 1
-  1 不能线索化^root节点^ 
-  14 的前驱节点为 1，后继节点为 6
-  6 有左节点，有节点没有元素，不能线索化

<font style="color:red">**引出问题**</font>：根据中序遍历结果：{8,3,10,1,14,6} 在图中1的前驱是3后继是6可是中序遍历的==结果不一样== 

**说明：当线索化二叉树后，Node节点的 属性 left和 right，有如下情况**：

1.  left 指向的是左子树，也可能是指向的前驱节点，比如①节点left指向的左子树，而⑩节点的left指向的就是前驱节点
2.  right指向的是右子树，也可能是指向后继节点，比如 ①节点right指向的是右子树，而⑩节点的right指向的是后继节点

**代码实现**：

**下面的代码，有几个地方需要注意**：

-  HeroNode就是一个 简单的二叉树节点，不同的是多了两个type属性：

   -  **leftType**：左节点的类型：0 表示[==左子树==]，1 表示[==前驱节点==]
   -  **rightType**：右节点的类型：0 表示[==右子树==]，1 表示[==后继节点==]


-  <font style="color:red">**为什么需要**</font>？
   -  上面原理讲解了，left 或者 right 会有==两种身份==，需要一个==额外的属性来指明== 

-  threadNodes：线索化二叉树**方法** 

   -  是将一颗==二叉树==，进行==线索化标记==。==只是将可以线索化的节点进行赋值==。

```java
package cn.mrcode.study.dsalgtutorialdemo.datastructure.tree;

import org.junit.Test;

/**
 * 线索化二叉树
 */
public class ThreadedBinaryTreeTest {
    class HeroNode {
        public int id;
        public String name;
        public HeroNode left;
        public HeroNode right;
       
        /**
         * 左节点的类型：0：左子树，1：前驱节点
         */
        public int leftType;
        /**
         * 右节点的类型：0：右子树，1：后继节点
         */
        public int rightType;

        public HeroNode(int id, String name) {
            this.id = id;
            this.name = name;
        }

        @Override
        public String toString() {
            return "HeroNode{" +
                    "id=" + id +
                    ", name='" + name + '\'' +
                    '}';
        }
    }

    class ThreadedBinaryTree {
        public HeroNode root;
        public HeroNode pre; // 保留上一个节点

        /**
         * 线索化二叉树：以 中序的方式线索化
         * 重载方法方便调用而已
         */
        public void threadeNodes() {
            // 从 root 开始遍历，然后 线索化
            this.threadeNodes(root);
        }

        //进行中序线索化二叉树的方法
        private void threadeNodes(HeroNode node) {
            if (node == null) {
                return;
            }
            // 中序遍历顺序：先左、自己、再右
            // 向左递归到最后
            threadeNodes(node.left);
            // 难点就是在这里，如何线索化自己
            // 当自己的 left 节点为空，则设置为前驱节点
            if (node.left == null) {
                node.left = pre;
                node.leftType = 1;
            }

            // 因为要设置后继节点，只有回到自己的后继节点的时候，才能把自己设置为前一个的后继节点
            // 当前一个节点的 right 为空时，则需要自己是后继节点
            if (pre != null && pre.right == null) {
                pre.right = node;
                pre.rightType = 1;
            }

            // 数列： 1,3,6,8,10,14
            // 中序： 8,3,10,1,14,6
            // 这里最好结合图示的二叉树来看，容易理解
            // 因为中序遍历，先遍历左边，所以 8 是第一个输出的节点
            // 当 node = 8 时，pre 还没有被赋值过，则为空。这是正确的，因为 8 就是第一个节点
            // 当 8 处理完成之后，处理 3 时
            // 当 node = 3 时，pre 被赋值为 8 了。
            pre = node;
            threadeNodes(node.right);
        }
    }

    @Test
    public void threadeNodesTest() {
        HeroNode n1 = new HeroNode(1, "宋江");
        HeroNode n3 = new HeroNode(3, "无用");
        HeroNode n6 = new HeroNode(6, "卢俊");
        HeroNode n8 = new HeroNode(8, "林冲2");
        HeroNode n10 = new HeroNode(10, "林冲3");
        HeroNode n14 = new HeroNode(14, "林冲4");
        n1.left = n3;
        n1.right = n6;
        n3.left = n8;
        n3.right = n10;
        n6.left= n14;

        ThreadedBinaryTree tree = new ThreadedBinaryTree();
        tree.root = n1;

        tree.threadeNodes();

        // 验证：
        HeroNode left = n10.left;
        HeroNode right = n10.right;
        System.out.println("10 号节点的前驱节点：" + left.id);
        System.out.println("10 号节点的后继节点：" + right.id);
    }
}
```

输出结果：

```
10 号节点的前驱节点：3
10 号节点的后继节点：1
```

**如果看代码注释看不明白的话，下面来解释**：

-  线索化的时候，就是要按照 **中序遍历** 的顺序，去找可以线索化的节点

   **中序遍历顺序**：先左，自己，再右

   我们主要的代码是在 **自己这一块** 

-  确定前一个节点 pre

   这个 pre 很难理解，对照下图进行理解

![image-20201202230643686](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20201202230643686.f3691332.png)

```
// 数列： 1,3,6,8,10,14
// 中序： 8,3,10,1,14,6

// 因为中序遍历，先遍历左边，所以 8 是第一个输出的节点
// 当 node = 8 时，pre 还没有被赋值过，则为空。这是正确的，因为 8 就是第一个节点
// 当 8 处理完成之后，处理 3 时
// 当 node = 3 时，pre 被赋值为 8 了。
```

-  **设置前驱节点** 

   难点的讲解在于 pre，这里就简单了

   如果当 node = 8时，pre 还是 null，因为8 就是中序的第一个节点。因此 8 没有前驱

   如果当 node = 3时，pre = 8，那么3 是不符合线索化要求的，因为 8 是 3 的 left

-  **设置后继节点** 

   接上面的逻辑。

   如果当 node = 8 时，本来 该给 8 设置它的后继节点，但是此时根本就获取不到节点 3 ，因为节点是单向的。

   如果利用前一个节点 pre。

   当 node = 3 时， pre = 8 ，这时就可以为节点  8 处理它的后继节点了，**因为根据中序的顺序**，**左**，**自己**，**后**。**那么自己一定是前一个的后继**。只要前一个 right 为 null ，就符合线索化。

上述最难的 3 个点说明，请对照上图查看，先看一遍代码，再看说明。然后去debug你就懂了。

### ==遍历==线索化二叉树

**说明**：对前面的中序线索化的二叉树，进行==遍历== 

**分析**：因为线索化后，各个节点指向有变化，因此<strong style="color:red">原来的遍历方式不能使用</strong>,这时需要使用==新的方式==<strong>遍历线索化二叉树</strong>,各个节点可以通过==线型方式遍历==，因此==无需使用递归方式==，这样也==提高了遍历的效率==，<strong>遍历的==次序==应当和==中序遍历==保持==一致==</strong>。

<strong style="color:red">注意</strong>：使用原来的方式中序遍历线索化二叉树则会造成<font style="color:red">堆栈溢出</font>.

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/413:02
 * @comment
 */
@SuppressWarnings("all")
public class ThreadedBinaryTreeDemo01 {
    public static void main(String[] args) {
        HeroNode01 root = new HeroNode01(1, "tom");
        HeroNode01 node = new HeroNode01(3, "jack");
        HeroNode01 node1 = new HeroNode01(6, "smith");
        HeroNode01 node2 = new HeroNode01(8, "mary");
        HeroNode01 node3 = new HeroNode01(10, "king");
        HeroNode01 node4 = new HeroNode01(14, "dim");

        //手动创建二叉树
        root.left = node;
        root.right = node1;
        node.left = node2;
        node.right = node3;
        node1.left = node4;

        //测试线索化
        ThreadedBinaryTree01 threadedBinaryTree = new ThreadedBinaryTree01();
        //设置root节点
        threadedBinaryTree.setRoot(root);
        //调用线索化二叉树的方法进行线索化
        threadedBinaryTree.threadedNodes();

        //TODO 使用遍历二叉树的方式来中序遍历线索化后的二叉树则会造成堆栈溢出
//        threadedBinaryTree.infixOrder();

        //使用中序遍历线索化的方法来遍历线索化二叉树
        threadedBinaryTree.list();


    }
}
@SuppressWarnings("all")
//定义ThreadedBinaryTree 实现了线索化功能的二叉树
class ThreadedBinaryTree01 {
    public HeroNode01 root;

    /**
     * 为了实现线索化，需要创建指向当前节点的前驱节点的指针
     * 在递归进行线索化时，pre总是保留前一个节点
     */
    public HeroNode01 pre = null;
    public void setRoot(HeroNode01 root){
        this.root = root;
    }

    //重载方法 方便调用
    public void threadedNodes(){
        this.threadedNodes(root);
    }

    /**
     * 遍历线索化二叉树
     */
    public void list(){
        //定义一个变量，存储当前遍历的节点，从root开始
        HeroNode01 node = root;
        while(node != null){
            /**
             * 循环的找到leftType == 1的节点，第一个找到就是8节点
             * 后面随着遍历而变化，因为当leftType == 1时，说明该节点是按照线索化
             * 处理后的有效节点
             */
            while(node.leftType == 0){
                node = node.left;
            }
            //打印当前这个节点
            System.out.println(node);
            //如果当前节点的右指针指向的是后继节点，就一直输出
            while(node.rightType == 1){
                //获取到当前节点的后继节点
                node = node.right;
                System.out.println(node);
            }
            //替换这个遍历的节点
            node = node.right;
        }
    }

    /**
     * 编写对二叉树进行中序线索化的方法
     * @param node 就是当前需要线索化的节点
     */
    public void threadedNodes(HeroNode01 node){
        //如果node == null，不能进行线索化
        if(node == null){
            return;
        }
        //(一) 先线索化左子树
        threadedNodes(node.left);
        //(二) 线索化当前节点
        //处理当前节点的前驱节点
        if(node.left == null){
            //将当前节点的左指针执行前驱节点
            node.left = pre;
            //修改左指针类型标记为前驱节点
            node.leftType = 1;
        }
        //处理后继节点
        if(pre != null && pre.right == null){
            //将当前node节点设置pre右指针的后继节点
            pre.right = node;
            //修改右指针类型标记为前驱节点
            pre.rightType = 1;
        }
        //将当前节点赋值给pre节点(形成前后关系)
        pre = node;
        //(三) 先线索化右子树
        threadedNodes(node.right);
    }

    //中序遍历二叉树(TODO 注意：不能使用遍历线索化后的二叉树)
    public void infixOrder(){
        if(root != null){
            this.root.infixOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }
}
@SuppressWarnings("all")
//创建HeroNode节点
class HeroNode01 {
    public int no;
    public String name;

    //左节点
    public HeroNode01 left;
    //右节点
    public HeroNode01 right;

    /**
     * 说明：
     * 1.如果leftType == 0 表示指向的是左子树，如果1则表示指向前驱节点
     * 2.如果rightType == 0 表示指向的是右子树，如果1表示指向后继节点
     */
    public int leftType;
    public int rightType;

    public HeroNode01 (int no,String name){
        this.no = no;
        this.name = name;
    }

    //中序遍历
    public void infixOrder(){
        if(this.left != null){
            this.left.infixOrder();
        }
        System.out.println(this);
        if(this.right != null){
            this.right.infixOrder();
        }
    }

    @Override
    public String toString() {
        return "HeroNode01{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }
}
```

### ==前序遍历==线索化二叉树

二叉树 实现线索化二叉树

```java
public void threadedNodesPreOrder(){
   this.threadedNodesPreOrder(root);
}

public void threadedNodesPreOrder(HeroNode01 node){
   if(node == null){
      return;
   }

   if(node.left == null){
      node.left = pre;
      node.leftType = 1;
   }

   if(pre != null && pre.right == null){
      pre.right = node;
      pre.rightType = 1;
   }

   pre = node;

   if(node.leftType == 0){
      threadedNodesPreOrder(node.left);
   }

   if(node.rightType == 0){
      threadedNodesPreOrder(node.right);
   }
}
```

节点 实现：无

遍历代码线索化二叉树：

```java
public void listPreOrder(){
   HeroNode01 node = root;
   while(node != null){
      System.out.println(node);
      while(node.leftType == 0){
         node = node.left;
         System.out.println(node);
      }
      while(node.rightType == 1){
         node = node.right;
         System.out.println(node);
      }
      node = node.right;
   }
}
```

测试代码：

```java
HeroNode01 root = new HeroNode01(1, "tom");
HeroNode01 node = new HeroNode01(3, "jack");
HeroNode01 node1 = new HeroNode01(6, "smith");
HeroNode01 node2 = new HeroNode01(8, "mary");
HeroNode01 node3 = new HeroNode01(10, "king");
HeroNode01 node4 = new HeroNode01(14, "dim");

//手动创建二叉树
root.left = node;
root.right = node1;
node.left = node2;
node.right = node3;
node1.left = node4;

//测试线索化
ThreadedBinaryTree01 threadedBinaryTree = new ThreadedBinaryTree01();
//设置root节点
threadedBinaryTree.setRoot(root);
//调用前序线索化二叉树的方法进行线索化
threadedBinaryTree.threadedNodesPreOrder();
//使用前序遍历线索化的方法来遍历线索化二叉树
threadedBinaryTree.listPreOrder();
```

结果：

```
HeroNode01{no=1, name='tom'}
HeroNode01{no=3, name='jack'}
HeroNode01{no=8, name='mary'}
HeroNode01{no=10, name='king'}
HeroNode01{no=6, name='smith'}
HeroNode01{no=14, name='dim'}
```

### ==中序遍历==线索化二叉树

二叉树 实现线索化二叉树

```java
public void threadedInfixNodes(){
   this.threadedInfixNodes(root);
}

public void threadedInfixNodes(HeroNode01 node){
   //如果node == null，不能进行线索化
   if(node == null){
      return;
   }
   //(一) 先线索化左子树
   threadedInfixNodes(node.left);
   //(二) 线索化当前节点
   //处理当前节点的前驱节点
   if(node.left == null){
      //将当前节点的左指针执行前驱节点
      node.left = pre;
      //修改左指针类型标记为前驱节点
      node.leftType = 1;
   }
   //处理后继节点
   if(pre != null && pre.right == null){
      //将当前node节点设置pre右指针的后继节点
      pre.right = node;
      //修改右指针类型标记为前驱节点
      pre.rightType = 1;
   }
   //将当前节点赋值给pre节点(形成前后关系)
   pre = node;
   //(三) 先线索化右子树
   threadedInfixNodes(node.right);
}
```

节点 实现 ：无

遍历线索化二叉树

```java
public void listInfoxOrder(){
   //定义一个变量，存储当前遍历的节点，从root开始
   HeroNode01 node = root;
   while(node != null){
      /**
             * 循环的找到leftType == 1的节点，第一个找到就是8节点
             * 后面随着遍历而变化，因为当leftType == 1时，说明该节点是按照线索化
             * 处理后的有效节点
             */
      while(node.leftType == 0){
         node = node.left;
      }
      //打印当前这个节点
      System.out.println(node);
      //如果当前节点的右指针指向的是后继节点，就一直输出
      while(node.rightType == 1){
         //获取到当前节点的后继节点
         node = node.right;
         System.out.println(node);
      }
      //替换这个遍历的节点
      node = node.right;
   }
}
```

测试代码：

```java
HeroNode01 root = new HeroNode01(1, "tom");
HeroNode01 node = new HeroNode01(3, "jack");
HeroNode01 node1 = new HeroNode01(6, "smith");
HeroNode01 node2 = new HeroNode01(8, "mary");
HeroNode01 node3 = new HeroNode01(10, "king");
HeroNode01 node4 = new HeroNode01(14, "dim");

//手动创建二叉树
root.left = node;
root.right = node1;
node.left = node2;
node.right = node3;
node1.left = node4;

//测试线索化
ThreadedBinaryTree01 threadedBinaryTree = new ThreadedBinaryTree01();
//设置root节点
threadedBinaryTree.setRoot(root);
//调用中序线索化二叉树的方法进行线索化
threadedBinaryTree.threadedInfixNodes();
//使用中序遍历线索化的方法来遍历线索化二叉树
threadedBinaryTree.listInfoxOrder();
```

测试结果：

```java
HeroNode01{no=8, name='mary'}
HeroNode01{no=3, name='jack'}
HeroNode01{no=10, name='king'}
HeroNode01{no=1, name='tom'}
HeroNode01{no=14, name='dim'}
HeroNode01{no=6, name='smith'}
```

### ==后序遍历==线索化二叉树

二叉树  实现线索化二叉树

```java
public void threadedNodesPostOrder(){
   this.threadedNodesPostOrder(root);
}

public void threadedNodesPostOrder(HeroNode01 node){
   if (node == null) {
      return;
   }
   //线索化左子树
   threadedNodesPostOrder(node.left);

   //线索化右子树
   threadedNodesPostOrder(node.right);
   //线索化当前节点
   if (node.left == null) {
      node.left = pre;
      node.leftType = 1;
   }
   if (pre != null && pre.right == null) {
      pre.right = node;
      pre.rightType = 1;
   }
   pre = node;
}

//前序遍历添加父节点
public void preOrderAddPar() {
   if (this.root != null) {
      this.root.preOrderAddPar();
   } else {
      System.out.println("二叉树为空");
   }
}
```

节点  实现：添加父节点

```java
@SuppressWarnings("all")
//创建HeroNode节点
class HeroNode01 {
    public int no;
    public String name;

    //左节点
    public HeroNode01 left;
    //右节点
    public HeroNode01 right;

    /**
     * 说明：
     * 1.如果leftType == 0 表示指向的是左子树，如果1则表示指向前驱节点
     * 2.如果rightType == 0 表示指向的是右子树，如果1表示指向后继节点
     */
    public int leftType;
    public int rightType;

    //添加父节点
    public HeroNode01 parent;

    public HeroNode01 getParent() {
        return parent;
    }

    public void setParent(HeroNode01 parent) {
        this.parent = parent;
    }

    public HeroNode01 (int no,String name){
        this.no = no;
        this.name = name;
    }

    //中序遍历
    public void infixOrder(){
        if(this.left != null){
            this.left.infixOrder();
        }
        System.out.println(this);
        if(this.right != null){
            this.right.infixOrder();
        }
    }

    //前序遍历添加父节点
    public void preOrderAddPar() {
        while (this.left != null) {
            this.left.setParent(this);
            break;
        }
        while (this.right != null) {
            this.right.setParent(this);
            break;
        }

        if (this.left != null) {//2.向左遍历
            this.left.preOrderAddPar();
        }
        if (this.right != null) {//3.向右遍历
            this.right.preOrderAddPar();
        }
    }

    @Override
    public String toString() {
        return "HeroNode01{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }
}
```

遍历线索化二叉树：

```java
public void listPostOrder(){
   HeroNode01 node = root;
   while(node != null && node.leftType !=1) {
      node = node.left;
   }

   HeroNode01 pre = null;
   while(node != null) {
      //右节点是线索
      if (node.rightType == 1) {
         System.out.println(node);
         pre = node;
         node = node.right;

      } else {
         //如果上个处理的节点是当前节点的右节点
         if (node.right == pre) {
            System.out.println(node);
            if (node == root) {
               return;
            }
            pre = node;
            node = node.getParent();

         } else {    //如果从左节点的进入则找到有子树的最左节点
            node = node.right;
            while (node != null && node.leftType !=1) {
               node = node.left;
            }
         }
      }
   }
}
```

测试代码：

```java
HeroNode01 root = new HeroNode01(1, "tom");
HeroNode01 node = new HeroNode01(3, "jack");
HeroNode01 node1 = new HeroNode01(6, "smith");
HeroNode01 node2 = new HeroNode01(8, "mary");
HeroNode01 node3 = new HeroNode01(10, "king");
HeroNode01 node4 = new HeroNode01(14, "dim");

//手动创建二叉树
root.left = node;
root.right = node1;
node.left = node2;
node.right = node3;
node1.left = node4;

//测试线索化
ThreadedBinaryTree01 threadedBinaryTree = new ThreadedBinaryTree01();
//设置root节点
threadedBinaryTree.setRoot(root);
//为节点遍历添加父节点
threadedBinaryTree.preOrderAddPar();
//调用后序线索化二叉树的方法进行线索化
threadedBinaryTree.threadedNodesPostOrder();
//使用后序遍历线索化的方法来遍历线索化二叉树
threadedBinaryTree.listPostOrder();
```

测试结果：

```
HeroNode01{no=8, name='mary'}
HeroNode01{no=10, name='king'}
HeroNode01{no=3, name='jack'}
HeroNode01{no=14, name='dim'}
HeroNode01{no=6, name='smith'}
HeroNode01{no=1, name='tom'}
```

### 完整代码：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/413:02
 * @comment
 */
@SuppressWarnings("all")
public class ThreadedBinaryTreeDemo01 {
    public static void main(String[] args) {
        HeroNode01 root = new HeroNode01(1, "tom");
        HeroNode01 node = new HeroNode01(3, "jack");
        HeroNode01 node1 = new HeroNode01(6, "smith");
        HeroNode01 node2 = new HeroNode01(8, "mary");
        HeroNode01 node3 = new HeroNode01(10, "king");
        HeroNode01 node4 = new HeroNode01(14, "dim");

        //手动创建二叉树
        root.left = node;
        root.right = node1;
        node.left = node2;
        node.right = node3;
        node1.left = node4;

        //测试线索化
        ThreadedBinaryTree01 threadedBinaryTree = new ThreadedBinaryTree01();
        //设置root节点
        threadedBinaryTree.setRoot(root);
        //为节点遍历添加父节点
        threadedBinaryTree.preOrderAddPar();

        //调用中序线索化二叉树的方法进行线索化
//        threadedBinaryTree.threadedInfixNodes();
        //调用前序线索化二叉树的方法进行线索化
//        threadedBinaryTree.threadedNodesPreOrder();
        //调用后序线索化二叉树的方法进行线索化
        threadedBinaryTree.threadedNodesPostOrder();

        //TODO 使用遍历二叉树的方式来中序遍历线索化后的二叉树则会造成堆栈溢出
//        threadedBinaryTree.infixOrder();

        //使用中序遍历线索化的方法来遍历线索化二叉树
//        threadedBinaryTree.listInfoxOrder();
        //使用前序遍历线索化的方法来遍历线索化二叉树
//        threadedBinaryTree.listPreOrder();
        //使用后序遍历线索化的方法来遍历线索化二叉树
        threadedBinaryTree.listPostOrder();
    }
}
@SuppressWarnings("all")
//定义ThreadedBinaryTree 实现了线索化功能的二叉树
class ThreadedBinaryTree01 {
    public HeroNode01 root;

    /**
     * 为了实现线索化，需要创建指向当前节点的前驱节点的指针
     * 在递归进行线索化时，pre总是保留前一个节点
     */
    public HeroNode01 pre = null;
    public void setRoot(HeroNode01 root){
        this.root = root;
    }

    public void threadedInfixNodes(){
        this.threadedInfixNodes(root);
    }

    public void threadedNodesPreOrder(){
        this.threadedNodesPreOrder(root);
    }

    public void threadedNodesPostOrder(){
        this.threadedNodesPostOrder(root);
    }

    /**
     * 前序遍历线索化二叉树
     */
    public void listPreOrder(){
        HeroNode01 node = root;
        while(node != null){
            System.out.println(node);
            while(node.leftType == 0){
                node = node.left;
                System.out.println(node);
            }
            while(node.rightType == 1){
                node = node.right;
                System.out.println(node);
            }
            node = node.right;
        }
    }

    /**
     * 中序遍历线索化二叉树
     */
    public void listInfoxOrder(){
        //定义一个变量，存储当前遍历的节点，从root开始
        HeroNode01 node = root;
        while(node != null){
            /**
             * 循环的找到leftType == 1的节点，第一个找到就是8节点
             * 后面随着遍历而变化，因为当leftType == 1时，说明该节点是按照线索化
             * 处理后的有效节点
             */
            while(node.leftType == 0){
                node = node.left;
            }
            //打印当前这个节点
            System.out.println(node);
            //如果当前节点的右指针指向的是后继节点，就一直输出
            while(node.rightType == 1){
                //获取到当前节点的后继节点
                node = node.right;
                System.out.println(node);
            }
            //替换这个遍历的节点
            node = node.right;
        }
    }


    public void listPostOrder(){
        HeroNode01 node = root;
        while(node != null && node.leftType !=1) {
            node = node.left;
        }

        HeroNode01 pre = null;
        while(node != null) {
            //右节点是线索
            if (node.rightType == 1) {
                System.out.println(node);
                pre = node;
                node = node.right;

            } else {
                //如果上个处理的节点是当前节点的右节点
                if (node.right == pre) {
                    System.out.println(node);
                    if (node == root) {
                        return;
                    }
                    pre = node;
                    node = node.getParent();

                } else {    //如果从左节点的进入则找到有子树的最左节点
                    node = node.right;
                    while (node != null && node.leftType !=1) {
                        node = node.left;
                    }
                }
            }
        }
    }

    /**
     * 编写对二叉树进行中序线索化的方法
     * @param node 就是当前需要线索化的节点
     */
    public void threadedInfixNodes(HeroNode01 node){
        //如果node == null，不能进行线索化
        if(node == null){
            return;
        }
        //(一) 先线索化左子树
        threadedInfixNodes(node.left);
        //(二) 线索化当前节点
        //处理当前节点的前驱节点
        if(node.left == null){
            //将当前节点的左指针执行前驱节点
            node.left = pre;
            //修改左指针类型标记为前驱节点
            node.leftType = 1;
        }
        //处理后继节点
        if(pre != null && pre.right == null){
            //将当前node节点设置pre右指针的后继节点
            pre.right = node;
            //修改右指针类型标记为前驱节点
            pre.rightType = 1;
        }
        //将当前节点赋值给pre节点(形成前后关系)
        pre = node;
        //(三) 先线索化右子树
        threadedInfixNodes(node.right);
    }

    public void threadedNodesPreOrder(HeroNode01 node){
        if(node == null){
            return;
        }

        if(node.left == null){
            node.left = pre;
            node.leftType = 1;
        }

        if(pre != null && pre.right == null){
            pre.right = node;
            pre.rightType = 1;
        }

        pre = node;

        if(node.leftType == 0){
            threadedNodesPreOrder(node.left);
        }

        if(node.rightType == 0){
            threadedNodesPreOrder(node.right);
        }
    }


    public void threadedNodesPostOrder(HeroNode01 node){
        if (node == null) {
            return;
        }
        //线索化左子树
        threadedNodesPostOrder(node.left);

        //线索化右子树
        threadedNodesPostOrder(node.right);
        //线索化当前节点
        if (node.left == null) {
            node.left = pre;
            node.leftType = 1;
        }
        if (pre != null && pre.right == null) {
            pre.right = node;
            pre.rightType = 1;
        }
        pre = node;
    }

    //前序遍历添加父节点
    public void preOrderAddPar() {
        if (this.root != null) {
            this.root.preOrderAddPar();
        } else {
            System.out.println("二叉树为空");
        }
    }

    //中序遍历二叉树(TODO 注意：不能使用遍历线索化后的二叉树)
    public void infixOrder(){
        if(root != null){
            this.root.infixOrder();
        }else{
            System.out.println("二叉树为空");
        }
    }
}
@SuppressWarnings("all")
//创建HeroNode节点
class HeroNode01 {
    public int no;
    public String name;

    //左节点
    public HeroNode01 left;
    //右节点
    public HeroNode01 right;

    /**
     * 说明：
     * 1.如果leftType == 0 表示指向的是左子树，如果1则表示指向前驱节点
     * 2.如果rightType == 0 表示指向的是右子树，如果1表示指向后继节点
     */
    public int leftType;
    public int rightType;

    public HeroNode01 parent;

    public HeroNode01 getParent() {
        return parent;
    }

    public void setParent(HeroNode01 parent) {
        this.parent = parent;
    }

    public HeroNode01 (int no,String name){
        this.no = no;
        this.name = name;
    }

    //中序遍历
    public void infixOrder(){
        if(this.left != null){
            this.left.infixOrder();
        }
        System.out.println(this);
        if(this.right != null){
            this.right.infixOrder();
        }
    }

    //前序遍历添加父节点
    public void preOrderAddPar() {
        while (this.left != null) {
            this.left.setParent(this);
            break;
        }
        while (this.right != null) {
            this.right.setParent(this);
            break;
        }

        if (this.left != null) {//2.向左遍历
            this.left.preOrderAddPar();
        }
        if (this.right != null) {//3.向右遍历
            this.right.preOrderAddPar();
        }
    }

    @Override
    public String toString() {
        return "HeroNode01{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }
}
```

## 树结构实际应用

### 堆排序

**堆排序基本介绍**

1.  堆排序是利用**堆**这种数据结构而设计的一种==排序算法==，堆排序是一种==选择排序==,它的==最坏，最好，平均时间复杂度均为O(nlogn)，它也是不稳定排序==。
2.  堆是具有以下性质的==完全二叉树==：==每个结点的值==都==大于==或==等于==其==左右孩子结点的值==，称为==大顶堆== 
    -  <strong style="color:red">注意</strong>: 没有要求==结点==的==左孩子的值==和==右孩子的值==的==大小关系==。
3.  ==每个结点的值==都==小于==或==等于==其==左右孩子结点的值==，称为==小顶堆== 

#### 大顶堆举例说明

![image-20230704170408717](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704170408717.png)

我们对堆中的结点按层进行编号，映射到数组中就是下面这个样子: 

![image-20230704170435750](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704170435750.png)

**大顶堆特点**：`arr[i] >= arr[2\*i+1] && arr[i] >= arr[2\*i+2]` 

-  i 对应第几个节点，i从0开始编号

#### 小顶堆举例说明

![image-20230704170624790](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704170624790.png)

**小顶堆**：`arr[i] <= arr[2\*i+1] && arr[i] <= arr[2\*i+2]` 

-  i 对应第几个节点，i从0开始编号

6.  一般==升序采用大顶堆==，==降序采用小顶堆== 

**堆排序基本思想** 

1.  将待排序序列构造成一个大顶堆
2.  此时，整个序列的最大值就是堆顶的根节点。
3.  将其与末尾元素进行交换，此时末尾就为最大值。
4.  然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值<font style="color:red">^第二小的值^</font>。如此反复执行，便能得到一个有序序列了。

可以看到在构建大顶堆的过程中，元素的个数逐渐减少，最后就得到一个有序序列了.

**堆排序步骤图解说明** 

**步骤一 构造初始堆。将给定无序序列构造成一个大顶堆（一般升序采用大顶堆，降序采用小顶堆)**。  

1.  假设给定无序序列结构如下

![image-20230704171102845](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704171102845.png)

2.  此时我们从最后一个非叶子结点开始（叶结点自然不用调整，第一个非叶子结点 `arr.length/2-1=5/2-1=1`，也就是下面的 6 结点），从左至右，从下至上进行调整。

![image-20230704171121981](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704171121981.png)

3.  找到第二个非叶节点4，由于[4,9,8]中9元素最大，4和9交换。

![image-20230704171144931](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704171144931.png)

4.  这时，交换导致了子根[4,5,6]结构混乱，继续调整，[4,5,6]中6最大，交换4和6。

![image-20230704171207684](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704171207684.png)

此时，我们就将一个无序序列构造成了一个大顶堆。

**步骤二 将堆顶元素与末尾元素进行交换，使末尾元素最大。然后继续调整堆，再将堆顶元素与末尾元素交换，得到第二大元素。如此反复进行交换、重建、交换**。

1.  将堆顶元素9和末尾元素4进行交换

![image-20230704171233081](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704171233081.png)

2.  重新调整结构，使其继续满足堆定义

![image-20230704171255536](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704171255536.png)

3.  再将堆顶元素8与末尾元素5进行交换，得到第二大元素8.

![image-20230704171316338](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704171316338.png)

4.  后续过程，继续进行调整，交换，如此反复进行，最终使得整个序列有序

![image-20230704171334035](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230704171334035.png)

**再简单总结下堆排序的基本思路：** 

1.  **将无序序列构建成一个堆，根据升序降序需求选择大顶堆或小顶堆;**
2.  **将堆顶元素与末尾元素交换，将最大元素"沉"到数组末端;**
3.  **重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整+交换步骤，直到整个序列有序。**

要求：给你一个数组 {4,6,8,5,9} , 要求使用堆排序法，将数组升序排序。

**代码实现**：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/416:10
 * @comment
 */
public class HeapSort {
    public static void main(String[] args) {
        int[] arr = {4,6,8,5,9};
        heapSort(arr);
    }
    //堆排序方法
    public static void heapSort(int[] arr){
        int temp = 0;
//        adjustHeap(arr,1,arr.length);
//        System.out.println("第一次"+ Arrays.toString(arr));//4, 9, 8, 5, 6
//        adjustHeap(arr,0,arr.length);
//        System.out.println("第二次"+Arrays.toString(arr));//9,6,8,5,4
        //将无序序列构建成一个堆，根据升序降序需求选择大顶堆或小顶堆
        for(int i = arr.length / 2 - 1;i >= 0;i --){
            adjustHeap(arr,i,arr.length);
        }
        /**
         * 2.将堆顶元素与末尾元素交换，将最大元素"沉"到数组末端
         * 3.重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整+交换步骤，直到
         * 整个序列有序
         */
        for(int j = arr.length - 1;j > 0;j --){
            temp = arr[j];
            arr[j] = arr[0];
            arr[0] = temp;
            adjustHeap(arr,0,j);
        }
        System.out.println(Arrays.toString(arr));
    }

    /**
     * 将一个数组(二叉树)，调整成一个大顶堆
     * 功能：完成将以i对应的非叶子结点的树调整成大顶堆
     * 举例：int[] arr = {4,6,8,5,9}; => i = 1 => adjustHeap => 得到 {4,9,8,5,6}
     * 如果我们再次调用adjustHeap传入的是i = 0 => 得到 {4,9,8,5,6} => {9,6,8,5,4}
     * @param arr 待调整的数组
     * @param i 数组中的下标
     * @param length 大小
     */
    public static void adjustHeap(int[] arr,int i,int length){
        //先取出当前元素的值，保存在临时变量
        int temp = arr[i];
        /**
         * 开始调整
         * 说明：
         * 1.k = i * 2 + 1 k是i节点的左子节点
         */
        for(int k = i * 2 + 1;k < length;k = k * 2 + 1){
            //说明 左子节点的值小于右子节点的值
            if(k + 1 < length && arr[k] < arr[k + 1]){
                //k 指向右子节点
                k ++;
            }

            //如果子节点大于父节点
            if(arr[k] > temp){
                //把较大的值赋值给当前节点
                arr[i] = arr[k];
                //i 指向 k,继续循环比较
                i = k;
            }else{
                break;
            }
        }
        //当for循环结束后，我们已经将i为父节点的树的最大值，放在了 最顶(局部)
        //将temp值放到调整后的位置
        arr[i] = temp;
    }
}
```
