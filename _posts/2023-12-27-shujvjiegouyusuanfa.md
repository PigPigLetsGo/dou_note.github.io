---
layout: post
title: 数据结构与算法
categories: Algorithm
description: 一种使用递归思路实现的简易全排列算法。
keywords: 算法, 全排列, 递归
---

## 数据结构和算法的概述

**数据结构和算法的关系**。

-  ==数据(Data)结构(Structure)==是一门研究组织==数据==方式的学科，有了编程语言也就有了数据结构，学好数据结构可以编写出更加漂亮，更加有效率的代码。
-  要学好数据结构就要多多考虑如何将生活中遇到的问题，用程序去实现解决。
-  程序 = ==数据结构== + 算法
-  ==数据结构==是==算法的基础==，换言之，想要==学好算法==，需要把==数据结构学到位==。
-  思维概括：学数据结构和算法只是打个基础，并不是学会了就能写出好的程序，比方说你会数组那么就会归并算法了吗？并不会所以什么东西都是需要学的不会的就学。在学会一个算法的基础上去营造一个新的算法就是进步

**数据结构和算法的联系**：

-  简单的一些问题就用数据结构
-  比较复杂的问题就实用数据结构 + 算法

1.  修路问题 => 最小生成树(加权值)[数据结构] + 普利姆算法
2.  最短路径问题 => 图 + 费罗伊德算法
3.  汉若塔 => 分支算法
4.  八皇后问题 => 回溯法

## 线性结构和非线性结构

### 线性结构

1.  线性结构作为最常用的数据结构，其特点是==数据元素之间存在一对一的线性关系==。
2.  线性结构有两种不同的存储结构，即==顺序存储结构(**数组**)==和==链式存储结构(**链表**)==。顺序存储的线性表称为顺序表，顺序表中的==存储元素(地址)是连续==的。
3.  链式存储的线性表称为链表，链表中的==存储元素不一定是连续==的，元素节点中存放数据元素以及相邻元素的地址信息
4.  线性结构常见的有：==数组，队列，链表和栈==。

### 非线性结构

非线性结构，数学用语，其[逻辑](https://baike.baidu.com/item/逻辑/543)特征是一个结点元素可能有多个直接前驱和多个直接后继。

非线性结构包括：二维数组，多维数组，广义表，树结构，图结构

# 数据结构和算法

## 稀疏数组和队列

### 稀疏sparsearray数组

**先看一个实际的需求** 。

>  编写的五子棋程序中，有==存盘退出==和==续上盘==的功能。

![1](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161855953.png)

**分析问题**：

因为该二维数组的很多值是默认值0，因此记录了==很多没有意义的数据== -> 使用==稀疏数组==对二维数组进行压缩。

**基本介绍**。

当一个数组中大部分元素为0，或者为同一个值的数组时，可以使用稀疏数组来保存该数组。

**稀疏数组的处理方法是**：

1.  记录数组==一共有几行几列，有多少个不同==的值。
2.  把具有不同值的元素的行列及值记录在一个小规模的数组中，从而==缩小程序==的规模。

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161855598.png)

二维数组 转 稀疏数组的思路：

1.  遍历 原始的二维数组，得到有效数据的个数sum
2.  根据sum就可以创建稀疏数组`sparse int[sum + 1][3]` 
3.  将二维数组的有效数据存入到稀疏数组

稀疏 数组转原始的二维数组的思路：

1.  先读取稀疏数组的第一行，根据第一行的数据，创建原始的二维数组，比如上面的`chessArr2 = int[11][11]` 
2.  再读取稀疏数组后几行的数据，并赋给 原始的二维数组即可。

![1](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161855360.png)

代码实现：

```java
public static void main(String ... args){
   //创建二维数组 棋盘，地图
   int[][] arr = new int[11][11];
   arr[1][2] = 1;
   arr[2][3] = 2;
   arr[5][5] = 2;
   //输出二维数组中的数据
   System.out.println("原始的二维数组");
   for(int[] item:arr){
      for(int i:item){
         System.out.printf("%d\t",i);
      }
      System.out.println();
   }

   //将二维数组 转 稀疏数组的思路
   //1.先遍历二维数组，得到非0数据个数
   int count = 0;
   for(int i = 0;i < arr.length;i ++){
      for(int j = 1;j < arr[i].length;j ++){
         if(arr[i][j] != 0){
            count ++;
         }
      }
   }
   System.out.println("二维数组中有效数据的个数："+count);
   int[][] sparse = new int[count+1][3];
   //给稀疏数组赋值
   for(int i = 0;i < arr.length;i ++){
      sparse[0][0] = arr.length;
      sparse[0][1] = arr[i].length;
      sparse[0][2] = count;
   }
   //遍历二维数组，将非0的值存放到稀疏数组中
   //定义一个计数器来记录第几个非0的数据
   int count1 = 0;
   for(int i = 0;i < arr.length;i ++){
      for(int j = 0;j < arr.length;j ++){
         if(arr[i][j] != 0){
            count1 ++;
            sparse[count1][0] = i;
            sparse[count1][1] = j;
            sparse[count1][2] = arr[i][j];
         }
      }
   }

   //输出稀疏数组的值
   for(int[] item:sparse){
      for(int i:item){
         System.out.printf("%d\t",i);
      }
      System.out.println();
   }

   //将稀疏数组 恢复为 原始的二维数组
   //1.先读取稀疏数组的第一行，根据第一行的数据，创建原始的二维数组
   int[][] arr1 = new int[sparse[0][0]][sparse[0][1]];

   //输出的二维数组的数据全都是为0的
   //for(int[] item:arr1){
   //    for(int i:item){
   //        System.out.printf("%d\t",i);
   //    }
   //    System.out.println();
   //}

   //2.再读取稀疏数组后几行的数据(从第二行开始)，并赋给 原始的二维数组即可
   for(int i = 1;i < sparse.length;i ++){
      //sparse第i行的第2列是存储二维数据的值的位置
      arr1[sparse[i][0]][sparse[i][1]] = sparse[i][2];
   }

   //输出的二维数组的数据
   for(int[] item:arr1){
      for(int i:item){
         System.out.printf("%d\t",i);
      }
      System.out.println();
   }
}
```

运行结果：

```
原始的二维数组
0	0	0	0	0	0	0	0	0	0	0	
0	0	1	0	0	0	0	0	0	0	0	
0	0	0	2	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	2	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	


二维数组中有效数据的个数：3


输出稀疏数组的值
11	11	3	
1	2	1	
2	3	2	
5	5	2	


输出稀疏数组恢复后的数据
0	0	0	0	0	0	0	0	0	0	0	
0	0	1	0	0	0	0	0	0	0	0	
0	0	0	2	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	2	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
```

## 队列

银行排队的案例：

![1](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161855839.png)

队列介绍：

-  队列是一个==有序列表==，可以用==数组==或是==链表==来实现。
-  遵循==先入先出==的原则，即：==先存入队列的数据，要先取出，后存入的要后取出==。
-  示意图(使用数组模拟队列示意图)：

![1q](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161855969.png)

### 数组模拟队列

 - 队列本身是有序列表，若使用数组的结构来存储队列的数据，则队列数组的声明如下图，其中maxSize是该队列的最大容量。
 - 因为队列的输出，输入是分别从前后端来处理，因此需要两个变量**front**及**rear**分别**记录队列全后端**的下标，front会随着数据输出而改变，而rear则是随着数据输入而改变。如下图所示：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161855300.png)

- 当我们将数据存入队列时称为"addQueue"，"addQueue"的处理需要有两个步骤：**思路分析**

1. 将尾指针往后移：rear+1,当front == rear [空]
2. 若尾指针rear小于队列的最小下标 maxSize - 1,则将数据存入rear所指的数组元素中，否则无法存入数据。rear == maxSize - 1 [队列满]。

代码实现：

```java
public class ArrayQueueDemo {
    public static void main(String ... args){
        Scanner sc = new Scanner(System.in);
        //测试一把
        //初始化队列
        ArrayQueue arrayQueue = new ArrayQueue(3);
        //接受用户输入
        char event_key = ' ';
        boolean loop = true;
        while(loop){
            //显示菜单
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出程序");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出数据");
            System.out.println("h(head): 查看队列头的数据");
            System.out.println("============================");
            event_key = sc.next().charAt(0);
            switch(event_key){
                case 's':
                    arrayQueue.showQueue();
                    break;
                case 'e':
                    sc.close();
                    loop = false;
                    break;
                case 'a':
                    System.out.println("请添加数据到队列中");
                    int num = sc.nextInt();
                    arrayQueue.addQueue(num);
                    break;
                case 'g':
                    try{
                        int value = arrayQueue.getQueue();
                        System.out.println(value);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try{
                        int head = arrayQueue.headQueue();
                        System.out.println(">> "+head);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                default:
                    System.out.println("请选择存在项");
                    break;
            }
        }
        System.out.println(">> 程序退出");
    }
}
//使用数组模拟队列-编写一个ArrayQueue类
class ArrayQueue {
    //表示数组的最大容量
    private int maxSize;
    //队列头
    private int front;
    //队列尾
    private int rear;
    //该数组用于存放数据，模拟队列
    private int[] arr;

    //创建队列的构造器
    public ArrayQueue (int maxSize) {
        this.maxSize = maxSize;
        arr = new int[maxSize];
        //指向队列头部，分析出front是指向队列头的前一个位置
        front = -1;
        //指向队列尾，指向队列尾的数据(即就是队列最后一个数据)
        rear = -1;
    }

    //判断队列是否满
    public boolean isFull(){
        return rear == maxSize - 1;
    }

    //判断队列是否为空
    public boolean isEmpty(){
        return rear == front;
    }

    //添加数据到队列
    public void addQueue(int n){
        //判断队列是否满
        if(isFull()){
            System.out.println("队列满了不能加入数据了");
            return;
        }
        //rear向后移
        rear++;
        //向数组中添加数据
        arr[rear] = n;
    }

    //获取队列数据，出队列
    public int getQueue(){
        //判断队列是否为空
        if(isEmpty()){
            //抛出异常会导致程序停止相当于return终止程序了所以下面不能再写代码了
            throw new RuntimeException("队列为空");
        }
        //front向前移
        front++;
        //取出当前front位置的数组元素
        int result = arr[front];
        arr[front] = 0;
        return result;
    }

    //显示队列的所有数据
    public void showQueue(){
        //判断队列是否为空
        if(isEmpty()){
            System.out.println("数组为空，没有数据");
            return;
        }
        //数组遍历元素
        for(int i = 0;i < arr.length;i ++){
            System.out.printf(">> arr[%d]=%d\n",i,arr[i]);
        }
    }

    //显示队列头数据，注意：不能取出数据
    public int headQueue(){
        //判断队列是否为空
        if(isEmpty()){
            throw new RuntimeException("队列为空");
        }
        return arr[front + 1];
    }
}
```

**问题分析和优化**：

1. 目前数组使用一次就不能用，没有达到复用的效果
2. 将这个数组使用算法，改进成一个环形的数组 取模: `%`的方式来完成(使用一个算法)。 

### 数组模拟环形队列

对前面的数组模拟队列的优化，充分利用数组，因此将数组看做是一个环形的。(通过取模的方式来实现即可)

**分析说明**：

1.	 尾索引的下一个为头索引时表示队列满，即将队列容量空出一个作为约定，这个在做判断队列满的时候需要注意(rear + 1) % maxSize == front [满]
2.	rear == front [空]
3.	测试示意图：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161856111.png)

**思路如下**:

1.  front  变量的含义做一个调整： front 就指向队列的第一个元素, 也就是说 arr[front] 就是队列的第一个元素

	 front 的初始值 = 0

2.  rear 变量的含义做一个调整：rear 指向队列的最后一个元素的后一个位置. 因为希望空出一个空间做为约定.

	rear 的初始值 = 0

3. 当队列满时，条件是 (rear  + 1) % maxSize == front 【满】

4. 对队列为空的条件， rear == front 空

5. 当我们这样分析， 队列中有效的数据的个数 (rear + maxSize - front) % maxSize // rear = 1 front = 0

6. 我们就可以在原来的队列上修改得到，一个环形队列

代码实现：

```java
@SuppressWarnings("all")
public class ArrayQueueDemo01 {
    public static void main(String ... args){
        Scanner sc = new Scanner(System.in);
        //测试一把
        //初始化环形队列
        ArrayQueue01 arrayQueue = new ArrayQueue01(4);//说明设置4,其队列的有效数据最大是3.空出了一个约定空间
        //接受用户输入
        char event_key = ' ';
        boolean loop = true;
        while(loop){
            //显示菜单
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出程序");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出数据");
            System.out.println("h(head): 查看队列头的数据");
            System.out.println("============================");
            event_key = sc.next().charAt(0);
            switch(event_key){
                case 's':
                    try{
                        arrayQueue.showQueue();
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    sc.close();
                    loop = false;
                    break;
                case 'a':
                    try{
                        System.out.println("请添加数据到队列中");
                        int num = sc.nextInt();
                        arrayQueue.addQueue(num);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'g':
                    try{
                        int value = arrayQueue.getQueue();
                        System.out.println(value);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try{
                        int head = arrayQueue.headQueue();
                        System.out.println(">> "+head);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                default:
                    System.out.println("请选择存在项");
                    break;
            }
        }
        System.out.println(">> 程序退出");
    }
}
@SuppressWarnings("All")
class ArrayQueue01 {
    private int front;
    private int rear;
    private int maxSize;
    private int[] arr;
    public ArrayQueue01 (int maxSize){
        this.maxSize = maxSize;
        arr = new int[maxSize];
    }

    //判断队列是否满
    public boolean isFull(){
        return (rear + 1) % maxSize == front;
    }

    //判断队列是否为空
    public boolean isEmpty(){
        return rear == front;
    }

    //添加数据到队列
    public void addQueue(int n){
        //判断队列是否满
        if(isFull()){
            throw new RuntimeException("队列满了，不能添加数据了");
        }
        //向数组进行赋值
        arr[rear] = n;
        //将rear向后移，考虑环形数组所以需要使用取模
        rear = (rear + 1) % maxSize;
    }

    //获取队列的数据，出队列
    public int getQueue(){
        //判断是否为空
        if(isEmpty()){
            throw new RuntimeException("队列没有数据");
        }
        /**
         * 这里需要分析出front是指向队列的第一个元素
         * 1.先把front，对应的值保留到一个临时变量
         * 2.将front后移，考虑取模
         * 3.降临时保存的变量返回
         */
        int value = arr[front];
        front = (front + 1) % maxSize;
        return value;
    }

    //显示队列中所有数据
    public void showQueue(){
        //判断队列是否为空
        if(isEmpty()){
            throw new RuntimeException("队列没有数据");
        }
        for(int i = front;i < front + size();i ++){
            System.out.printf(">> arr[%d]=%d\n",i % maxSize,arr[i % maxSize]);
        }
    }

    //求出当前队列有效数据的个数
    public int size(){
        return (rear + maxSize - front) % maxSize;
    }

    //显示队列的头元素
    public int headQueue(){
        if(isEmpty()){
            throw new RuntimeException("队列为空");
        }
        return arr[front];
    }
}
```

## 链表

链表是有序的列表，但是它在内存中是存储如下：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161856538.png)

小结：

1.	 链表是以节点方式来存储，是链式存储。
2.	每个节点包含data域，next域：指向下一个节点。
3.	如图：发现链表的各个节点不一定是连续存储。
4.	链表分带头节点的链表和没有头节点的链表，根据实际的需求来确定。

**单链表**(带头节点) **逻辑结构**示意图如下：==并不是连续存储的==。

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161856015.png)

**单链表的应用实例**:

使用带head头的单向链表实现 –水浒英雄排行榜管理

1. 第一种方法在添加英雄时，直接添加到链表的尾部
- 思路分析示意图：
![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161856161.png)
2. 第二种方式在添加英雄时，根据排名将英雄插入到指定位置  
(如果有这个排名，则添加失败，并给出提示)
- 思路分析示意图：
![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161856950.png)
3. 修改节点功能
- 思路：先找到该节点，通过遍历,temp.name = h.name;temp.nickname = h.nickname
4. 删除节点
- 思路分析示意图：
![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161857570.png)


 ### 单链表面试题(新浪、百度、腾讯)

单链表的常见面试题有如下:

1. 求单链表中有效节点的个数

   ```java
   /**
    * 方法：获取到单链表的节点的个数(如果是带头节点的链表，需求不统计头节点)
    * @param h 链表的头节点
    * @return 返回的就是有效节点的个数
    */
   public static int getLength(HeroNode h){
      //空链表直接返回0
      if(h.next == null){
         return 0;
      }
      //定义辅助变量，没有统计头节点，通过h.next直接获取下一个节点的数据
      HeroNode temp = h.next;
      int count = 0;
      while(temp != null){
         count++;
         temp = temp.next;
      }
      //返回有效的个数
      return count;
   }
   ```

2. 查找单链表中的倒数第k个结点  【新浪面试题】

   ```java
   /**
    * 查找单链表中的倒数第k个节点[新浪面试题]
    * 思路：
    * 1.编写一个方法，接受head节点，同时接受一个index
    * 2.index表示是倒数第index个节点
    * 3.先把链表从头到尾遍历，得到链表的总的长度getLength
    * 4.得到size后，我们从链表的第一个开始遍历(size - index)个，就可以得到
    * 5.如果找到了，则返回该节点，否则返回null
    * @param h
    * @param index
    * @return
    */
   public static HeroNode findLastIndexNode(HeroNode h,int index){
      //判断如果链表为空，则返回Null
      if(h.next == null){
         return null;
      }
      //第一个遍历得到链表的长度(节点个数)
      int size = getLength(h);
      //第二次遍历，size - index位置，就是我们倒数的第k个节点
      //先做一个index的校验
      if(index <= 0 || index > size){
         return null;
      }
      //定义辅助变量，for循环定位到倒数的index数据
      HeroNode cur = h.next;
      for(int i = 0;i < size - index;i ++){
         cur = cur.next;
      }
      return cur;
   }
   ```

3. 单链表的反转【腾讯面试题，有点难度】

   -  思路分析示意图：

   ![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161856299.png)

   ```java
   /**
    * 将单链表反转 [腾讯面试题]
    * @param h
    */
   public static void reverseList(HeroNode h){
      //如果当前链表为空，或者只有一个节点，无需反转，直接返回
      if(h.next == null || h.next.next == null){
         return ;
      }
      //定义一个辅助变量，帮助我们遍历原来的链表
      HeroNode cur = h.next;
      //指向当前节点[cur]的下一个节点
      HeroNode next = null;
      HeroNode reverseHead = new HeroNode(0,"","");
      //遍历原来的链表，每遍历一个节点，就将其取出，并放在心的链表reverseHead的最前端
      while(cur != null){
         //先暂时保存当前节点的下一个节点，因为后面需要使用
         next = cur.next;
         //将cur的下一个节点指向新的链表的最前端
         cur.next = reverseHead.next;
         //将cur连接到新的链表上
         reverseHead.next = cur;
         //让cur后移
         cur = next;
      }
      //将head.next指向reverseHead.next,实现单链表的反转
      h.next = reverseHead.next;
   }
   ```

4. 从尾到头打印单链表 (<font style="color:red">**不破坏链表结构**</font>)【百度，要求方式1：反向遍历 。  方式2：Stack栈】

   - 思路分析示意图：

   ![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161856313.png)

   ```java
    /**
     * 利用栈这个数据结构，将各个节点压入到栈中，然后利用栈的先进后出的特点，就实现了逆序打印的效果
     * 这样就能保证链表本身的结构不发生变化
     * @param h
     */
    public static void reversePrint(HeroNode h){
       //判断链表是否为空
       if(h.next == null){
          return;
       }
       //创建一个栈，将各个节点压入栈
       Stack<HeroNode> stack = new Stack<HeroNode>();
       HeroNode cur = h.next;
       //将链表的所有节点压入栈
       while(cur != null){
          //入栈
          stack.push(cur);
          //向后移动，遍历节点
          cur = cur.next;
       }
       //将栈中的节点进行打印,pop出栈
       while(stack.size() > 0){
          System.out.println(stack.pop());
       }
    }
   ```

完整代码实现：

```java
@SuppressWarnings("all")
public class SingleLinkedList {
    public static void main(String ... args){
        //进行测试
        //创建几个节点
        HeroNode h = new HeroNode(1,"松江","及时雨");
        HeroNode h1 = new HeroNode(2,"卢俊义","玉麒麟");
        HeroNode h2 = new HeroNode(3,"吴用","智多星");
        HeroNode h3 = new HeroNode(4,"林冲","豹子头");
        //创建链表并加入链表中
        SingleLinkedListTwo linkedList = new SingleLinkedListTwo();
//        linkedList.add(h);
//        linkedList.add(h3);
//        linkedList.add(h2);
//        linkedList.add(h1);

        linkedList.addByOrder(h);
        linkedList.addByOrder(h3);
        linkedList.addByOrder(h2);
        linkedList.addByOrder(h1);

        System.out.println("正常情况下链表的数据");
        linkedList.list();
        linkedList.reverseList(linkedList.getHead());
        System.out.println("反转后的链表数据");
        linkedList.list();

        System.out.println("逆序打印链表的数据");
        linkedList.reversePrint(linkedList.getHead());
/*
        //显示链表中的数据
        System.out.println("修改之前的数据");
        linkedList.list();

        //测试修改节点的代码
        HeroNode h02 = new HeroNode(2,"小卢","玉麒麟~~");
        linkedList.update(h02);

        //显示链表中的数据
        System.out.println("修改之后的数据");
        linkedList.list();

        linkedList.del(2);
        System.out.println("删除后的数据");
        linkedList.list();

        linkedList.del(4);
        System.out.println("删除后的数据");
        linkedList.list();

        //获取单链表的有效个数
        System.out.println("返回有效数据个数："+linkedList.getLength(linkedList.getHead()));

        //得到倒数第k个数据
        HeroNode result = linkedList.findLastIndexNode(linkedList.getHead(),1);
        System.out.println("倒数第k个节点的数据："+result);
*/

    }
}
//定义SingleLinkedListTwo管理节点
class SingleLinkedListTwo {
    //初始化头结点
    private HeroNode head = new HeroNode(0,"","");

    public HeroNode getHead(){
        return head;
    }

    /**
     * 将单链表反转
     * @param h
     */
    public static void reverseList(HeroNode h){
        //如果当前链表为空，或者只有一个节点，无需反转，直接返回
        if(h.next == null || h.next.next == null){
            return ;
        }
        //定义一个辅助变量，帮助我们遍历原来的链表
        HeroNode cur = h.next;
        //指向当前节点[cur]的下一个节点
        HeroNode next = null;
        HeroNode reverseHead = new HeroNode(0,"","");
        //遍历原来的链表，每遍历一个节点，就将其取出，并放在心的链表reverseHead的最前端
        while(cur != null){
            //先暂时保存当前节点的下一个节点，因为后面需要使用
            next = cur.next;
            //将cur的下一个节点指向新的链表的最前端
            cur.next = reverseHead.next;
            //将cur连接到新的链表上
            reverseHead.next = cur;
            //让cur后移
            cur = next;
        }
        //将head.next指向reverseHead.next,实现单链表的反转
        h.next = reverseHead.next;
    }

    /**
     * 利用栈这个数据结构，将各个节点压入到栈中，然后利用栈的先进后出的特点，就实现了逆序打印的效果
     * 这样就能保证链表本身的结构不发生变化
     * @param h
     */
    public static void reversePrint(HeroNode h){
        //判断链表是否为空
        if(h.next == null){
            return;
        }
        //创建一个栈，将各个节点压入栈
        Stack<HeroNode> stack = new Stack<HeroNode>();
        HeroNode cur = h.next;
        //将链表的所有节点压入栈
        while(cur != null){
            //入栈
            stack.push(cur);
            //向后移动，遍历节点
            cur = cur.next;
        }
        //将栈中的节点进行打印,pop出栈
        while(stack.size() > 0){
            System.out.println(stack.pop());
        }
    }

    //添加节点到单向链表
    /**
     * 思路：当不考虑编号顺序时
     * 1.找到当前链表的最后节点
     * 2.将最后这个节点的next，指向 新的节点
     * @param h
     */
    public void add(HeroNode h){
        //因为head节点不能动，因此我们需要一个辅助变量temp
        HeroNode temp = head;
        //遍历链表，找到最后
        while(true){
            //找到链表的最后
            if(temp.next == null){
                break;
            }
            //如果没有找到最后，就将temp后移
            temp = temp.next;
        }
        //当退出while循环是，那么temp就指向了链表的最后
        //将最后这个节点的next，指向 新的节点
        temp.next = h;
    }

    /**
     * 第二中方式在添加人物时，根据排名也就是编号，将人物插入到指定位置
     * (如果有这个排名，则添加失败，并给出提示)
     * @param h
     */
    public void addByOrder(HeroNode h){
        //因为头节点不能动，因此我们仍然通过一个辅助指针(变量)来帮助找到添加的位置
        //因为单链表，因为我们找的temp是位于添加位置的前一个节点，否则插入不了
        HeroNode temp = head;
        //标示添加的编号是否存在，默认为false
        boolean flag = false;
        while(true){
            //说明temp已经在链表的最后
            if(temp.next == null){
                break;
            }
            //位置找到，就在temp的后面插入
            if(temp.next.no > h.no){
                break;
                //说明希望添加的节点的编号已经存在了
            }else if(temp.next.no == h.no){
                flag = true;
                break;
            }
            //后移
            temp = temp.next;
        }
        if(flag){
            System.out.printf("添加的人物编号%d已经存在,不能重复添加",h.no);
        }else{
            h.next = temp.next;
            temp.next = h;
        }
    }

    /**
     * 修改节点的信息,根据no编号来修改，即no编号不能改
     * 说明：
     * 1.根据h的no来修改即可
     * @param h
     */
    public void update(HeroNode h){
        //判断链表是否为空
        if(head.next == null){
            System.out.println("链表为空");
            return;
        }
        //找到需要修改的节点，根据no编号
        //定义一个辅助变量
        HeroNode temp = head.next;
        //标志 标记是否找到
        boolean flag = false;
        while(true){
            //判断是否遍历到链表的最后
            if(temp == null){
                break;
            }
            //判断是否找到
            if(temp.no == h.no){
                flag = true;
                break;
            }
            temp = temp.next;
        }

        if(flag){
            temp.name = h.name;
            temp.nickName = h.nickName;
            System.out.println("被修改的人物编号："+h.no);
        }else{
            System.out.println("没有找到需要修改的编号，无法进行修改");
        }

    }

    /**
     * 删除节点
     * 思路：
     * 1.head不能动，因此我们需要一个temp辅助节点找到待删除节点的前一个节点
     * 2.说明我们在比较时，是temp.next.no和 需要删除的节点的no比较
     * @param no
     */
    public void del(int no){
        //定义临时变量
        HeroNode temp = head;
        //标志，记录是否找到待删除节点
        boolean flag = false;
        while(true){
            //判断是否到链表的最后
            if(temp.next == null){
                break;
            }
            //判断是否找到要删除的节点
            if(temp.next.no == no){
                //标志设置可执行
                flag = true;
                break;
            }
            //向后移动
            temp = temp.next;
        }
        //判断标志
        if(flag){
            //将temp下一个引用指向temp下下个next那么中间的就没有引用了jvm就会做垃圾回收机制将其处理
            temp.next = temp.next.next;
        }else{
            System.out.println("删除节点不存在");
        }
    }

    /**
     * 查找单链表中的倒数第k个节点[新浪面试题]
     * 思路：
     * 1.编写一个方法，接受head节点，同时接受一个index
     * 2.index表示是倒数第index个节点
     * 3.先把链表从头到尾遍历，得到链表的总的长度getLength
     * 4.得到size后，我们从链表的第一个开始遍历(size - index)个，就可以得到
     * 5.如果找到了，则返回该节点，否则返回null
     * @param h
     * @param index
     * @return
     */
    public static HeroNode findLastIndexNode(HeroNode h,int index){
        //判断如果链表为空，则返回Null
        if(h.next == null){
            return null;
        }
        //第一个遍历得到链表的长度(节点个数)
        int size = getLength(h);
        //第二次遍历，size - index位置，就是我们倒数的第k个节点
        //先做一个index的校验
        if(index <= 0 || index > size){
            return null;
        }
        //定义辅助变量，for循环定位到倒数的index数据
        HeroNode cur = h.next;
        for(int i = 0;i < size - index;i ++){
            cur = cur.next;
        }
        return cur;
    }

    /**
     * 方法：获取到单链表的节点的个数(如果是带头节点的链表，需求不统计头节点)
     * @param h 链表的头节点
     * @return 返回的就是有效节点的个数
     */
    public static int getLength(HeroNode h){
        //空链表直接返回0
        if(h.next == null){
            return 0;
        }
        //定义辅助变量，没有统计头节点，通过h.next直接获取下一个节点的数据
        HeroNode temp = h.next;
        int count = 0;
        while(temp != null){
            count++;
            temp = temp.next;
        }
        //返回有效的个数
        return count;
    }

    /**
     * 显示链表的数据[遍历]
     */
    public void list(){
        //判断链表是否为空
        if(head.next == null){
            System.out.println("链表为空");
            return;
        }
        //因为头节点不能动，因此我们需要一个辅助变量来进行遍历
        HeroNode temp = head.next;
        while(true){
            //判断是否到链表的最后
            if(temp == null){
                //结束循环
                break;
            }
            //输出节点信息
            System.out.println(temp);
            //将temp向后移动,不后移就是死循环
            temp = temp.next;
        }
    }
}
//定义HeroNode 每个 HeroNode对象就是一个节点
class HeroNode {
    public int no;
    public String name;
    public String nickName;
    //指向下一个节点
    public HeroNode next;

    //构造器 初始化
    public HeroNode(int no, String name, String nickName) {
        this.no = no;
        this.name = name;
        this.nickName = nickName;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickName='" + nickName + '\'' +
                '}';
    }
}
```

### 双向链表应用实例

使用带head头的双向链表实现 –水浒英雄排行榜

管理单向链表的缺点分析:

1. 单向链表，查找的方向只能是一个方向，而双向链  表可以向前或者向后查找。
2. 单向链表不能自我删除，需要靠辅助节点 ，而双向 链表，则可以自我删除，所以前面我们单链表删除 时节点，总是找到temp,temp是待删除节点的前一  个节点(认真体会).
3. 示意图帮助理解删除

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161856489.png)

分析 双向链表的遍历，添加，修改，删除的操作思路===》代码实现

1. 遍历 方和 单链表一样，只是可以向前，也可以向后查找
2. 添加  (默认添加到双向链表的最后)
	1. 先找到双向链表的最后这个节点
	2. temp.next = newHeroNode
	3. newHeroNode.pre = temp;
3.  修改 思路和 原来的单向链表一样.
4.  删除
	1. 因为是双向链表，因此，我们可以实现自我删除某个节点
	2. 直接找到要删除的这个节点，比如temp
	 temp.pre.next = temp.next
	    temp.next.pre = temp.pre;

代码实现：

```java
@SuppressWarnings("all")
public class DoubleLinkedListDemo {
    public static void main(String ... args){
        System.out.println("双向链表的测试");
        DoubleLinkedList du = new DoubleLinkedList();
        HeroNode01 h = new HeroNode01(1,"松江","及时雨");
        HeroNode01 h1 = new HeroNode01(2,"卢俊义","玉麒麟");
        HeroNode01 h2 = new HeroNode01(3,"吴用","智多星");
        HeroNode01 h3 = new HeroNode01(4,"林冲","豹子头");
        du.addByOrder(h);
        du.addByOrder(h3);
        du.addByOrder(h2);
        du.addByOrder(h1);

//        System.out.println("修改前的数据");
        du.list();


//        HeroNode01 h01 = new HeroNode01(2,"小卢","玉麒麟~~");
//        du.update(h01);
//        System.out.println("修改后的数据");
//        du.list();
//
//        du.del(1);
//        du.del(3);
//        System.out.println("删除后的数据");
//        du.list();
    }
}
class DoubleLinkedList {
    //初始化头节点，这个节点不动
    HeroNode01 head = new HeroNode01(0,"","");

    //返回头节点
    public HeroNode01 getHead(){
        return head;
    }

    /**
     * 有序添加元素
     * @param h
     */
    public void addByOrder(HeroNode01 h){
        HeroNode01 temp = head;
        boolean flag = false;
        while(true){
            if(temp.next == null){
                break;
            }
            if(temp.next.no > h.no){
                break;
            }else if(temp.next.no == h.no){
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if(flag){
            System.out.printf("节点的主键(%d)存在重复",temp.next.no);
        }else{
            /**
             * 为防止出现空指针情况，需要对temp节点位置进行判断
             * 若双向链表尚未达尾端，则需要将h节点与其相邻的后面的节点进行连接
             */
            if(temp.next != null){
                h.next = temp.next;
                temp.next.pre = h;
            }
            //无论双向链表是否达尾端，都需要将h节点与其相邻的前面的节点进行连接
            temp.next = h;
            h.pre = temp;
        }
    }

    /**
     * 双向链表删除节点
     * 说明：
     * 1.对于双向链表，我们可以直接找到要删除这个节点
     * 2.找到后，自我删除即可,而不用说找到删除节点的前一个节点然后进行删除
     * @param no
     */
    public void del(int no){
        HeroNode01 temp = head.next;
        if(head.next == null){
            System.out.println("链表为空");
            return;
        }
        boolean flag = false;
        while(true){
            if(temp == null){
                break;
            }
            if(temp.no == no){
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if(flag){

            /**
             * 如果删除最后一个节点
             * 让当前要删除的节点的前一个节点的next指向下一个节点
             * 而后一个节点是null所以next为null
             */
            temp.pre.next = temp.next;

            /**
             * 如果是最后一个节点执行下面操作会出现异常
             * 最后一个节点的next是空的
             * 所以next为null就不可能有pre因此就产生了问题
             * 最后一个节点的时候就不需要执行下面的操作了，否则报错PointerException
             * 判断后一个节点是否为null解决报错问题
             */
            if(temp.next != null){
                temp.next.pre = temp.pre;
            }
        }else{
            System.out.println("没有可删除节点");
        }
    }

    public void update(HeroNode01 n){
        boolean flag = false;
        if(head.next == null){
            System.out.println("链表为空");
            return ;
        }
        HeroNode01 temp = head.next;
        while(true){
            if(temp.next == null){
                break;
            }
            if(temp.no == n.no){
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if(flag){
            temp.name = n.name;
            temp.nickName = n.nickName;
            System.out.println("修改的编号为："+temp.no);
        }else{
            System.out.println("没有可修改的编号");
        }

    }

    //双向链表的添加
    public void add(HeroNode01 h){
        HeroNode01 temp = head;
        while(true){
            if(temp.next == null){
                break;
            }
            temp = temp.next;
        }
        temp.next = h;
        h.pre = temp;
    }

    //遍历双向链表的数据
    public void list(){
        //判断链表是否为空
        if(head.next == null){
            System.out.println("链表为空");
            return ;
        }
        //因为头结点不能动，所以需要使用辅助变量
        HeroNode01 temp = head.next;
        while(true){
            //遍历是否循环到链表最后
            if(temp == null){
                break;
            }
            System.out.println(temp);
            temp = temp.next;
        }
    }
}

class HeroNode01 {
    //指向下一个节点,默认为null
    public HeroNode01 next;
    //指向前一个节点,默认为null
    public HeroNode01 pre;
    public int no;
    public String name;
    public String nickName;

    public HeroNode01(int no, String name, String nickName) {
        this.no = no;
        this.name = name;
        this.nickName = nickName;
    }

    @Override
    public String toString() {
        return "HeroNode01{"+
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickName='" + nickName + '\'' +
                '}';
    }
}
```

### 单向环形链表应用场景

Josephu(约瑟夫、约瑟夫环) 问题

Josephu 问题为：设编号为1，2，… n的n个人围坐一圈，约定编号为k（1<=k<=n）的人从1开始报数，数到m 的那个人出列，它的下一位又从1开始报数，数到m的那个人又出列，依次类推，直到所有人出列为止，由此产生一个出队编号的序列。

提示：用一个不带头结点的循环链表来处理Josephu 问题：先构成一个有n个结点的单循环链表，然后由k结点起从1开始计数，计到m时，对应结点从链表中删除，然后再从被删除结点的下一个结点又从1开始计数，直到最后一个结点从链表中删除算法结束。

**单向环形链表介绍** 

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161857436.png)

**Josephu问题** 

ØJosephu 问题

Josephu 问题为：设编号为1，2，… n的n个人围坐一圈，约定编号为k（1<=k<=n）的人从1开始报数，数到m 的那个人出列，它的下一位又从1开始报数，数到m的那个人又出列，依次类推，直到所有人出列为止，由此产生一个出队编号的序列。

Ø提示

用一个不带头结点的循环链表来处理Josephu 问题：先构成一个有n个结点的单循环链表，然后由k结点起从1开始计数，计到m时，对应结点从链表中删除，然后再从被删除结点的下一个结点又从1开始计数，直到最后一个结点从链表中删除算法结束。

Ø示意图说明

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161857188.png)

约瑟夫问题-创建环形链表的思路图解：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161858378.png)

约瑟夫问题-小孩出圈的思路分析图：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161858452.png)

Josephu代码实现：

```java
package com.atguigu.linkedlist;

public class Josepfu {

	public static void main(String[] args) {
		// 测试一把看看构建环形链表，和遍历是否ok
		CircleSingleLinkedList circleSingleLinkedList = new CircleSingleLinkedList();
		circleSingleLinkedList.addBoy(125);// 加入5个小孩节点
		circleSingleLinkedList.showBoy();
		
		//测试一把小孩出圈是否正确
		circleSingleLinkedList.countBoy(10, 20, 125); // 2->4->1->5->3
		//String str = "7*2*2-5+1-5+3-3";
	}

}

// 创建一个环形的单向链表
class CircleSingleLinkedList {
	// 创建一个first节点,当前没有编号
	private Boy first = null;

	// 添加小孩节点，构建成一个环形的链表
	public void addBoy(int nums) {
		// nums 做一个数据校验
		if (nums < 1) {
			System.out.println("nums的值不正确");
			return;
		}
		Boy curBoy = null; // 辅助指针，帮助构建环形链表
		// 使用for来创建我们的环形链表
		for (int i = 1; i <= nums; i++) {
			// 根据编号，创建小孩节点
			Boy boy = new Boy(i);
			// 如果是第一个小孩
			if (i == 1) {
				first = boy;
				first.setNext(first); // 构成环
				curBoy = first; // 让curBoy指向第一个小孩
			} else {
				curBoy.setNext(boy);//
				boy.setNext(first);//
				curBoy = boy;
			}
		}
	}

	// 遍历当前的环形链表
	public void showBoy() {
		// 判断链表是否为空
		if (first == null) {
			System.out.println("没有任何小孩~~");
			return;
		}
		// 因为first不能动，因此我们仍然使用一个辅助指针完成遍历
		Boy curBoy = first;
		while (true) {
			System.out.printf("小孩的编号 %d \n", curBoy.getNo());
			if (curBoy.getNext() == first) {// 说明已经遍历完毕
				break;
			}
			curBoy = curBoy.getNext(); // curBoy后移
		}
	}

	// 根据用户的输入，计算出小孩出圈的顺序
	/**
	 * 
	 * @param startNo
	 *            表示从第几个小孩开始数数
	 * @param countNum
	 *            表示数几下
	 * @param nums
	 *            表示最初有多少小孩在圈中
	 */
	public void countBoy(int startNo, int countNum, int nums) {
		// 先对数据进行校验
		if (first == null || startNo < 1 || startNo > nums) {
			System.out.println("参数输入有误， 请重新输入");
			return;
		}
		// 创建要给辅助指针,帮助完成小孩出圈
		Boy helper = first;
		// 需求创建一个辅助指针(变量) helper , 事先应该指向环形链表的最后这个节点
		while (true) {
			if (helper.getNext() == first) { // 说明helper指向最后小孩节点
				break;
			}
			helper = helper.getNext();
		}
		//小孩报数前，先让 first 和  helper 移动 k - 1次
		for(int j = 0; j < startNo - 1; j++) {
		    //first和helper一块向后移动它们两个始终都是围绕着我个环形first为头helper为绕环形最后一个
			first = first.getNext();
			helper = helper.getNext();
		}
		//当小孩报数时，让first 和 helper 指针同时 的移动  m  - 1 次, 然后出圈
		//这里是一个循环操作，知道圈中只有一个节点
		while(true) {
			if(helper == first) { //说明圈中只有一个节点
				break;
			}
			//让 first 和 helper 指针同时 的移动 countNum - 1
			for(int j = 0; j < countNum - 1; j++) {
				first = first.getNext();
				helper = helper.getNext();
			}
			//这时first指向的节点，就是要出圈的小孩节点
			System.out.printf("小孩%d出圈\n", first.getNo());
			//这时将first指向的小孩节点出圈
			first = first.getNext();
			helper.setNext(first); //
			
		}
		System.out.printf("最后留在圈中的小孩编号%d \n", first.getNo());
		
	}
}
// 创建一个Boy类，表示一个节点
class Boy {
	private int no;// 编号
	private Boy next; // 指向下一个节点,默认null

	public Boy(int no) {
		this.no = no;
	}
	public int getNo() {
		return no;
	}
	public void setNo(int no) {
		this.no = no;
	}
	public Boy getNext() {
		return next;
	}
	public void setNext(Boy next) {
		this.next = next;
	}
}
```

## 栈

**栈的一个实际需求** 

请输入一个表达式

计算式:[7*2*2-5+1-5+3-3] 点击计算【如下图】

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161858884.png)

请问: 计算机底层是如何运算得到结果的？ 注意不是简单的把算式列出运算,因为我们看这个算式 7 * 2 * 2 - 5, 但是计算机怎么理解这个算式的(对计算机而言，它接收到的就是一个字符串)，我们讨论的是这个问题。-> ==栈==.

**栈的介绍** 

1. 栈的英文为(==stack==)
2. 栈是一个==先入后出==(FILO-First In Last Out)的有序列表。
3. 栈(stack)是限制线性表中元素的插入和删除只能在线性表的同一端进行的一种特殊线性表。允许插入和删除的一端，为==变化的一端，称为栈顶(Top)==，另一端为==固定的一端，称为栈底(Bottom)==。
4. 根据栈的定义可知，最先放入栈中元素在栈底，最后放入的元素在栈顶，而删除元素刚好相反，最后放入的元素最先删除，最先放入的元素最后删除
5. ==出栈(pop)和入栈(push)的概念==(如图所示)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161858370.png)

**栈的应用场景** 

1. 子程序的调用：在跳往子程序前，会先将下个指令的地址存到堆栈中，直到子程序执行完后再将地址取出，以回到原来的程序中。
2. 处理递归调用：和子程序的调用类似，只是除了储存下一个指令的地址外，也将参数、区域变量等数据存入堆栈中。
3. 表达式的转换[中缀表达式转后缀表达式]与求值(实际解决)。
4. 二叉树的遍历。
5. 图形的深度优先(depth一first)搜索法。

**栈的快速入门** 

1. 用数组模拟栈的使用，由于栈是一种有序列表，  当然可以使用数组的结构来储存栈的数据内容，  下面我们就用数组模拟栈的出栈，入栈等操作。
2. 实现思路分析,并画出示意图

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161858827.png)

代码实现：

```java
public class ArrayStackDemo {
    public static void main(String ... args){
        //测试数组模拟栈是否正确
        //先创建一个ArrayStack对象 --> 表示栈
        ArrayStack stack = new ArrayStack(4);
        char  key = ' ';
        boolean flag = true;
        Scanner sc = new Scanner(System.in);
        while(flag){
            System.out.println("s(show): 展示栈数据");
            System.out.println("a(push): 向栈中添加数据(入栈)");
            System.out.println("g(pop): 弹出栈顶元素(出栈)");
            System.out.println("e(exit): 退出数组模拟栈程序");
            System.out.println("============================");
            key = sc.next().charAt(0);
            switch(key){
                case 's':
                    try{
                        stack.list();
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'a':
                    try{
                        System.out.println("请添加数据");
                        int value = sc.nextInt();
                        stack.push(value);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'g':
                    try{
                        int pop = stack.pop();
                        System.out.printf("弹出栈顶元素：%d\n",pop);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    sc.close();
                    flag = false;
                    break;
                default:
                    System.out.println("请选择存在的选项");
                    break;
            }
        }
    }
}
class ArrayStack {
    //栈的大小
    private int maxSize;
    //数组模拟栈，数据存入该数组中
    private int[] stack;
    //top表示栈顶，初始化为-1表示没有数据
    private int top = -1;

    //构造器
    public ArrayStack (int maxSize) {
        this.maxSize = maxSize;
        stack = new int[maxSize];
    }

    //栈满
    public boolean isFull(){
        return top == maxSize - 1;
    }

    //栈空
    public boolean isEmpty(){
        return top == -1;
    }

    //入栈
    public void push(int value){
        //判断栈是否满了
        if(isFull()){
            throw new RuntimeException("栈满了");
        }
        top++;
        stack[top] = value;
    }

    //出栈
    public int pop(){
        //判断栈是否为空
        if(isEmpty()){
            throw new RuntimeException("栈为空");
        }
        int value = stack[top];
        top--;
        return value;
    }

    //遍历栈,遍历时需要从栈顶开始遍历
    public void list(){
        if(isEmpty()){
            throw new RuntimeException("栈为空");
        }
        for(int i = top;i >= 0;i --){
            System.out.printf(">> stack[%d] = %d\n",i,stack[i]);
        }
    }
}
```

3. 课堂练习，将老师写的程序改成使用链表来模拟栈

- 出栈的图解示意图

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161858565.png)

代码实现：

```java
@SuppressWarnings("all")
public class LinkedListAnArrayStackDemo {
    public static void main(String ... args) {
        LinkedListDemo demo = new LinkedListDemo();
        char key = ' ';
        boolean flag  = true;
        Scanner sc = new Scanner(System.in);
        while(flag){
            System.out.println("a(push) 添加数据");
            System.out.println("s(list) 显示栈中所有数据");
            System.out.println("g(pop) 弹出栈顶元素");
            System.out.println("=================");
            key = sc.next().charAt(0);
            switch(key){
                case 'a':
                    try{
                        System.out.println("请添加数据");
                        int value = sc.nextInt();
                        demo.add(value);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'g':
                    try{
                        int i = demo.get();
                        System.out.println(">> 弹出元素为："+i);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 's':
                    try{
                        demo.list();
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                default:
                    System.out.println("请选择存在选项");
                    break;

            }
        }
    }
}
class LinkedListDemo {
    //定义头节点
    private LinkedList stack = new LinkedList(0);
    private LinkedList top = null;

    public LinkedList getStack(){
        return stack;
    }

    //入栈
    public void add(int h){
        LinkedList new_element = new LinkedList(h);
        //因为头节点不能动，需要一个辅助变量完成
        LinkedList temp = stack;
        //对链表进行遍历，遍历到最后一个节点即为链表的尾部，进行添加节点
        while(true){
            //链表的最后
            if(temp.next == null){
                break;
            }
            //移动节点位置
            temp = temp.next;
        }
        //对节点的下一个节点进行赋值
        temp.next = new_element;
        //将top节点指向这个最后一个数据的节点
        top = new_element;
    }

    //出栈
    public int get(){
        //将top指针前移动一个
        //需要重新遍历链表找到top节点的前一个节点，再由top指针指向这个节点
        LinkedList head = stack;
        //将 top 指针指向的节点出栈
        //因为出站后，top指针需要向前移动，所以需要一个辅助指针完成出栈
        LinkedList tmp = top;
        //判断栈是否为空
        if(head.next == null){
            throw new RuntimeException("栈为空");
        }
        while(true){
            //找到head.next == null节点的前一个节点
            if(head.next == top){
                break;
            }
            //指针后移
            head = head.next;
        }
        //退出循环后，找到top节点的前一个节点，对head.next节点进行删除赋值null
        //因为top.next节点是null,而head.next节点就是top.next节点的上一个节点
        head.next = top.next;
        //将top指针指向这个节点，完成top指针的前移动作
        top = head;
        //返回前一个节点的data数据
        return tmp.data;
    }

    //显示所有数据
    public void list(){
        LinkedList temp = stack;
        if(temp.next == null){
            throw new RuntimeException("栈为空");
        }
        int count = 0;
        while(true){
            if(temp.next == null){
                break;
            }
            count++;
            System.out.println(">> ("+count+")\t:\t"+temp.next.data);
            temp = temp.next;
        }
    }
}
class LinkedList {
    public int data;
    public LinkedList next;
    public LinkedList (int data) {
        this.data = data;
    }
}
```

### 栈实现综合计算器

使用栈来实现综合计算器-自定义优先级[priority]
Ø简化思路:

1. 3+2*6-2
2. 30+2*6-2
3. 7*2*2-5+1-5+3-4

**使用栈完成表达式的计算 思路** 

1. 通过一个 index 值（索引），来遍历我们的表达式

2. 如果我们发现是一个数字, 就直接入数栈

3. 如果发现扫描到是一个符号, 就分如下情况

	3.1 如果发现当前的符号栈为 空，就直接入栈
	3.2 如果符号栈有操作符，就进行比较,如果当前的操作符的优先级小于或者等于栈中的操作符， 就需要从数栈中pop出两个数,在从符号栈中pop出一个符号，进行运算，将得到结果，入数栈，然后将当前的操作符入符号栈(*计算后下一个就是当前的操作符-入栈)， 如果当前的操作符的优先级大于栈中的操作符， 就直接入符号栈.
4. 当表达式扫描完毕，就顺序的从 数栈和符号栈中pop出相应的数和符号，并运行.
5. 最后在数栈只有一个数字，就是表达式的结果

验证： 3+2*6-2 = 13

示意图：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161858790.png)

代码实现：

```java
@SuppressWarnings("all")
public class Calculator {
    public static void main(String[] args) {
        //创建一个运算的表达式
        String expression = "(7-2)*3+1";
        //创建两个栈，一个数栈，一个字符栈各自存储不同的东西
        ArrayStack02 numStack = new ArrayStack02(10);
        ArrayStack02 operStack = new ArrayStack02(10);
        //定义需要的相关变量
        int index = 0;//扫描表达式的index值
        int num1 = 0;//从数栈中pop的第一个值
        int num2 = 0;//从数栈中pop的第二个值
        int oper = 0;//从字符栈中pop出的运算符
        int res = 0;//从数栈中pop出两个数值计算的结果
        char ch = ' ';//将每次扫描得到的char保存到ch
        String keepNum = "";//用于拼接多位数
        //开始while循环的扫描expression运算表达式
        while(true) {
            //依次得到expression 的每一个字符
            ch = expression.substring(index, index+1).charAt(0);
            //判断ch是什么，然后做相应的处理
            if(operStack.isOper(ch)) {//如果是运算符
                //判断当前的符号栈是否为空
                if(!operStack.isEmpty()) {
                    //如果符号栈有操作符，就进行比较,如果当前的操作符的优先级小于或者等于栈中的操作符,就需要从数栈中pop出两个数,
                    //这里需要判断是否此时的栈顶是否为左括号，如果是左括号不进入此循环
                    //我们设定的左括号是优先级大于加减乘除，所以当发现下一个进栈的符号的优先级比此时的栈顶的左括号优先级小的时候，
                    //应该让符号直接进栈，不进行弹出左符号的运算（左括号弹出来运算是不行的）
                    if(operStack.priority(ch) <= operStack.priority(operStack.peek()) & operStack.peek() != 40) {
                        num1 = numStack.pop();
                        num2 = numStack.pop();
                        oper = operStack.pop();
                        res = numStack.cal(num1, num2, oper);
                        //把运算的结果如数栈
                        numStack.push(res);
                        //然后将当前的操作符入符号栈
                        operStack.push(ch);
                        /**
                         * 进行右括号的判断。匹配左括号
                         * 当发现进入的是右括号时就优先进行括号内的计算
                         */
                    } else if(ch == 41){
                        //先让右括号进栈
                        operStack.push(ch);
                        if (ch == 41) {
                            //再把右括号弹出
                            int oper1 = operStack.pop();
                            //弹出右括号后开始进行括号内运算
                            while(true) {
                                //右括号
                                num1 = numStack.pop();
                                num2 = numStack.pop();
                                oper = operStack.pop();
                                res = numStack.cal(num1, num2, oper);
                                //把运算的结果如数栈
                                numStack.push(res);
                                //当运算到栈顶符号为左括号时候，就弹出栈顶元素左括号，结束循环
                                if(operStack.peek() == 40) {
                                    int oper2 = operStack.pop();
                                    break;
                                }
                            }
                        }
                        //如果当前的操作符的优先级大于栈中的操作符， 就直接入符号栈.
                    }
                    else {
                        operStack.push(ch);
                    }
                }else {
                    //如果为空直接入符号栈
                    operStack.push(ch);
                }
            } else { //如果是数，则直接入数栈
                //分析思路
                //1. 当处理多位数时，不能发现是一个数就立即入栈，因为他可能是多位数
                //2. 在处理数，需要向expression的表达式的index 后再看一位,如果是数就进行扫描，如果是符号才入栈
                //3. 因此我们需要定义一个变量 字符串，用于拼接
                //处理多位数
                keepNum += ch;

                //如果ch已经是expression的最后一位，就直接入栈
                if (index == expression.length() - 1) {
                    numStack.push(Integer.parseInt(keepNum));
                }else{
                    //判断下一个字符是不是数字，如果是数字，就继续扫描，如果是运算符，则入栈
                    //注意是看后一位，不是index++
                    if (operStack.isOper(expression.substring(index+1,index+2).charAt(0))) {
                        //如果后一位是运算符，则入栈 keepNum = "1" 或者 "123"
                        numStack.push(Integer.parseInt(keepNum));
                        //重要的!!!!!!, keepNum清空
                        keepNum = "";

                    }
                }
            }
            //让index + 1, 并判断是否扫描到expression最后.
            index++;
            if (index >= expression.length()) {
                break;
            }
        }
        //当表达式扫描完毕，就顺序的从数栈和符号栈中pop出相应的数和符号，并运行
        while(true){
            //如果符号栈为空，则计算到最后的结果，数栈中只有一个数字[结果]
            if(operStack.isEmpty()){
                break;
            }
            num1 = numStack.pop();
            num2 = numStack.pop();
            oper = operStack.pop();
            res = numStack.cal(num1,num2,oper);
            numStack.push(res);//入栈
        }
        //将数栈的最后数，pop出，就是结果
        int res2 = numStack.pop();
        System.out.printf("表达式%s = %d",expression,res2);
    }
}
@SuppressWarnings("all")
class ArrayStack02 {
    //栈的大小
    private int maxSize;
    //数组模拟栈，数据存入该数组中
    private int[] stack;
    //top表示栈顶，初始化为-1表示没有数据
    private int top = -1;

    //构造器
    public ArrayStack02 (int maxSize) {
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
    }

    //增加一个方法，可以返回当前栈顶的值，但是不是真正的pop
    public int peek(){
        return stack[top];
    }

    //栈满
    public boolean isFull(){
        return top == maxSize - 1;
    }

    //栈空
    public boolean isEmpty(){
        return top == -1;
    }

    //入栈
    public void push(int value){
        //判断栈是否满了
        if(isFull()){
            throw new RuntimeException("栈满了");
        }
        top++;
        stack[top] = value;
    }

    //出栈
    public int pop(){
        //判断栈是否为空
        if(isEmpty()){
            throw new RuntimeException("栈为空");
        }
        int value = stack[top];
        top--;
        return value;
    }

    //遍历栈,遍历时需要从栈顶开始遍历
    public void list(){
        if(isEmpty()){
            throw new RuntimeException("栈为空");
        }
        for(int i = top;i >= 0;i --){
            System.out.printf(">> stack[%d] = %d\n",i,stack[i]);
        }
    }

    /**
     * 返回运算符的优先级，优先级是程序员来确定，优先级使用数字表示
     * 数字越大，则优先级越高
     * @return
     */
    public int priority(int oper){
        if(oper == '(' || oper == ')'){
            return 2;
        }else if(oper == '*' || oper == '/'){
            return 1;
        }else if(oper == '+' || oper == '-'){
            return 0;
        }else{
            return -1;//假设目前的表达式只有 + - * /
        }
    }

    /**
     * 判断是否为一个运算符
     * @return
     */
    public boolean isOper(char val){
        return val == '+' || val == '-' || val == '*' || val == '/' || val == '('
                || val == ')';
    }

    /**
     * 计算方法
     * @return
     */
    public int cal(int num1,int num2,int  oper){
        int res = 0;
        //TODO 注意计算的顺序
        switch(oper){
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num2 - num1;
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num2 / num1;
                break;
            default:
                System.out.println("计算错误");
                break;
        }
        return res;
    }
}
```

### 前缀、中缀、后缀表达式(逆波兰表达式)

#### 前缀表达式(波兰表达式)

1. 前缀表达式又称==波兰式==，前缀表达式的运算符位于操作数之前
2. 举例说明： (3+4)×5-6 对应的前缀表达式就是 - × + 3 4 5 6

**前缀表达式的计算机求值** 

从==右==至==左==扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的计算（栈顶元素 和  次顶元素），并将结果入栈；重复上述过程直到表达式最左端，最后运算得出的值即为表达式的结果

例如:  (3+4)×5-6 对应的前缀表达式就是 - × + 3 4 5 6 , 针对前缀表达式求值步骤如下:

1. 从右至左扫描，将6、5、4、3压入堆栈
2. 遇到+运算符，因此弹出3和4（3为栈顶元素，4为次顶元素），计算出3+4的值，得7，再将7入栈
3. 接下来是×运算符，因此弹出7和5，计算出7×5=35，将35入栈
4. 最后是-运算符，计算出35-6的值，即29，由此得出最终结果


#### 中缀表达式

1. 中缀表达式就是常见的运算表达式，如(3+4)×5-6
2. 中缀表达式的求值是我们人最熟悉的，但是对计算机来说却不好操作(前面我们讲的案例就能看的这个问题)，因此，在计算结果时，往往会将中缀表达式转成其它表达式来操作(一般转成后缀表达式.)

#### 后缀表达式

1. 后缀表达式又称逆波兰表达式,与前缀表达式相似，只是运算符位于操作数之后
2. 中举例说明： (3+4)×5-6 对应的后缀表达式就是  3 4 + 5 × 6 –
3. 再比如:

| 正常的表达式 | 逆波兰表达式  |
| ------------ | ------------- |
| a+b          | a b +         |
| a+(b-c)      | a b c - +     |
| a+(b-c)*d    | a b c - d * + |
| a+d*(b-c)    | a d b c - * + |
| a=1+3        | a 1 3 + =     |

**后缀表达式的计算机求值** 

从==左==至==右==扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的计算（次顶元素 和 栈顶元素），并将结果入栈；重复上述过程直到表达式最右端，最后运算得出的值即为表达式的结果

例如: **(3+4)×5-6** 对应的后缀表达式就是 **3 4 + 5 × 6 -** **,** **针对后缀表达式求值步骤如下**:

1.  从左至右扫描，将3和4压入堆栈；
2.  遇到+运算符，因此弹出4和3（4为栈顶元素，3为次顶元素），计算出3+4的值，得7，再将7入栈；
3.  将5入栈；
4.  接下来是×运算符，因此弹出5和7，计算出7×5=35，将35入栈；
5.  将6入栈；
6.  最后是-运算符，计算出35-6的值，即29，由此得出最终结果 

#### 中缀转后缀表达式

大家看到，后缀表达式适合计算式进行运算，但是人却不太容易写出来，尤其是表达式很长的情况下，因此在开发中，我们需要将 **中缀表达式**转成**后缀表达式**。

具体步骤如下：

1.  初始化两个栈：运算符栈s1和储存中间结果的栈s2
2.  从左至右扫描中缀表达式
3.  遇到操作数时，将其压s2
4.  遇到运算符时，比较其与s1栈顶运算符的优先级：
    1.  如果s1为空，或栈顶运算符为左括号“(”，则直接将此运算符入栈
    2.  否则，若优先级比栈顶运算符的高，也将运算符压入s1
    3.  否则，将s1栈顶的运算符弹出并压入到s2中，再次转到(4-1)与s1中新的栈顶运算符相比较
5.  遇到括号时:
    1.  如果是左括号“(”，则直接压入s1
    2.  如果是右括号“)”，则依次弹出s1栈顶的运算符，并压入s2，直到遇到左括号为止，此时将这一对括号丢弃
6.  重复步骤2至5，直到表达式的最右边
7.  将s1中剩余的运算符依次弹出并压入s2
8.  依次弹出s2中的元素并输出，**结果的逆序即为中缀表达式对应的后缀表达式** 

示意图：

中缀表达式 转 后缀表达式的 思路步骤分析

打比方 ： 降龙十八掌 ：学习 -》 应用 [层次]

算法 -》 第一个层面： 理解算法-》灵活运用算法

第二层： 设计算法-》 运用 【】

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859882.png)

**举例说明**：

将中缀表达式**"1+((2+3)×4)-5"**转换为后缀表达式的过程如下:

因此结果为**"1 2 3 + 4 × + 5 –"** 

| 扫描到的元素 | s2(栈底->栈顶)     | s1 (栈底->栈顶) | 说明                               |
| ------------ | ------------------ | --------------- | ---------------------------------- |
| 1            | 1                  | 空              | 数字，直接入栈                     |
| +            | 1                  | +               | s1为空，运算符直接入栈             |
| (            | 1                  | + (             | 左括号，直接入栈                   |
| (            | 1                  | + ( (           | 同上                               |
| 2            | 1 2                | + ( (           | 数字                               |
| +            | 1 2                | + ( ( +         | s1栈顶为左括号，运算符直接入栈     |
| 3            | 1 2 3              | + ( ( +         | 数字                               |
| )            | 1 2 3 +            | + (             | 右括号，弹出运算符直至遇到左括号   |
| ×            | 1 2 3 +            | + ( ×           | s1栈顶为左括号，运算符直接入栈     |
| 4            | 1 2 3 + 4          | + ( ×           | 数字                               |
| )            | 1 2 3 + 4 ×        | +               | 右括号，弹出运算符直至遇到左括号   |
| -            | 1 2 3 + 4 × +      | -               | -与+优先级相同，因此弹出+，再压入- |
| 5            | 1 2 3 + 4 × + 5    | -               | 数字                               |
| 到达最右端   | 1  2 3 + 4 × + 5 - | 空              | s1中剩余的运算符                   |

代码实现：

```java
@SuppressWarnings("all")
public class PolandNotation {
    public static void main(String[] args) {
        //定义中缀表达式
        String suffixExpression = "1+((2+3)x4)-5";
        /**
         * 完成将一个中缀表达式转成后缀表达式的功能
         * 说明：
         * 1.1+((2+3)x4)-5 => 1 2 3 + 4 x + 5 -
         * 2.因为直接对str进行操作，不太方便，因此 先将 “1+((2+3)x4)-5” => 中缀的表达式对应的List即"1+((2+3)x4)-5"
         * => ArrayList[1,2,3,+,4,x,+,5,-]
         * 3.将得到的中缀表达式对应的List => 后缀表达式对应的List即ArrayList[1+((2+3)x4)-5]
         * => ArrayList[1,2,3,+,4,x,+,5,-]
         */
        List<String> resultList = toInfixExpressionList(suffixExpression);
        System.out.println(resultList);//[1, +, (, (, 2, +, 3, ), x, 4, ), -, 5]

        List<String> resultList1 = parseSuffixExpressionList(resultList);
        System.out.println(resultList1);

        System.out.printf("逆波兰表达式运算结果: %d",calculate(resultList1));

    }

    /**
     * 将一个逆波兰表达式，依次将数据和运算符，放入到ArrayList中
     * @param suffixExpression
     * @return
     */
    public static List<String> getListString(String suffixExpression){
        //将suffixExpression分割
        List<String> list = new ArrayList<>();
        String[] split = suffixExpression.split(" ");
        for(String ele:split){
            list.add(ele);
        }
        return list;
    }

    /**
     * 即ArrayList [1, +, (, (, 2, +, 3, ), x, 4, ), -, 5] => ArrayList[1,2,3,+,4,*,+,5,-]
     * 方法：将得到的中缀表达式对应的List => 后缀表达式对应的List
     * @param list
     * @return
     */
    public static List<String> parseSuffixExpressionList(List<String> list){
        //定义一个栈一个集合
        Stack<String> stack = new Stack<>();
        /**
         * 说明：因为ls这个栈，在整个转换过程中，没有pop操作，而且后面我们还需要逆序输出
         * 因此比较麻烦，这里我们就不用Stack<String>直接使用List<String> ls
         * Stack<String> ls = new Stack<String>(); 存储中间结果的栈ls
         */
        List<String> ls = new ArrayList<>();
        //遍历List集合中的数据
        for(String i:list){
            //如果是一个数，加入List集合中
            if(i.matches("\\d+")){
                ls.add(i);
            }else if(i.equals("(")){
                stack.push(i);
            }else if(i.equals(")")){
                //如果是右括号")",则依次弹出Stack栈顶的运算符，并压入List集合，直到遇到左括号为止，此时将一对括号丢弃
                while(!stack.peek().equals("(")){
                    ls.add(stack.pop());
                }
                stack.pop();
            }else{
                /**
                 * 当item的优先级小于等于stack栈顶运算符，将stack栈顶的运算符弹出并加入到list中，再次转到(4.1)与stack中新的
                 * 栈顶运算符相比较
                 * 问题：我们缺少一个比较优先级高低的方法
                 */
                while(stack.size() != 0 && Operation.getValue(stack.peek()) >= Operation.getValue(i)){
                    ls.add(stack.pop());
                }
                //还需要将i压入栈
                stack.push(i);
            }
        }
        //将stack中剩余的运算符依次弹出并加入list中
        while(stack.size() != 0){
            ls.add(stack.pop());
        }
        return ls;
    }

    /**
     * 方法：将中缀表达式转成对应的list
     * s = "1+((2+3)x4)-5";
     * @param s
     * @return
     */
    public static List<String> toInfixExpressionList(String s){
        //定义一个List，存放中缀表达式对应的内容
        List<String> list = new ArrayList<>();
        //这是一个指针，用于遍历中缀表达式字符串
        int i = 0;
        //对多位数的拼接
        String str;
        //每遍历到一个字符， 就放入到ch
        char ch;
        do{
            //如果ch是一个非数组，我们需要加入到list集合中
            if((ch = s.charAt(i)) < 48 || (ch = s.charAt(i)) > 57){
                list.add(ch + "");
                //i指针后移
                i++;
            }else{
                //如果是一个数，需要考虑多位数
                //先将str置成空字符串"" '0'[48] -> '9'[57]
                str = "";
                while(i < s.length() && (ch = s.charAt(i)) >= 48 && (ch = s.charAt(i)) <= 57){
                    //拼接多位数
                    str += ch;
                    //i指针后移
                    i++;
                }
                list.add(str);
            }
        }while(i < s.length());
        return list;
    }

    /**
     * 完成对逆波兰表达式的运算
     * 1.从左至右扫描，将3和4压入栈
     * 2.遇到+运算符，因此弹出4和3(4为栈顶元素，3为次顶元素),计算出3+4的值，得7，再将7入栈
     * 3.将5入栈
     * 4.接下来是x运算符，因此弹出5和7，计算出7x5=35,将35入栈
     * 5.将6入栈
     * 6.最后是-运算符，计算出35-6的值，即29，由此得出最终结果
     * @param list
     * @return
     */
    public static int calculate(List<String> list){
        //创建一个栈，只需要一个栈即可
        Stack<String> stack = new Stack<>();
        for(String i:list){
            if(i.matches("\\d+")){
                stack.add(i);
            }else{
                //pop出两个数，并运算，再入栈
                int num2 = Integer.parseInt(stack.pop());
                int num1 = Integer.parseInt(stack.pop());
                int res = 0;
                if(i.equals("+")){
                    res = num1 + num2;
                }else if(i.equals("-")){
                    res = num1 - num2;
                }else if(i.equals("x")){
                    res = num1 * num2;
                }else if(i.equals("/")){
                    res = num1 / num2;
                }else{
                    throw new RuntimeException("您输入的逆波兰表达式有问题不能计算");
                }
                //把结果压入栈中
                stack.push(res+"");
            }
        }
        //最后留在栈中的数据是运算结果
        return Integer.parseInt(stack.pop());
    }
}
//编写一个类Operation可以返回一个运算符对应的优先级
class Operation {
    private static int ADD = 1;
    private static int SUB = 1;
    private static int MUL = 2;
    private static int DIV = 2;

    //写一个方法，返回对应的优先级数字
    public static int getValue(String key){
        int result = 0;
        switch(key){
            case "+":
                result = ADD;
                break;
            case "-":
                result = SUB;
                break;
            case "x":
                result = MUL;
                break;
            case "/":
                result = DIV;
                break;
            default:
                System.out.println("操作符发生了错误");
                break;
        }
        return result;
    }
}
```

### 逆波兰计算器

我们完成一个逆波兰计算器，要求完成如下任务：

1.  输入一个逆波兰表达式(后缀表达式)，使用栈(stack)，计算其结果
2.  支持小括号和多位数整数，因为这里我们主要讲的是数据结构，因此计算器进行简化，只支持对整数的计算
3.  思路分析
    1.  从左至右扫描，将3和4压入栈
    2.  遇到+运算符，因此弹出4和3(4为栈顶元素，3为次顶元素)，计算出3+4的值，得7，再将7入栈
    3.  将5入栈
    4.  接下来是x运算符，因此弹出5和7，计算出7x5=35，将35入栈
    5.  将6入栈
    6.  最后是-运算符，计算出35-6的值，即29，由此得出最终结果
4.  代码完成：

```java
@SuppressWarnings("all")
public class PolandNotation {
    public static void main(String[] args) {
        //定义逆波兰表达式
        //说明：为了方便，逆波兰表达式的数字和符号使用空格隔开
        String suffixExpression = "3 4 + 5 x 6 -";
        /**
         * 1.先将"3 4 + 5 x 6 -" => 放到ArrayList中
         * 2.将ArrayList传递给一个方法，遍历ArrayList 配置栈 完成计算
         */
        List<String> listString = getListString(suffixExpression);
        int calculate = calculate(listString);
        System.out.println(">> 逆波兰表达式运算结果："+calculate);
    }

    /**
     * 将一个逆波兰表达式，依次将数据和运算符，放入到ArrayList中
     * @param suffixExpression
     * @return
     */
    public static List<String> getListString(String suffixExpression){
        //将suffixExpression分割
        List<String> list = new ArrayList<>();
        String[] split = suffixExpression.split(" ");
        for(String ele:split){
            list.add(ele);
        }
        return list;
    }

    /**
     * 完成对逆波兰表达式的运算
     * 1.从左至右扫描，将3和4压入栈
     * 2.遇到+运算符，因此弹出4和3(4为栈顶元素，3为次顶元素),计算出3+4的值，得7，再将7入栈
     * 3.将5入栈
     * 4.接下来是x运算符，因此弹出5和7，计算出7x5=35,将35入栈
     * 5.将6入栈
     * 6.最后是-运算符，计算出35-6的值，即29，由此得出最终结果
     * @param list
     * @return
     */
    public static int calculate(List<String> list){
        //创建一个栈，只需要一个栈即可
        Stack<String> stack = new Stack<>();
        for(String i:list){
            if(i.matches("\\d+")){
                stack.add(i);
            }else{
                //pop出两个数，并运算，再入栈
                int num2 = Integer.parseInt(stack.pop());
                int num1 = Integer.parseInt(stack.pop());
                int res = 0;
                if(i.equals("+")){
                    res = num1 + num2;
                }else if(i.equals("-")){
                    res = num1 - num2;
                }else if(i.equals("x")){
                    res = num1 * num2;
                }else if(i.equals("/")){
                    res = num1 / num2;
                }else{
                    throw new RuntimeException("您输入的逆波兰表达式有问题不能计算");
                }
                //把结果压入栈中
                stack.push(res+"");
            }
        }
        //最后留在栈中的数据是运算结果
        return Integer.parseInt(stack.pop());
    }
}
```

## 递归

**递归应用场景** 

看个实际应用场景，迷宫问题(回溯)， 递归(Recursion)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859123.png)

**递归的概念**  

简单的说: 递归就是方法自己调用自己,每次调用时传入不同的变量.递归有助于编程者解决复杂的问题,同时可以让代码变得简洁。

**递归调用机制** 

我列举两个小案例,来帮助大家理解递归，部分学员已经学习过递归了，这里在给大家回顾一下递归调用机制

1. 打印问题

```java
//输出什么?
public static void test(int n) {
	if (n > 2) {
		test(n - 1);
	}
	System.out.println("n=" + n);
}
```

2. 阶乘问题

```java
//阶乘
public static int factorial(int n) {
	if (n == 1) {

	return 1;

	} else {

	return factorial(n - 1) * n;

	}
}
```

示意图：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859550.png)

**递归能解决什么样的问题** 

递归用于解决什么样的问题

1. 各种数学问题如: 8皇后问题 , 汉诺塔, 阶乘问题, 迷宫问题, 球和篮子的问题(google编程大赛)
2. 各种算法中也会使用到递归，比如快排，归并排序，二分查找，分治算法等.
3. 将用栈解决的问题-->递归代码比较简洁

**递归需要遵守的重要规则** 

递归需要遵守的重要规则

1. 执行一个方法时，就创建一个新的受保护的独立空间(栈空间)
2. 方法的局部变量是独立的，不会相互影响, 比如n变量
3. 如果方法中使用的是引用类型变量(比如数组)，就会共享该引用类型的数据.
4. ==递归必须向退出递归的条件逼近==，否则就是无限递归,出现StackOverflowError，死归了:)
5. 当一个==方法执行完毕，或者遇到return，就会返回，遵守谁调用，就将结果返回给谁==，同时当方法执行完毕或者返回时，该方法也就执行完毕。

### 递归-迷宫问题

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859781.png)

说明:

1. 小球得到的路径，和程序员  设置的找路策略有关即：找  路的上下左右的顺序相关
2. 再得到小球路径时，可以先  使用(下右上左)，再改成(上  右下左)，看看路径是不是有变化
3. 测试回溯现象
	1. 创建一面墙将小球堵住
	```java
	  //使用二维数组模拟迷宫
        //地图
        int[][] arr = new int[8][7];
        //生成地图的地形
        for(int i = 0;i < arr[0].length;i ++){
            arr[0][i] = 1;
            arr[arr.length - 1][i] = 1;
            arr[i][0] = 1;
            arr[i][arr[0].length - 1] = 1;
        }
        //添加地形中的一面阻碍的墙
        arr[3][1] = 1;
        arr[3][2] = 1;
        //添加一面墙将小球堵死
        arr[1][2] = 1;
        arr[2][2] = 1;
	```
4. 思考: 如何求出最短路径?

代码实现：

```java
public class MiGong {
    public static void main(String[] args) {
        //使用二维数组模拟迷宫
        //地图
        int[][] arr = new int[8][7];
        //生成地图的地形
        for(int i = 0;i < arr[0].length;i ++){
            arr[0][i] = 1;
            arr[arr.length - 1][i] = 1;
            arr[i][0] = 1;
            arr[i][arr[0].length - 1] = 1;
        }
        //添加地形中的一面阻碍的墙
        arr[3][1] = 1;
        arr[3][2] = 1;
        //添加一面墙将小球堵死
//        arr[1][2] = 1;
//        arr[2][2] = 1;
        System.out.println("输出地图情况");
        for(int[] temp:arr){
            for(int i:temp){
                System.out.print("\t\t"+i);
            }
            System.out.println();
        }
        boolean flag = setWay01(arr,1,1);
        System.out.println(flag);
        //输出新的地图，小球走过并标识过的地图
        System.out.println("小球走过并标识的 输出地图情况");
        for(int[] temp:arr){
            for(int i:temp){
                System.out.print("\t\t"+i);
            }
            System.out.println();
        }
    }

    /**
     * 说明：
     * 使用递归回溯给小球找路
     * @param arr 表示地图
     * @param i 表示从哪个位置开始找
     * @param j 表示从哪个位置开始找
     *  i j : 表示的是地图中的位置坐标点
     * @return 如果找到了通路返回true 否则返回false
     * 1.规则:
     * 如果小球找到int[6][5]则说明通路找到了
     * 2.约定:
     * 当int[i][j]为0时表示位置没有走过,当为1表示为墙不能越过,2表示通路可以走,3表示该位置已经走过了但是走不通
     * 3.策略:
     * 走迷宫时需要确定一个策略(思考的方法) 下 -> 右 -> 上 -> 左. 如果该点走不通再回溯
     */
    public static boolean setWay(int[][] arr,int i,int j){
        if(arr[6][5] == 2){//通路已经找到
            return true;
        }else{
            if(arr[i][j] == 0){ //判断如果这个点还没走过
                arr[i][j] = 2;
                if(setWay(arr,i+1,j)){ //下走
                    return true;
                }else if(setWay(arr,i,j+1)){ //右走
                    return true;
                }else if(setWay(arr,i-1,j)){ //上走
                    return true;
                }else if(setWay(arr,i,j-1)){ //左走
                    return true;
                }else{ //走不同 标记为3
                    arr[i][j] = 3;
                    //返回false
                    return false;
                }
            }else{
                //如果arr[i][j] != 0，可能是1,2,3
                return false;
            }
        }
    }

    /**
     * 怎么走出迷宫是代码决定的，换一种思路走出迷宫
     * @param arr 表示地图
     * @param i 表示球的位置
     * @param j 表示球的位置
     * @return 返回true表示走出迷宫，false表示没有走出迷宫
     */
    public static boolean setWay01(int[][] arr,int i,int j){
        if(arr[6][5] == 2){
            return true;
        }else if(arr[i][j] == 0){
            arr[i][j] = 2;
            if(setWay01(arr,i-1,j)){//上
                return true;
            }else if(setWay01(arr,i,j+1)){//右
                return true;
            }else if(setWay01(arr,i+1,j)){//下
                return true;
            }else if(setWay01(arr,i,j-1)){//左
                return true;
            }else{
                arr[i][j] = 3;
                return false;
            }
        }else{
            return false;
        }
   }
}
```

### 递归-八皇后问题(回溯算法)

**八皇后问题介绍** 

八皇后问题，是一个古老而著名的问题，是回溯算法的典型案例。该问题是国际西洋棋棋手马克斯·贝瑟尔于1848年提出：在8×8格的国际象棋上摆放八个皇后，使其不能互相攻击，即：任意两个皇后都不能处于同一行、同一列或同一斜线上，问有多少种摆法(92)。

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859594.png)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859391.png)

**八皇后问题算法思路分析** 

1. 第一个皇后先放第一行第一列
2. 第二个皇后放在第二行第一列、然后判断是否OK， 如果不OK，继续放在第二列、第三列、依次把所有列都放完，找到一个合适
3. 继续第三个皇后，还是第一列、第二列……直到第8个皇后也能放在一个不冲突的位置，算是找到了一个正确解
4. 当得到一个正确解时，在栈回退到上一个栈时，就会开始回溯，即将第一个皇后，放到第一列的所有正确解，全部得到.
5. 然后回头继续第一个皇后放第二列，后面继续循环执行 1,2,3,4的步骤 【示意图】

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900978.png)

**说明**：理论上应该创建一个二维数组来表示棋盘，但是实际上可以通过算法，用一个一维数组即可解决问题. arr[8] = {0 , 4, 7, 5, 2, 6, 1, 3} //对应arr 下标 表示第几行，即第几个皇后，arr[i] = val , val 表示第i+1个皇后，放在第i+1行的第val+1列

代码实现：

```java
@SuppressWarnings("all")
public class Bahuanghou {
    //定义一个max表示共有多少个皇后
    int max = 8;
    //定义数组array, 保存皇后放置位置的结果,比如 arr = {0 , 4, 7, 5, 2, 6, 1, 3}
    int[] array = new int[max];
    static int count = 0;
    static int judgeCount = 0;
    public static void main(String[] args) {
        //测试一把 ， 8皇后是否正确
        Bahuanghou queue8 = new Bahuanghou();
        queue8.check(0);
        System.out.printf("一共有%d解法", count);
        System.out.printf("一共判断冲突的次数%d次", judgeCount); // 1.5w

    }

    //编写一个方法，放置第n个皇后
    //特别注意： check 是 每一次递归时，进入到check中都有  for(int i = 0; i < max; i++)，因此会有回溯
    private void check(int n) {
        if(n == max) {  //n = 8 , 其实8个皇后就既然放好
            print();
            return;
        }

        //依次放入皇后，并判断是否冲突
        for(int i = 0; i < max; i++) {
            //先把当前这个皇后 n , 放到该行的第1列
            array[n] = i;
            //判断当放置第n个皇后到i列时，是否冲突
            if(judge(n)) { // 不冲突
                //接着放n+1个皇后,即开始递归
                check(n+1); //
            }
            //如果冲突，就继续执行 array[n] = i; 即将第n个皇后，放置在本行得 后移的一个位置
        }
    }

    /**
     * 查看当我们放置第n个皇后, 就去检测该皇后是否和前面已经摆放的皇后冲突
     * @param n 表示第n个皇后
     * @return
     */
    private boolean judge(int n) {
        judgeCount++;
        for(int i = 0; i < n; i++) {
            // 说明
            //1. array[i] == array[n]  表示判断 第n个皇后是否和前面的n-1个皇后在同一列
            //2. Math.abs(n-i) == Math.abs(array[n] - array[i]) 表示判断第n个皇后是否和第i皇后是否在同一斜线
            // n = 1  放置第 2列 1 n = 1 array[1] = 1
            // Math.abs(1-0) == 1  Math.abs(array[n] - array[i]) = Math.abs(1-0) = 1
            //3. 判断是否在同一行, 没有必要，n 每次都在递增
            if(array[i] == array[n] || Math.abs(n-i) == Math.abs(array[n] - array[i]) ) {
                return false;
            }
        }
        return true;
    }

    //写一个方法，可以将皇后摆放的位置输出
    private void print() {
        count++;
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i] + " ");
        }
        System.out.println();
    }

}
```

## 排序算法的介绍

排序也称排序算法(Sort Algorithm)，排序是将**一  组数据**，依**指定的顺序**进行**排列  的过程**。

排序不是数据结构而是算法

排序的分类：

1.  内部排序:

指将需要处理的所有数据都加载  到**内部存储器**(==内存==)中进行排序。


2.  外部排序法：

**数据量过大**，无法全部加载到内  存中，需要借助**外部存储**(==文件等==)进行


排序。

3.  常见的排序算法分类(见下图):

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859572.png)

### 算法的时间复杂度

度量一个程序(算法)执行时间的两种方法

1. 事后统计的方法  
这种方法可行, 但是有两个问题：一是要想对设计的算法的运行性能进行评测，需要实际运行该程序；二是所得时间的统计量依赖于计算机的硬件、软件等环境因素, 这种方式，要在同一台计算机的相同状态下运行，才能比较那个算法速度更快。


2. 事前估算的方法  
通过分析某个算法的时间复杂度来判断哪个算法更优.

### 算法的时间复杂度

时间频度

Ø基本介绍

时间频度：一个算法花费的时间与算法中语句的执行次数成正比例，哪个算法中语句执行次数多，它花费时间就多。一个算法中的语句执行次数称为语句频度或时间频度。记为T(n)。[举例说明]

Ø**举例说明-基本案例** 

比如计算1-100所有数字之和, 我们设计两种算法：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859900.png)

T(n)=n+1;

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859622.png)

T(n)=1；

Ø**举例说明-忽略常数项** 

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161859777.png)

Ø**举例说明-忽略低次项** 

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900511.png)

Ø**举例说明-忽略系数** 

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900231.png)

**时间复杂度** 

1. 一般情况下，算法中的基本操作语句的重复执行次数是问题规模n的某个函数，用T(n)表示，若有某个辅助函数f(n)，使得当n趋近于无穷大时，T(n) / f(n) 的极限值为不等于零的常数，则称f(n)是T(n)的同数量级函数。记作 T(n)=Ｏ( f(n) )，称Ｏ( f(n) ) 为算法的渐进时间复杂度，简称时间复杂度。


2. T(n) 不同，但时间复杂度可能相同。 如：T(n)=n²+7n+6 与 T(n)=3n²+2n+2 它们的T(n) 不同，但时间复杂度相同，都为O(n²)。

3. 计算时间复杂度的方法：

•用常数1代替运行时间中的所有加法常数 T(n)=n²+7n+6 => T(n)=n²+7n+1

•修改后的运行次数函数中，只保留最高阶项 T(n)=n²+7n+1 => T(n) = n²

•去除最高阶项的系数 T(n) = n²  => T(n) = n² => O(n²)

### 冒泡排序

基本介绍

冒泡排序（Bubble Sorting）的基本思想是：通过对待

排序序列从前向后（从下标较小的元素开始）,依次比较

相邻元素的值，若发现逆序则交换，使值较大

的元素逐渐从前移向后部，就象水底下的气泡一样逐渐

向上冒。

因为排序的过程中，各元素不断接近自己的位置，如果一趟比较下

来没有进行过交换，就说明序列有序，因此要在排序过程中设置

一个标志flag判断元素是否进行过交换。从而减少不必要的比较。(这里说的优化，可以在冒泡排序写好后，在进行)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900791.png)

演示冒泡过程的例子(图解)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900022.png)

冒泡排序应用实例

我们举一个具体的案例来说明冒泡法。我们将五个无序的数：3, 9, -1, 10, -2  使用冒泡排序法将其排成一个从小到大的有序数列。

图解：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900920.png)

代码实现：

先将排序的结构拆分了写，步骤如下：

每次进行排序的时候都需要将长度 - 1 - n 因为排序后范围总是要缩小的否则就会一直来回的排序。

```java
public class BubbleSort01 {
    public static void main(String[] args) {
        int[] arr = {-1,-2,3,1,20,10,9,5};
        int temp = 0;
        for(int i = 0;i < arr.length - 1;i ++){
            if(arr[i] > arr[i+1]){
                temp = arr[i];
                arr[i] = arr[i+1];
                arr[i+1] = temp;
            }
        }
        System.out.println("第一趟排序结果："+ Arrays.toString(arr));
        for(int i = 0;i < arr.length - 1 - 1;i ++){
            if(arr[i] > arr[i+1]){
                temp = arr[i];
                arr[i] = arr[i+1];
                arr[i+1] = temp;
            }
        }
        System.out.println("第二趟排序结果："+Arrays.toString(arr));
        for(int i = 0;i < arr.length - 1 - 2;i ++){
            if(arr[i] > arr[i+1]){
                temp = arr[i];
                arr[i] = arr[i+1];
                arr[i+1] = temp;
            }
        }
        System.out.println("第三趟排序结果："+Arrays.toString(arr));
    }
}
```

简化代码：将排序的过程结合为嵌套循环来完成冒泡排序：

代码如下：

```java
@SuppressWarnings("all")
public class BubbleSort {
    public static void main(String[] args) {
        int temp = 0;
        int[] arr = {-1,-2,3,1,10,9,5};
        //标识变量
        boolean flag = false;
        //冒泡排序 的时间复杂度为 O(n^2)
        for(int i = 0;i < arr.length - 1;i ++){
            for(int j = 0;j < arr.length - i - 1;j ++){
                if(arr[j] > arr[j+1]){
                    //如果进行过交换则将标识变量赋值为true
                    flag = true;
                    temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
            System.out.println("第"+i+"趟，的排序结果为: "+Arrays.toString(arr));
            //判断标识变量进行对应的操作
            if(!flag){
                //在一趟排序中，一次交换都没有发生过
                break;
            }else{
                //如果发生过交换则进入else中，将flag重置进行下次判断
                flag = false;
            }
        }
    }
}
```

### 选择排序

**基本介绍** 

选择式排序也属于内部排序法，是从欲排序的数据中，按指定的规则选出某一元素，再依规定交换位置后达到排序的目的。

**选择排序思想**: 

选择排序（select sorting）也是一种简单的排序方法。它的**基本思想**是：第一次从arr[0]`~`arr[n-1]中选取最小值，与arr[0]交换，第二次从arr[1]`~`arr[n-1]中选取最小值，与arr[1]交换，第三次从arr[2]`~`arr[n-1]中选取最小值，与arr[2]交换，…，第i次从arr[i-1]~arr[n-1]中选取最小值，与arr[i-1]交换，…, 第n-1次从arr[n-2]~arr[n-1]中选取最小值，与arr[n-2]交换，总共通过n-1次，得到一个按排序码从小到大排列的有序序列。

**选择排序思路分析图***: 

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161901514.png)
101, 34, 119, 1

代码实现：

```java
public static void selectionSort(int[] arr) {
   /*判断数组为空或为一个元素的情况，即边界检查*/
   if (arr == null || arr.length < 2) {
      return;
   }

   /*每次要进行比较的两个数，的前面那个数的下标*/
   for (int i = 0; i < arr.length - 1; i++) { 
      //min变量保存该趟比较过程中，最小元素所对应的索引，
      //先假设前面的元素为最小元素
      int minIndex = i;
      /*每趟比较，将前面的元素与其后的元素逐个比较*/
      for (int j = i + 1; j < arr.length; j++) {
         //如果后面的元素小，将后面元素的索引极为最小值的索引
         if(arr[j] < arr[minIndex]) {
            minIndex = j;
         }
      }
      //然后交换此次查找到的最小值和原始的最小值
      swap(arr, i, minIndex);
   }
}

public static void swap(int[] arr, int i, int j) {
   int tmp = arr[i];
   arr[i] = arr[j];
   arr[j] = tmp;
}
```

-  选择排序的优化

```java
public class Demo {
    public static void main(String[] args) {
        int[] arr = {-1,-2,3,1,20,10,9,5};
        sort(arr);
    }
    public static void sort(int[] arr){
        int left = 0;
        int right = arr.length - 1;
        while(left < right){
            int max = right;
            int min = left;
            for(int i = left;i <= right;i ++){
                if(arr[max] < arr[i])
                    max = i;
                if(arr[min] > arr[i])
                    min = i;
            }
            //最大值放在最右端
            int temp = arr[max];
            arr[max] = arr[right];
            arr[right] = temp;

            if(min == right){
                min = max;
            }
            //最下值放在最左端
            temp = arr[min];
            arr[min] = arr[left];
            arr[left] = temp;
            left ++;
            right --;
        }
        System.out.println(Arrays.toString(arr));
    }
}
```

### 插入排序

**插入排序法介绍**：

插入式排序属于内部排序法，是对于预排序的元素以插入的方式找寻元素的适当位置，以达到排序的目的。

**插入排序法思想**:

插入排序（Insertion Sorting）的**基本思想**是：把n个待排序的元素看成为一个有序表和一个无序表，开始时有序表中只包含一个元素，无序表中包含有n-1个元素，排序过程中每次从无序表中取出第一个元素，把它的排序码依次与有序表元素的排序码进行比较，将它插入到有序表中的适当位置，使之成为新的有序表。

**插入排序思路图**：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161901111.png)

代码实现：

```java
public static void main(String[]args){
    int[] ar = {5,3,4,6,2,1};
    //需要使用j循环值来进行位置互换
    int j;
    for(int i = 1;i < ar.length;i ++){
        //取一个值进行比较
        int tmp = ar[i];
        //j = i - 1 循环0等于或到j的数值,j也就是i-1的值,判断是否满足交换条件执行则j-- 
        for(j = i-1;j >= 0 && ar[j] > tmp;j --){
            //将大的元素赋值到索引前
            ar[j+1] = ar[j];
        }
        //将元素小的赋值到索引后,因为 j-- j循环变量在满足条件执行方法体时不会及时执行 j--
        ar[j+1] = tmp;
    }
    System.out.println(Arrays.toString(ar));
}
```

-  优化 - 折半插入排序

```java
public static void main(String[]args){
    int[] arr = {5,3,4,6,2,1};
    int n = arr.length;
    int j,low,mid,high,temp;
    for(int i = 1;i < n;i ++){
        //循环的起始数据
        low = 0;
        //循环的循环数值
        high = i - 1;
        //取第i个元素赋值到tmp中进行比较
        temp = arr[i];
        /*找到合适的插入位置high+1，如果中间位置元素
         *比要插入元素大，则查找区域向低半区移动，否
         *则向高半区移动
         */
        //从前面的元素开始判断到temp的元素如果不满足则直接进行下一次循环
        while(low <= high){
        /*每次执行将low与长度相加的值除2得出mid指针位置*/
            mid = (low+high)/2;
            //判断mid的索引位置的值 是否 大于temp的值
            if(arr[mid] > temp){
                //满足交换条件,指针位置-1,缩短判断范围
                high = mid - 1;
            }else{
                //不满足交换条件 指针位置+1,缩短判断范围
                low = mid + 1;
            }
        }
        /*high+1后的元素后移*/
        //经过上面判断i-1为temp后一位的位置,从high循环到 i
        for(j = i - 1;j >= high + 1;j --) {
            arr[j + 1] = arr[j];
        }
        /*将元素插入到指定位置*/
        arr[j + 1] = temp;
    }
    System.out.println(Arrays.toString(arr));
}
```

-  优化 - 路插入排序

```java
int j, first, last, mid;
int len = arr.length;
/*临时数组*/
int[] tempArr =new int[len];
tempArr[0] = arr[0];
/*first和last分别指临时数组tempArr中排好序的元素的第一个和最后一个位置*/
first = last = 0;

for(int i = 1; i<len; i++){
   /*j 是调整系数*/
   if(first > last){
      j = len;        
   }else{
      j = 0;
   }
   /*tempArr中间元素的位置*/
   mid = ((first+last+j)/2)%len; 
   /*arr[i]应该插入在tempArr的前半部分*/
   if(arr[i] < tempArr[mid]){      
      /*j指向tempArr数组中的第一个元素*/
      j = first;
      /*first 前移，取余是为了实现循环数组效果*/
      first = (first-1+len)%len;  
      /*待插元素大于 j 所指元素*/
      while(arr[i] > tempArr[j]){    
         /*j 所指元素前移，取余是为了实现循环数组效果*/
         tempArr[(j-1+len)%len] = tempArr[j];  
         /*j 指向下一个元素*/
         j = j+1;
      }
      /*移动结束，待插元素插在tempArr[j]前*/
      tempArr[(j-1+len)%len] = arr[i];    
      /*arr[i]应该插入在tempArr的后半部分*/
   }else{
      /*j指向tempArr数组中的最后一个元素*/
      j = last;
      /*last后移， 指向插入后的最后一个元素*/
      last++;
      /*待插元素小于 j 所指元素*/
      while(arr[i] < tempArr[j]){  
         /*j 所指元素后移*/
         tempArr[(j+1)%len] = tempArr[j]; 
         /*j 指向上一个元素*/
         j = (j-1+len)%len;         
      }
      /*移动结束，待插元素插在tempArr[j]后*/
      tempArr[(j+1)%len] = arr[i]; 
   }
}

/*把在tempArr中排好序的元素依次赋给arr*/
for(int i = 0; i < len; i++){                    
   arr[i] = tempArr[(first+i)%len];
}
```

### 希尔排序

**简单插入排序存在的问题** 

我们看简单的插入排序可能存在的问题.

数组 arr = {2,3,4,5,6,1} 这时需要插入的数 1(最小), 这样的过程是：

{2,3,4,5,6,6}

{2,3,4,5,5,6}

{2,3,4,4,5,6}

{2,3,3,4,5,6}

{2,2,3,4,5,6}

{1,2,3,4,5,6}

**结论**:当需要插入的数是较小的数时，后移的次数明显增多，对效率有影响.

希尔排序法介绍

希尔排序是希尔（Donald Shell）于1959年提出的一种排序算法。希尔排序也是一种插入排序，它是简单插入排序经过改进之后的一个更高效的版本，也称为缩小增量排序。

希尔排序法基本思想

希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个文件恰被分成一组，算法便终止

希尔排序法  的示意图

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161901589.png)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161901412.png)

代码实现：

==交换法==（效率较低）

```java
public static void main(String[]args){
    int[] ar = {8,9,1,7,2,3,5,4,6,0};
    art(ar);
}
public static void art(int[] arr){
    int len = arr.length;
    //每次执行外循环将数组的 长度/2 分开
    for (int gap = len/2;gap > 0;gap = gap/2) {
        //从/2的长度开始循环数组全部从/2后的长度开始循环到数组长度
        for (int i = gap;i < len;i++) {
            //将从/2的长度开始进行遍历
            int j = i;
            //判断循环 判断j是否大于等于0 和 数组前后元素的大小
            while (j - gap >= 0 && arr[j] < arr[j - gap]) {
                //调用排序方法
                sort(arr,j,j - gap);
                //当j-gap还是满足if判断条件那么就看第二个条件是否满足如果满足则再次进行交换位置
                j = j - gap;
            }
        }
    }
}
public static void sort(int[] ar,int a,int b){
    int tmp = ar[b];
    ar[b] = ar[a];
    ar[a] = tmp;
    System.out.println(Arrays.toString(ar));
}
```

==移动法==（效率比交换高很多很多）

```java
//希尔排序
public static void shellSort(int[] arr){
   int len = arr.length;
   for(int gap = len / 2;gap > 0;gap = gap / 2){
      for(int  i = gap;i < len;i ++){
         //使用插入排序的思路
         int j = i;
         int temp = arr[j];
         while(j - gap >= 0 && temp < arr[j - gap]){
            arr[j] = arr[j - gap];
            j -= gap;
         }
         arr[j] = temp;
      }
   }
}
```

### 快速排序

**快速排序法介绍**:

快速排序（Quicksort）是对冒泡排序的一种改进。基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列

**快速排序法示意图**:

以每个递归分支的最后一个元素为基准进行排序

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902712.png)

以每个递归分支的最中间的元素为基准进行排序

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902376.png)

代码实现:

```java
@SuppressWarnings("all")
public class QuickSort {
    public static void main(String[] args) {
        int[] arr = {-1,-2,3,1,20,10,9,5};
        quickSort(arr,0,arr.length - 1);
        System.out.println(Arrays.toString(arr));
    }

    public static void quickSort(int[] arr,int left,int right){
        int low = left;//左下标
        int max = right;//右下标
        //中间下标
        int pivot = arr[(left + right) / 2];

        //临时交换值变量
        int temp = 0;
        //while循环的目的是让比pivot值小放到左边比pivot值大的放到右边
        while(low < max){
            //在pivot的左边一直找，找到大于pivot值，才退出
            while(arr[low] < pivot){
                low += 1;
            }

            //在pivot的右边一直找，找到小于pivot值，才退出
            while(arr[max] > pivot){
                max -= 1;
            }

            //如果low >= max说明pivot 的左右两边的值，已经按照左边全部是
            //小于等于pivot的值，右边全部是大于等于pivot的值
            if(low >= max){
                break;
            }

            //交换
            temp = arr[low];
            arr[low] = arr[max];
            arr[max] = temp;

            //如果交换完后，发现这个arr[low] == pivot值 相等 则 max -- ，前移
            if(arr[low] == pivot){
                max -= 1;
            }

            //如果交换完后，发现这个arr[max] == pivot值 相等 则 low ++ ，后移
            if(arr[max] == pivot){
                low += 1;
            }

            //如果low == max,必须low ++ , max -- ,否则会出现堆栈溢出
            if(low == max){
                low += 1;
                max -= 1;
            }

            //向左递归
            if(left < max){
                quickSort(arr,left,max);
            }

            //向右递归
            if(right > low){
                quickSort(arr,low,right);
            }
        }
    }
}
```

### 归并排序

**归并排序介绍**:

归并排序（MERGE-SORT）是利用归并的思想实现的排序方法，该算法采用经典的分治（divide-and-conquer）策略（分治法将问题分(divide)成一些小的问题然后递归求解，而治(conquer)的阶段则将分的阶段得到的各答案"修补"在一起，即分而治之)。

**归并排序思想示意图1-基本思想**:

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902386.png)

**说明**:

可以看到这种结构很像一棵完全二叉树，本文的归并排序我们采用递归去实现（也可采用迭代的方式去实现）。分阶段可以理解为就是递归拆分子序列的过程。

**归并排序思想示意图2-合并相邻有序子序列**:

再来看看治阶段，我们需要将两个已经有序的子序列合并成一个有序序列，比如上图中的最后一次合并，要将[4,5,7,8]和[1,2,3,6]两个已经有序的子序列，合并为最终序列[1,2,3,4,5,6,7,8]，来看下实现步骤

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902315.png)

代码实现：

```java
@SuppressWarnings("all")
public class GuibingSort {
    public static void main(String[] args) {
        int[] arr= {8,4,5,7,1,3,6,2,0};
        //归并排序需要一个额外空间
        int[] temp = new int[arr.length];
        mergeSort(arr,0,arr.length - 1,temp);
        System.out.println(Arrays.toString(arr));
    }

    /**
     * 分+合方法
     * @param arr
     * @param left
     * @param right
     * @param temp
     */
    public static void mergeSort(int[] arr,int left,int right,int[] temp){
        if(left < right){
            //中间索引
            int mid = (left + right) / 2;
            //向左递归进行分解
            mergeSort(arr,left,mid,temp);
            //向右递归进行分解
            mergeSort(arr,mid + 1,right,temp);
            //合并
            merge(arr,left,mid,right,temp);
        }
    }

    /**
     * 合并方法
     * @param arr 排序的原始数据
     * @param left 左边有序序列的初始索引
     * @param mid 中间索引
     * @param right 右边索引
     * @param temp 做中转的数组
     */
    public static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left;//初始化i，左边有序序列的初始索引
        int j = mid + 1;//初始化j，右边有序序列的初始索引
        int t = 0;//指向temp数组的当前索引

        /**
         * (一)
         * 先把左右两边(有序)的数据按照规则填充到temp数组
         * 直到左右两边的有序序列，有一边处理完毕为止
         */
        while (i <= mid && j <= right) {
            /**
             * 如果左边有序序列的当前元素，小于等于右边有序的当前元素
             * 即将左边的当前元素，拷贝到temp数组
             * 然后t ++ i ++
             */
            if (arr[i] <= arr[j]) {
                temp[t] = arr[i];
                t += 1;
                i += 1;
            }else{
                temp[t] = arr[j];
                j += 1;
                t += 1;
            }
        }

        /**
         * (二)
         * 把有剩余数据的一边的数据依次全部填充到temp
         */
        //左边的有序序列还有剩余的元素，就全部填充到temp
        while(i <= mid){
            temp[t] = arr[i];
            t += 1;
            i += 1;
        }
        //右边的有序序列还有剩余的元素，就全部填充到temp
        while(j <= right){
            temp[t] = arr[j];
            t += 1;
            j += 1;
        }

        /**
         * (三)
         * 将temp数组的元素拷贝到arr
         * 注意：并不是每次都拷贝所有的数据
         */
        t = 0;
        int tempLeft = left;
        /**
         * 第一次合并tempLeft = 0, right = 1
         * 第二次合并tempLeft = 2 right = 3
         * 第三次合并tempLeft = 0 right = 3
         * 最后一次tempLeft = 0 right = 7
         */
        while(tempLeft <= right){
            arr[tempLeft] = temp[t];
            t += 1;
            tempLeft += 1;
        }
    }
}
```

### 基数排序

**基数排序(桶排序)介绍**:

1. [基](https://baike.baidu.com/item/%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F/7875498)[数排序](https://baike.baidu.com/item/%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F/7875498)（radix sort）属于“分配式排序”（distribution sort），又称“桶子法”（bucket sort）或bin sort，顾名思义，它是通过键值的各个位的值，将要排序的[元素分配](https://baike.baidu.com/item/%E5%85%83%E7%B4%A0%E5%88%86%E9%85%8D/2107419)至某些“桶”中，达到排序的作用

2. 基数排序法是属于稳定性的排序，基数排序法的是效率高的稳定性排序法

3. 基数排序(Radix Sort)是[桶排序](http://www.cnblogs.com/skywang12345/p/3602737.html)的扩展

4. 基数排序是1887年赫尔曼·何乐礼发明的。它是这样实现的：将整数按位数切割成不同的数字，然后按每个位数分别比较。

**基数排序基本思想** 

1. 将所有待比较数值统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后, 数列就变成一个有序序列。

2. 这样说明，比较难理解，下面我们看一个图文解释，理解基数排序的步骤

**基数排序图文说明** 

将数组 {53, 3, 542, 748, 14, 214} 使用基数排序, 进行升序排序。

第1轮排序 [按照个位排序]:

说明： 事先准备10个数组(10个桶)， 0-9 分别对应 位数的 0-9

1. 将 各个数，按照个位大小 放入到 对应的 各个数组中
2. 然后从 0-9 个数组/桶，依次，按照加入元素的先后顺序取出

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902994.png)

示意图：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902405.png)

分解代码：

```java
@SuppressWarnings("all")
public class JichuSort {
    public static void main(String[] args) {
        int[] arr = {53,3,542,748,14,214};
        radixSort(arr);
    }

    //基数排序方法
    public static void radixSort(int[] arr){

        /**
         * 第一轮（针对每个元素的个位进行排序处理）
         * 定义一个二维数组，表示10个桶，每个桶就是一个一维数组
         * 说明：
         * 1.二维数组包含10个一维数组
         * 2.为了防止在放入数值的时候，数据溢出，则每个一维数组（桶），大小定义为arr.length
         * 3.名明确,基础排序是使用空间换时间的经典算法
         */
        int[][] bucket = new int[10][arr.length];

        /**
         * 为了记录每个桶中，实际存放了多少个数据，我们定义一个一维数组来记录各个桶的每次放入的数据个数
         * 可以这么理解:
         * 比如：bucketElementCounts[0],记录的就是bucket[0]桶的放入数据个数
         */
        int[] bucketElementCounts = new int[10];

        //第一轮（针对每个元素的个位进行排序处理）
        for(int j = 0;j < arr.length;j ++){
            //取出每个元素的个位的值
            int digitOfElement = arr[j] % 10;
            //放入到对应的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement] ++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来数组）
        int index = 0;
        //遍历每一桶，并将桶中的数据，放入到原数组
        for(int k = 0;k < bucketElementCounts.length;k ++){
            //如果桶中，有数据，我们才放入原数组
            if(bucketElementCounts[k] != 0){
                //循环该桶即第k个桶（即第k个一维数组），放入
                for(int l = 0;l < bucketElementCounts[k];l ++){
                    //取出元素放入到arr
                    arr[index ++] = bucket[k][l];
                }
            }
            bucketElementCounts[k] = 0;
        }
        System.out.println("第一轮排序结果："+Arrays.toString(arr));
        //======================================
        //第二轮(针对十位进行排序)
        for(int j = 0;j < arr.length;j ++){
            //取出每个元素的十位的值
            int digitOfElement = arr[j] / 10 % 10;
            //放入到对应的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement] ++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来数组）
        index = 0;
        //遍历每一桶，并将桶中的数据，放入到原数组
        for(int k = 0;k < bucketElementCounts.length;k ++){
            //如果桶中，有数据，我们才放入原数组
            if(bucketElementCounts[k] != 0){
                //循环该桶即第k个桶（即第k个一维数组），放入
                for(int l = 0;l < bucketElementCounts[k];l ++){
                    //取出元素放入到arr
                    arr[index ++] = bucket[k][l];
                }
            }
            bucketElementCounts[k] = 0;
        }
        System.out.println("第二轮排序结果："+Arrays.toString(arr));
        //==========================================
        //第三轮(针对百位进行排序)
        for(int j = 0;j < arr.length;j ++){
            //取出每个元素的百位的值
            int digitOfElement = arr[j] / 100 % 10;
            //放入到对应的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement] ++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来数组）
        index = 0;
        //遍历每一桶，并将桶中的数据，放入到原数组
        for(int k = 0;k < bucketElementCounts.length;k ++){
            //如果桶中，有数据，我们才放入原数组
            if(bucketElementCounts[k] != 0){
                //循环该桶即第k个桶（即第k个一维数组），放入
                for(int l = 0;l < bucketElementCounts[k];l ++){
                    //取出元素放入到arr
                    arr[index ++] = bucket[k][l];
                }
            }
        }
        System.out.println("第三轮排序结果："+Arrays.toString(arr));
    }
}
```

规律：每轮求数值的位数值一直在改变其它不变

基数排序实现：

```java
@SuppressWarnings("all")
public class JichuSort {
    public static void main(String[] args) {
        int[] arr = {53, 3, 542, 748, 14, 214};
        radixSort(arr);
    }

    //基数排序方法
    public static void radixSort(int[] arr) {
        //根据前面的推到过程，我们可以得到最终的基数排序代码
        //1.得到数组中最大的数的位数
        int max = arr[0];//假设第一个元素就是最大为数
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        //得到最大数是几位数
        int maxLength = (max + "").length();

        /**
         * 第一轮（针对每个元素的个位进行排序处理）
         * 定义一个二维数组，表示10个桶，每个桶就是一个一维数组
         * 说明：
         * 1.二维数组包含10个一维数组
         * 2.为了防止在放入数值的时候，数据溢出，则每个一维数组（桶），大小定义为arr.length
         * 3.名明确,基础排序是使用空间换时间的经典算法
         */
        int[][] bucket = new int[10][arr.length];

        /**
         * 为了记录每个桶中，实际存放了多少个数据，我们定义一个一维数组来记录各个桶的每次放入的数据个数
         * 可以这么理解:
         * 比如：bucketElementCounts[0],记录的就是bucket[0]桶的放入数据个数
         */
        int[] bucketElementCounts = new int[10];

        for (int i = 0, n = 1; i < maxLength; i++, n *= 10) {

            //第一轮（针对每个元素的个位进行排序处理）
            for (int j = 0; j < arr.length; j++) {
                //取出每个元素的个位的值
                int digitOfElement = arr[j] / n % 10;
                //放入到对应的桶中
                bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
                bucketElementCounts[digitOfElement]++;
            }
            //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来数组）
            int index = 0;
            //遍历每一桶，并将桶中的数据，放入到原数组
            for (int k = 0; k < bucketElementCounts.length; k++) {
                //如果桶中，有数据，我们才放入原数组
                if (bucketElementCounts[k] != 0) {
                    //循环该桶即第k个桶（即第k个一维数组），放入
                    for (int l = 0; l < bucketElementCounts[k]; l++) {
                        //取出元素放入到arr
                        arr[index++] = bucket[k][l];
                    }
                }
                bucketElementCounts[k] = 0;
            }
            System.out.println("第"+i+"轮排序结果：" + Arrays.toString(arr));
        }
    }
}
```

### 常用排序算法总结和对比

常用排序算法对比

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902636.png)

**相关术语解释**：

1. 稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面；
2. 不稳定：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；
3. 内排序：所有排序操作都在内存中完成；
4. 外排序：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；
5. 时间复杂度： 一个算法执行所耗费的时间。
6. 空间复杂度：运行完一个程序所需内存的大小。
7. ==n==: 数据规模
8. ==k==: “桶”的个数
9. ==In-place==: 不占用额外内存
10. ==Out-place==: 占用额外内存 

## 查找算法

**查找算法介绍** 

在java中，我们常用的查找有四种:

1. 顺序(线性)查找
2. 二分查找/折半查找
3. 插值查找
4. 斐波那契查找

### 线性查找算法

<font style="color:red">要求数组是一个有序的数组</font>.

有一个数列： {1,8, 10, 89, 1000, 1234} ，判断数列中是否包含此名称【顺序查找】  要求: 如果找到了，就提示找到，并给出下标值。  

思路：如果查找到全部符合条件的值。[思路分析.]

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902359.png)

### 二分查找算法

**二分查找**：

<font style="color:red">要求数组是一个有序的数组</font>.

请对一个有序数组进行二分查找 {1,8, 10, 89, 1000, 1234} ，输入一个数看看该数组是否存在此数，并且求出下标，如果没有就提示"没有这个数"。  

课后思考题： {1,8, 10, 89, 1000, 1000，1234} 当一个有序数组中，有多个相同的数值时，如何将所有的数值都查找到，比如这里的 1000.

代码实现：

```java
public class Binary_search {
    //二分查找
    public static void main(String[]args){
        int[] ar = {1,3,5,7,8,9};
        int x = run(ar,8);
        System.out.println(x);
    }
    public static int run(int[] nums,int trans){
        int mid = 0;
        int first = 0;
        int last = nums.length - 1;
        //判断 目标值是否小于索引0的值,目标值是否大于索引长度的值,长度
        if(trans < nums[first] || trans > nums[last] || first > last)
            return -1;
        //判断循环
        while(first <= last){
            //将长度除2的值赋值给mid,起始位置,访问数组中间的数据
            mid = (first + last) / 2;
            //判断数字是否小了
            if(nums[mid] > trans)
                //如果小了则让last - 1然后再次除2判断数字的大小
                last = mid - 1;
            //else只执行一个结果
            else
                //判断数字是否大了
            if(nums[mid] < trans)
                //如果数字大了则first + 1然后再次除2判断数字的大了,每次都是一半一半的进行试探的判断
                first = mid + 1;
            //else只执行一个结果
            else
                //如果两个判断不满足则执行返回值当前的索引位置就是目标值
            return mid;
        }
        //如果全程没有找到则返回 -1
        return -1;
    }
}
```

递归实现：

```java
@SuppressWarnings("all")
public class Chazhao01 {
    public static void main(String[] args) {
        int[] arr = {1,8, 10, 89, 1000, 1000,1234};
        int i = binarySearch(arr, 0, arr.length - 1, 89);
        System.out.println(i);
        List<Integer> i1 = binarySearch1(arr, 0, arr.length - 1, 1000);
        System.out.println("多个值的下标："+i1);
    }

    /**
     * @param arr 数组
     * @param left 左边的索引
     * @param right 右边的索引
     * @param value 要查找的值
     * @return 如果找到就返回下标，否则返回-1
     */
    public static int binarySearch(int[] arr,int left,int right,int value){
        if(left > right){
            return - 1;
        }

        int mid = (left + right) / 2;
        int midValue = arr[mid];
        if(value > midValue){//向右递归
            return binarySearch(arr,mid + 1,right,value);
        }else if(value < midValue){//向左递归
            return binarySearch(arr,left,mid - 1,value);
        }else {
            return mid;
        }
    }

    public static List<Integer> binarySearch1(int[] arr,int left,int right,int value){
        if(left > right){
            return new ArrayList<>();
        }

        int mid = (left + right) / 2;
        int midValue = arr[mid];
        if(value > midValue){//向右递归
            return binarySearch1(arr,mid + 1,right,value);
        }else if(value < midValue){//向左递归
            return binarySearch1(arr,left,mid - 1,value);
        }else{
            /**
             * 思路分析
             * 1.在找到mid索引值，不要马上返回
             * 2.向mid索引值的左边扫描，将所有满足value的元素下标加入集合中
             * 3.向mid索引值的右边扫描，将所有满足value的元素下标加入集合中
             * 4.将集合返回
             */
            List<Integer> list = new ArrayList<>();
            //向mid索引值的左边扫描，将所有满足value的元素的下标加入集合中
            int temp = mid - 1;
            while(true){
                if(temp < 0 || arr[temp] != value){//结束
                    break;
                }
                //否则，将temp放入到集合中
                list.add(temp);
                //temp左移
                temp -= 1;
            }
            list.add(mid);
            temp = mid + 1;
            while(true){
                if(temp > arr.length - 1 || arr[temp] != value){
                    break;
                }
                list.add(temp);
                temp += 1;
            }
            return list;
        }
    }
}
```

### 插值查找算法

**插值查找原理介绍**:

1. 插值查找算法类似于二分查找，不同的是插值查找每次从自适应mid处开始查找。
2. 将折半查找中的求mid 索引的公式 , low 表示左边索引left, high表示右边索引right.  key 就是前面我们讲的 findVal
![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902056.png)
3. int mid = low + (high - low) `*` (key - arr[low]) / (arr[high] -arr[low]) ;/*插值索引*/  
对应前面的代码公式：  
int mid = left + (right – left) * (findVal – arr[left]) / (arr[right] – arr[left])  


4. 举例说明插值查找算法 1-100 的数组

代码实现：

```java
@SuppressWarnings("all")
public class InsertValueSearch {
    public static void main(String[] args) {
        int[] arr = new int[100];
        for(int i = 1;i <= 100;i ++){
            arr[(i - 1)] += i;
        }
        System.out.println(insertValuesearch(arr,0,arr.length - 1,100));
    }

    /**
     * @param arr 数组
     * @param left 左边索引
     * @param right 右边索引
     * @param value 查找值
     * @return 如果找到，返回对应下标，没有返回 -1
     */
    public static int insertValuesearch(int[] arr,int left,int right,int value){
        /**
         * 注意：value < arr[0] 和 value > arr[arr.length - 1] 必须需要
         * 否则我们得到的mid可能越界
         * arr.length - 1 < right 判断传入的right值是否大于了传入数组的长度
         */
        if(left > right || value < arr[0] || value > arr[arr.length - 1] || arr.length - 1 < right){
            return -1;
        }

        //求mid的值 称为自适应写法
        int mid = left + (right - left) * (value - arr[left]) / (arr[right] - arr[left]);

        int midVal = arr[mid];

        if (value > midVal) {
            //向右边递归
            return insertValuesearch(arr,mid + 1,right,value);
        }else if(value < midVal) {
            //向左边递归
            return insertValuesearch(arr,left,mid - 1,value);
        }else{
            return mid;
        }
    }
}
```

<font style="color:red">插入查找注意事项</font>：

1.  对于数据量较大，**关键字分布比较均匀**的查找表来说，采用**插值查找**,**速度较快**.
2.  关键字分布不均匀的情况下，该方法不一定比折半查找要好

### 斐波那契(黄金分割法)查找算法

**斐波那契(黄金分割法)查找基本介绍**:

1. 黄金分割点是指把一条[线段](https://baike.baidu.com/item/%E7%BA%BF%E6%AE%B5/8679802)分割为两部分，使其中一部分与全长之比等于另一部分与这部分之比。取其前三位数字的近似值是[0.618](https://baike.baidu.com/item/0.618)。由于按此比例设计的造型十分美丽，因此称为[黄金分割](https://baike.baidu.com/item/%E9%BB%84%E9%87%91%E5%88%86%E5%89%B2/115896)，也称为[中外比](https://baike.baidu.com/item/%E4%B8%AD%E5%A4%96%E6%AF%94/7590939)。这是一个神奇的数字，会带来意向不大的效果。  

2. 斐波那契数列  {1, 1, 2, 3, 5, 8, 13, 21, 34, 55 } 发现斐波那契数列的两个相邻数 的比例，无限接近 黄金分割值0.618

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902110.png)

斐波那契(黄金分割法)查找算法

**斐波那契(黄金分割法)原理**:

斐波那契查找原理与前两种相似，仅仅  改变了中间结点（mid）的位置，mid不  再是中间或插值得到，而是位于黄金分  割点附近，即mid=low+F(k-1)-1  （F代表斐波那契数列），如下图所示
对F(k-1)-1的理解：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902101.png)

1. 由斐波那契数列 F[k]=F[k-1]+F[k-2] 的性质，可以得到 （F[k]-1）=（F[k-1]-1）+（F[k-2]-1）+1 。该式说明：只要顺序表的长度为F[k]-1，则可以将该表分成长度为F[k-1]-1和F[k-2]-1的两段，即如上图所示。从而中间位置为mid=low+F(k-1)-1
2. 类似的，每一子段也可以用相同的方式分割
3. 但顺序表长度n不一定刚好等于F[k]-1，所以需要将原来的顺序表长度n增加至F[k]-1。这里的k值只要能使得F[k]-1恰好大于或等于n即可，由以下代码得到,顺序表长度增加后，新增的位置（从n+1到F[k]-1位置），都赋为n位置的值即可。

**斐波那契查找应用案例**：

请对一个有序数组进行斐波那契查找 {1,8, 10, 89, 1000, 1234} ，输入一个数看看该数组是否存在此数，并且求出下标，如果没有就提示"没有这个数"。

**代码实现**：

```java
@SuppressWarnings("all")
public class FibonnacciSearch {
    public static int maxSize = 20;
    public static void main(String[] args) {
        int[] arr = {1,8, 10, 89, 1000, 1234};
        int i = fibSearch(arr, 89);
        System.out.println("index : "+i);
        System.out.println(Arrays.toString(fib()));
    }
    /**
     * 因为后面我们mid = low + f[k - 1] - 1;需要使用到斐波那契数列，因此我们需要先获取到一个斐波那契数列
     * 非递归方法得到一个斐波那契数列
     * @return
     */
    public static int[] fib(){
        int[] f = new int[maxSize];
        f[0] = 1;
        f[1] = 1;
        for(int i = 2;i < maxSize;i ++){
            f[i] = f[i - 1] + f[i - 2];
        }
        return f;
    }

    /**
     * 编写斐波那契数列查找算法
     * 使用递归方式编写算法
     * @param a 数组
     * @param key 我们需要查找的关键值
     * @return 返回对应的下标，如果没有返回-1
     */
    public static int fibSearch(int[] a,int key){
        int low = 0;
        int high = a.length - 1;
        int k = 0;//表示斐波那契数列分割数值的下标
        int mid = 0;//存放mid的值
        int[] f = fib();
        //f[k] = 1,1,2,3,5,8 (8 - 1 = 7) ==> false k == 5
        while(high > f[k] - 1){
            k ++;
        }
        /**
         * 因为f[k]值可能大于a的长度，因此我们需要使用Arrays类，构造一个新数组，并指向a[]
         * 不足的部分会使用0填充
         * [1, 8, 10, 89, 1000, 1234, 0, 0]
         */
        int[] temp = Arrays.copyOf(a,f[k]);
        /**
         * 实际上需求使用a数组最后的数填充temp
         * 举例：
         * temp = {1,8, 10, 89, 1000, 1234} => {1,8, 10, 89, 1000, 1234,1234,1234}
         */
        for(int i = high + 1;i < temp.length;i ++){
            temp[i] = a[high];
        }
        //使用while来循环处理，找到我们的数key
        while(low <= high){
            mid = low + f[k - 1] - 1;
            if(key < temp	[mid]){
                high = mid - 1;
                /**
                 * 为什么是 k--
                 * 说明：
                 * 1.全部元素 = 前面的元素 + 后面元素
                 * 2.f[k] = f[k - 1] + f[k - 2];
                 * 因为前面有f[k - 1]个元素，所以可以继续拆分f[k - 1] = f[k - 2] + f[k - 3]
                 * 即在f[k - 1]的前面继续查找k --
                 * 即下次循环mid = f[k - 1 - 1] - 1
                 */
                k--;
            }else if(key > temp[mid]){//我们应该继续向数组的后面查找(右边)
                low = mid + 1;
                /**
                 * 为什么是 k -= 2;
                 * 说明：
                 * 1.全部元素 = 前面元素+后面元素
                 * 2.f[k] = f[k - 1]  + f[k - 2]
                 * 3.因为后面我们有f[k - 2]所以可以继续拆分f[k - 1] = f[k - 3] + f[k - 4]
                 * 4.即在f[k - 2]的前面进行查找k -= 2
                 * 5.即下次循环mid = f[k - 1 - 2] - 1
                 */
                k -= 2;
            }else{
                //需要确定，返回的是哪个下标
                if(mid <= high){
                    return mid;
                }else{
                    return high;
                }
            }
        }
        return -1;
    }
}
```

## 哈希表

**看一个实际需求，google公司的一个上机题**:

有一个公司,当有新的员工来报道时,要求将该员工的信息加入(id,性别,年龄,住址..),当输入该员工的id时,要求查找到该员工的 所有信息.

要求: 不使用数据库,尽量节省内存,速度越快越好  =>  ==哈希表==(散列)

**哈希表的基本介绍** 

[散列表](https://baike.baidu.com/item/%E6%95%A3%E5%88%97%E8%A1%A8)（Hash table，也叫**哈希表**），是根据关键码值(Key value)而直接进行访问的[数据结构](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1450)。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做[散列函数](https://baike.baidu.com/item/%E6%95%A3%E5%88%97%E5%87%BD%E6%95%B0)，存放记录的[数组](https://baike.baidu.com/item/%E6%95%B0%E7%BB%84)叫做[散列](https://baike.baidu.com/item/%E6%95%A3%E5%88%97%E8%A1%A8)[表](https://baike.baidu.com/item/%E6%95%A3%E5%88%97%E8%A1%A8)。

15  111 % 15

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903974.png)

**哈希表(散列)-应用实例** 

**google公司的一个上机题**:

有一个公司,当有新的员工来报道时,要求将该员工的信息加入 (id,性别,年龄,名字,住址..),当输入该员工的id时,要求查找到该员工的 所有信息.

要求:

不使用数据库,速度越快越好 => ==哈希表== (散列)

添加时，保证按照id从低到高插入  [课后思考：如果id不是从 低到高插入，但要求各条链表仍是从低到高，怎么解决?]

1. 使用链表来实现哈希表, 该链表不带表头  [即: 链表的第一个结点就存放雇员信息]
2. 思路分析并画出示意图
3. 代码实现[增删改查(显示所有员工，按id查询)]

示意图：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161903704.png)

代码实现：

```java
@SuppressWarnings("all")
public class HashTableDemo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        HashTable hashTable = new HashTable(7);
        char ch = ' ';
        boolean flag = true;
        while(flag){
            System.out.println("l(list) 查看所有雇员信息");
            System.out.println("a(add) 添加雇员信息到链表");
            System.out.println("s(select) 根据id查找指定雇员");
            System.out.println("d(delet) 根据id删除指定雇员信息,并返回信息");
            System.out.println("D(delet) 根据id删除指定雇员信息");
            System.out.println("e(exit) 退出");
            System.out.println("=======================");
            ch = sc.next().charAt(0);
            switch(ch){
                case 's':
                    System.out.println("请输入需要查找的雇员id");
                    int id1 = sc.nextInt();
                    hashTable.findEmpById(id1);
                    break;
                case 'l':
                    hashTable.list();
                    break;
                case 'a':
                    System.out.println("请输入id");
                    int id = sc.nextInt();
                    System.out.println("请输入名称");
                    String name = sc.next();
                    Emp emp = new Emp(id,name);
                    hashTable.add(emp);
                    break;
                case 'd':
                    try{
                        System.out.println("请输入要删除的雇员id");
                        int id2 = sc.nextInt();
                        Emp emp1 = hashTable.delEmp(id2);
                        System.out.printf(">>>\tData:\t%s\n",emp1);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'D':
                    try {
                        System.out.println("请输入要删除的雇员id");
                        int vlaue = sc.nextInt();
                        hashTable.delEmp01(vlaue);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    sc.close();
                    flag = false;
                    break;
                default:
                    System.out.println("选择存在项");
                    break;
            }
        }
    }
}
@SuppressWarnings("all")
//创建HashTable管理多条链表
class HashTable{
    private EmpLinkedList[] empLinkedListArray;
    //表示共有多少条链表(链表的大小)
    private int size;
    //构造器，初始化链表
    public HashTable(int size) {
        //初始化size值
        this.size = size;
        //初始化数组大小
        empLinkedListArray = new EmpLinkedList[size];
        //TODO 注意: 每一个链表都需要初始化否则报错空指针异常
        for(int i = 0;i < size;i ++){
            empLinkedListArray[i] = new EmpLinkedList();
        }
    }

    //添加雇员
    public void add(Emp emp){
        //根据员工的id，得到该员工应当添加到哪条链表
        int num = hashFun(emp.id);
        //将emp添加到对应的链表中
        empLinkedListArray[num - 1].add(emp);
    }

    //遍历所有的链表(遍历HashTable)
    public void list(){
        for(int i = 0;i < size;i ++){
            empLinkedListArray[i].list(i);
        }
    }

    //编写散列函数，使用一个简单取模法
    public int hashFun(int id){
        return id % size;
    }

    //根据id查找雇员
    public void findEmpById(int id){
        int num = hashFun(id);
        Emp emp = empLinkedListArray[num - 1].findById(id);
        if(emp != null){
            System.out.printf("在第%d个链表中查找到 的雇员信息如下：\n>>> id: %d\n>>> name: %s",num,emp.id,emp.name+"\n");
        }else{
            System.out.println("抱歉>"+num+"<链表中没有找到任何信息");
        }
    }

    public Emp delEmp(int id){
        int num = hashFun(id);
        Emp del = empLinkedListArray[num - 1].del(id);
        return del;
    }


    public void delEmp01(int id){
        int num = hashFun(id);
        empLinkedListArray[num - 1].del01(id);
    }
}
@SuppressWarnings("all")
//表示一个雇员
class Emp{
    public int id;
    public String name;
    public Emp next;

    public Emp(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "id=" + id +
                ", name='" + name + '\'';
    }
}
@SuppressWarnings("all")
//表示链表
class EmpLinkedList{
    //头指针，执行第一个Emp，因此我们这个链表的head是直接指向第一个Emp
    private Emp head;

    /**
     * 添加雇员到链表
     * 说明：
     * 1.假设，当添加雇员时，id是自增长，即id的分配总是从小到大
     * 因此我们将该雇员直接加入到本链表的最后即可
     * @param emp
     */
    public void add(Emp emp){
        //如果是添加第一个雇员
        if(head == null){
            head = emp;
            return;
        }
        //如果不是第一个雇员，则使用一个辅助指针，帮助定位到最后
        Emp curEmp = head;
        while(true){
            //说明到链表最后
            if(curEmp.next == null)
                break;
            //指针后移
            curEmp = curEmp.next;
        }
        //退出时直接将tmp加入到链表即可
        curEmp.next = emp;
    }

    /**
     * 遍历链表的雇员信a
     */
    public void list(int no){
        if(head == null){
            System.out.println("第>"+(no + 1)+"<个链表为空");
            return;
        }
        System.out.println("*-==第>"+(no + 1)+"<个链表中的雇员的信息为==-*");
        Emp curEmp = head;
        while(true){
            System.out.printf(">>> id: %d \n>>> name: %s",curEmp.id,curEmp.name+"\n");
            //判断是否遍历到链表的最后
            if(curEmp.next == null)
                break;
            //指针后移
            curEmp = curEmp.next;
        }
    }

    //根据id查找雇员信息
    public Emp findById(int id){
        if(head == null){
            return null;
        }
        Emp curEmp = head;
        boolean flag = false;
        while(true){
            if(curEmp.id == id){
                flag = true;
                break;
            }
            if(curEmp.next == null){
                break;
            }
            curEmp = curEmp.next;
        }
        if(flag){
            return curEmp;
        }else{
            return null;
        }
    }

    //根据id删除雇员,并返回删除的雇员信息
    public Emp del(int id){
        if(head == null){
            throw new RuntimeException(">>>"+id+"<链表为空删除无效");
        }
        //赋值指针
        Emp curEmp = head;
        //记录上一个位置指针
        Emp pre = head;
        while(true){
            if(curEmp.id == id){
                break;
            }
            if(curEmp == null){
                curEmp = null;
                break;
            }
            //将当前节点赋值给pre
            pre = curEmp;
            //curEmp节点向后移动
            curEmp = curEmp.next;
        }
        //判断当前节点是否为null
        if(curEmp == null){
            return null;
        }
        //判断头节点是否和当前节点相同
        if(head == curEmp){
            //将curEmp.next元素赋值给头节点
            head = curEmp.next;
            return curEmp;
        }
        //将赋值的元素再赋值给pre的next
        pre.next = curEmp.next;
        return curEmp;
    }

    //根据id删除雇员
    public void del01(int id){
        if(head == null){
            throw new RuntimeException(">>>"+id+"<链表为空删除无效");
        }
        Emp curEmp = head;
        boolean flag = false;
        while(true){
            if(curEmp.id == id){
                flag = true;
                break;
            }
            if(curEmp == null){
                break;
            }
            curEmp = curEmp.next;
        }
        if(flag){
            System.out.println("该"+id+"雇员删除成功");
            //判断下一个next节点是否为null
            if(curEmp.next == null){
                //如果下一个next就是null那么就说明了这是第一个元素直接将头元素赋值为null即可
                head = null;
            }else{
                //否则将下一个元素赋值为null
                curEmp.next = null;
            }
        }else{
            System.out.println("您输入的雇员可能本来就不存在");
        }
    }
}
```

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

### 赫夫曼树

**基本介绍** 

1.  给定n个权值作为n个 **叶子结点** ，构造一颗二叉树，若该树的**带权路径长度(wpl)达到最小**，称这样的二叉树为**最优二叉树**，也称为**赫夫曼树(HuffmanTree)**，还有的书翻译为**霍夫曼树**。
2.  赫夫曼树是**带权路径长度最短的树**，**权值较大的节点离根较近**。

**赫夫曼树几个重要概念和举例说明** 

1.  **路径和长度**：在一颗树中，从一个节点往下可以达到的孩子或孙子节点之间的通路，称为路径。通路中分支的数目称为路径长度。若规定根节点的层数为1，则从根节点到第L层节点的路径长度为L-1。
2.  **节点的权及带权路径长度**：若将树中节点赋给一个有着某种含义的数值，则这个数值称为该节点的权。**节点的带权路径长度为**：从根节点到该节点之间的路径长度与该节点的权的乘积。

![image-20230705194848316](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230705194848316.png)

3.  树的带权路径长度：树的带权路径长度规定为所有 **叶子结点** 的带权路径长度之和，记为WPL(weighted path length)，权值越大的节点离根节点越近的二叉树才是最优二叉树。
4.  WPL最小的就是**赫夫曼树** 下图中-中间的就是赫夫曼树

![image-20230705195146344](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230705195146344.png)

**赫夫曼树创建思路图解** 

给你一个数列 {13,7,8,3,29,6,1} ，要求转成一颗赫夫曼树

思路分析示意图：

![image-20230705195844840](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230705195844840.png)

**构成赫夫曼树的步骤**：

1.  从小到大进行排序，将每一个数据，每个数据都是一个节点，每个节点可以看成是一颗最简单的二叉树
2.  取出根节点权值最小的两颗二叉树
3.  组成一颗新的二叉树，该新的二叉树的根节点的权值是前两棵二叉树根节点权值的和
4.  再将这棵新的二叉树，以根节点的权值大小再次排序，不断重复 1-2-3-4 的步骤，直到数列中，所有的数据都被处理，就得到一颗赫夫曼树

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/519:01
 * @comment
 */
public class HuffmanTree {
    public static void main(String[] args) {
        int[] arr = {13,7,8,3,29,6,1};
        Node huffmanTree = createHuffmanTree(arr);
        preOrder(huffmanTree);
    }

    public static void preOrder(Node root){
        if(root != null){
            root.preOrder();
        }else{
            System.out.println("赫夫曼二叉树数据为空");
        }
    }

    /**
     * 创建赫夫曼树方法
     * @param arr
     */
    public static Node createHuffmanTree(int[] arr){
        /**
         * 1.遍历arr数组
         * 2.将arr的每个元素构成一个Node
         * 3.将Node放入到ArrayList中
         */
        List<Node> list = new ArrayList<>();
        for(int value:arr){
            list.add(new Node(value));
        }

        while(list.size() > 1){
            //排序从小打大
            Collections.sort(list);

            //取出节点权值最小的两棵二叉树
            //(1)取出权值最小的节点(二叉树)
            Node leftNode = list.get(0);

            //(2)取出第二小的节点(二叉树)
            Node rightNode = list.get(1);

            //(3)构建成一棵新的二叉树
            Node parent = new Node(leftNode.value + rightNode.value);
            parent.left = leftNode;
            parent.right = rightNode;

            //(4)从ArrayList删除处理过的二叉树
            list.remove(leftNode);
            list.remove(rightNode);
            //(5)将parent加入到list
            list.add(parent);
        }

        //返回赫夫曼树的root节点
        return list.get(0);
    }
}
//创建节点类
class Node implements Comparable<Node>{
    /**
     * value: 节点权值
     * left: 指向左子节点
     * right: 指向右子节点
     */
    int value;
    Node left;
    Node right;

    public Node (int value) {
        this.value = value;
    }

    //前序遍历
    public void preOrder(){
        System.out.println(this);
        if(this.left != null){
            this.left.preOrder();
        }
        if(this.right != null){
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }

    @Override
    public int compareTo(Node o) {
        return this.value - o.value;
    }
}
```

#### 赫夫曼编码

**基本介绍**：

1.  赫夫曼编码也翻译为 **哈夫曼编码(HuffmanCoding)** ，又称霍夫曼编码，是一种编码方式，属于一种程序算法
2.  赫夫曼编码是赫夫曼树在电讯通信中的经典的应用之一。
3.  赫夫曼编码广泛地用于数据文件压缩，其压缩率通常在20% ~ 90%之间
4.  赫夫曼编码是可变字长编码(VLC)的一种，Huffman与1952年提出一种编码方法，称之为最佳编码

原理剖析：

-  通信领域中信息的处理方式1- 定长编码

   -  i like like like java do you like a java    // 共40个字符(包括空格) 

   -  //对应Ascii码

      >  105 32 108 105 107 101 32 108 105 107 101 32 108 105 107 101 32 106 97 118 97 32 100 111 32 121 111 117 32 108 105 107 101 32 97 32 106 97 118 97 

   -  //对应的二进制

      >  01101001 00100000 01101100 01101001 01101011 01100101 00100000 01101100 01101001 01101011 01100101 00100000 01101100 01101001 01101011 01100101 00100000 01101010 01100001 01110110 01100001 00100000 01100100 01101111 00100000 01111001 01101111 01110101 00100000 01101100 01101001 01101011 01100101 00100000 01100001 00100000 01101010 01100001 01110110 01100001 

   -  按照二进制来传递信息，总的长度是 359  (包括空格)

-  通信领域中信息的处理方式2-变长编码

   -  i like like like java do you like a java    // 共40个字符(包括空格)

   -  d:1 y:1 u:1 j:2 v:2 o:2 l:4 k:4 e:4 i:5 a:5  :9 // 各个字符对应的个数

      0= , 1=a, 10=i, 11=e, 100=k, 101=l, 110=o, 111=v, 1000=j, 1001=u, 1010=y, 1011=d

   -  ==说明==：按照各个字符出现的次数进行编码，原则是出现次数越多的，则编码越小，比如 空格出现了9 次， 编码为0 ,其它依次类推.

   -  按照上面给各个字符规定的编码，则我们在传输 "i like like like java do you like a java" 数据时，编码就是

      `10010110100... `

   -  •字符的编码都不能是其他字符编码的前缀，符合此要求的编码叫做前缀编码， 即不能匹配到重复的编码(这个在**赫夫曼编码**中，我们还要进行举例说明, 不捉急)

-  通信领域中信息的处理方式3-赫夫曼编码

   -  d:1 y:1 u:1 j:2 v:2 o:2 l:4 k:4 e:4 i:5 a:5  :9 // 各个字符对应的个数

      按照上面字符出现的次数构建一颗赫夫曼树, 次数作为权值.(图后)

![image-20230706151354255](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230706151354255.png)

//根据赫夫曼树，给各个字符

//规定编码 ， 向左的路径为0

//向右的路径为1 ， 编码如下:

o: 1000  u: 10010 d: 100110 y: 100111 i: 101

a : 110   k: 1110  e: 1111    j: 0000    v: 0001

l: 001     : 01

按照上面的赫夫曼编码，我们的"i like like like java do you like a java"  字符串对应的编码为 (注意这里我们使用的无损压缩)

1010100110111101111010011011110111101001101111011110100001100001110011001111000011001111000100100100110111101111011100100001100001110

**长度为** ： 133 

==说明==:

1.  原来长度是 359 , 压缩了 (359-133) / 359 = 62.9%
2.  此编码满足前缀编码, 即字符的编码都不能是其他字符编码的前缀。不会造成匹配的多义性

==注意==:, 这个赫夫曼树根据排序方法不同，也可能不太一样，**这样对应的赫夫曼编码也不完全一样**，但是wpl 是一样的，都是最小的, 比如: 如果我们让每次生成的新的二叉树总是排在权值相同的二叉树的最后一个，则生成的二叉树为:

![image-20230706151550342](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230706151550342.png)

#### 最佳实践-数据压缩(创建赫夫曼树)

将给出的一段文本，比如 "i like like like java do you like a java" ， 根据前面的讲的赫夫曼编码原理，对其进行数据压缩处理 ，形式如 "1010100110111101111010011011110111101001101111011110100001100001110011001111000011001111000100100100110111101111011100100001100001110"

步骤1：根据赫夫曼编码压缩数据的原理，需要创建 "i like like like java do you like a java" 对应的赫夫曼树.

思路：前面已经分析过了，而且我们已然讲过了构建赫夫曼树的具体实现。

代码实现：

```java
package com.atguigu.huffmancode;

import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;


public class HuffmanCode {

	public static void main(String[] args) {
		String content="i like like like java do you like a java";
		//转成byte数组
		byte[] bytes = content.getBytes();
		//进行赫夫曼编码压缩
		huffmanZip(bytes);
	}
	
	
	private static byte[] huffmanZip(byte[] bytes) {
		//先统计每一个byte出现的次数，并放入一个集合中
		List<Node> nodes = getNodes(bytes);
		//创建一颗赫夫曼树
		Node root = createHuffmanTree(nodes);
		//测试一下 赫夫曼树 是否创建成功!
		//输出root 的 值
		System.out.println(root + " " + root.left + " " + root.right);
		return null;
	}
	
	/**
	 * 
	 * @param bytes byte数组，就是内容字符串对应的byte数组
	 * @return List<Node> 形式如 {Node{'i', 3}, Node{'a', 5}...}
	 */
	private static List<Node> getNodes(byte[] bytes) {
		List<Node> nodes = new ArrayList<>();
		//存储每一个byte出现了多少次。
		Map<Byte, Integer> counts = new HashMap<>();
		//统计每一个byte出现的次数
		for(byte b:bytes) {
			Integer count = counts.get(b);
			if(count==null) {
				counts.put(b, 1);
			}else {
				counts.put(b, count+1);
			}
		}
		//把每一个键值对转为一个node对象,并加入到 nodes集合
		for(Map.Entry<Byte, Integer> entry:counts.entrySet()) {
			nodes.add(new Node(entry.getKey(), entry.getValue()));
		}
		return nodes;
	}
	
	/**
	 * 创建赫夫曼树
	 * @param nodes 传入的是一个node集合
	 * @return 返回赫夫曼树的根节点
	 */
	private static Node createHuffmanTree(List<Node> nodes) {
		
		// 循环处理，
		while (nodes.size() > 1) { // 保证可以get(0) 和 get(1)
			// 排序
			// 说明
			// 1. 需要nodes 集合存放的对象实现 Comparable接口
			Collections.sort(nodes);
			// 取出来权值最小的两个二叉树
			Node left = nodes.get(0);
			// 取出最权值次小的二叉树, 作为新的二叉树的右子树
			Node right = nodes.get(1);
			// 创建一颗新的二叉树 ， 新的节点 data 没有，权值为两颗子树权值和
			Node parent = new Node(null, left.weight + right.weight);
			// 把之前取出来的两颗二叉树设置为新创建的二叉树的子树
			parent.left = left;
			parent.right = right;
			// 把取出来的两个二叉树移除
			nodes.remove(left);
			nodes.remove(right);
			// 放入原来的二叉树集合中
			nodes.add(parent);
		}
		// 返回的节点就是赫夫曼树的根节点.
		return nodes.get(0);
	}


}

//实现 Comparable<Node> 接口是让Node 对象可以进行排序
class Node implements Comparable<Node> {
	Byte data;  // 数据本身 , char 本质就是 Byte
	int weight; // 可以理解成权值, 即字符出现的次数
	Node left;
	Node right;

	public Node(Byte data, int weight) {
		this.data = data;
		this.weight = weight;
	}

	
	@Override
	public int compareTo(Node node) {
		//这样写是从小到大排序
		return this.weight - node.weight;
	}

	
	
	@Override
	public String toString() {
		return "Node [data=" + data + ", weight=" + weight + "]";
	}


	// 前序遍历
	public void preOrder() {
		System.out.println(this);// 先输出父节点
		if (this.left != null) {
			this.left.preOrder();
		}
		if (this.right != null) {
			this.right.preOrder();
		}
	}
}
```

#### 最佳实践-数据压缩(生成赫夫曼编码和赫夫曼编码后的数据)

我们已经生成了 赫夫曼树, 下面我们继续完成任务

1. 生成赫夫曼树对应的赫夫曼编码 , 如下表:  =01 a=100 d=11000 u=11001 e=1110 v=11011 i=101 y=11010 j=0010 k=1111 l=000 o=0011  
2. 使用赫夫曼编码来生成赫夫曼编码数据 ,即按照上面的赫夫曼编码，将"i like like like java do you like a java" 字符串生成对应的编码数据, 形式如下.  
1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100

**思路**：前面已经分析过了，而且我们讲过了生成赫夫曼编码的具体实现。

代码实现：

```java
package com.atguigu.huffmancode;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Set;


public class HuffmanCode {

	public static void main(String[] args) {
		String content="i like like like java do you like a java";
		//转成byte数组
		byte[] bytes = content.getBytes();
		//进行赫夫曼编码压缩
		byte[] huffmanBytes = huffmanZip(bytes);
		
	}
	
	/**
	 * 将 content 进程赫夫曼编码，返回的就是 按 赫夫曼编码后的 byte数组
	 * 比如: "i like like like java do you like a java" => 
	 * 101010011011110111101001101111011110100..  但是 是按照 byte数组来存放的,每8位 放入到一个 byte中!
	 * 比如：huffmanBytes[0] = -41(10101001)  huffmanBytes[1] = -61(10111101) ...
	 * @param bytes
	 * @return
	 */
	private static byte[] huffmanZip(byte[] bytes) {
		//先统计每一个byte出现的次数，并放入一个集合中
		List<Node> nodes = getNodes(bytes);
		//创建一颗赫夫曼树
		Node root = createHuffmanTree(nodes);
		//测试一下 赫夫曼树 是否创建成功!
		//输出root 的 值
		//System.out.println(root + " " + root.left + " " + root.right);
		//创建一个赫夫曼编码表
		Map<Byte, String> huffmanCodes = getCodes(root);
		//测试一把, 看看 	赫夫曼编码表 是否正确
		//32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011
		//System.out.println(huffmanCodes);
		//遍历一把
		Set<Byte> keySet = huffmanCodes.keySet();
		for(Byte key: keySet) {
			System.out.print((char)key.intValue() + "=" + huffmanCodes.get(key) + " ");
		}
		//对 bytes 进行哈夫曼编码，返回的是 一个byte数组
		//"i like like like java do you like a java" 
		//对应的 是 "1010100010111111110.."
		//huffmanBytes[0] = 10101000 = -88
		//huffmanBytes[1] = 10111111 = -65
		byte[] huffmanBytes = zip(bytes,huffmanCodes);
		//测试一个 
		System.out.println("\n" + Arrays.toString(huffmanBytes));
		return huffmanBytes;
		
	}
	
	
	//某个叶子节点存储路径
	static StringBuilder stringBuilder = new StringBuilder();
	//存储赫夫曼编码
	static Map<Byte, String> huffmanCodes = new HashMap<>();
	
	/**
	 * 传入赫夫曼树的根节点，返回对应的赫夫曼编码 Map<Byte, String>
	 * 即形式如(和生成的赫夫曼树有关系)： 	
	 * 			o: 1000   	u: 10010  	d: 100110  		y: 100111  		i: 101
				a: 110     k: 1110    	e: 1111       	j: 0000       	v: 0001
				l: 001 		 : 01

	 * @param tree
	 * @return
	 */
	private static Map<Byte, String> getCodes(Node root) {
		
		if(root==null) {
			return null;
		}
		//处理root的左子树
		getCodes(root.left,"0",stringBuilder);
		//处理root的右子树
		getCodes(root.right,"1",stringBuilder);
		return huffmanCodes;
		
	}
	
	
	/**
	 * 该方法会node 节点的所有叶子节点的赫夫曼编码,并存放在huffmanCodes集合中
	 * 其形式如：{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011}
	 * @param node
	 * @param code
	 * @param stringBuilder
	 */
	private static void getCodes(Node node, String code, StringBuilder stringBuilder) {
		StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
		stringBuilder2.append(code);
		if(node != null) {
			if (node.data == null) {
				getCodes(node.left, "0", stringBuilder2);
				getCodes(node.right, "1", stringBuilder2);
			} else {
				huffmanCodes.put(node.data, stringBuilder2.toString());
			}
		}
	}
	
	/**
	 * 
	 * @param bytes 传入的"i like .." 对应的字节数组，和 哈夫曼编码表
	 * @param huffCodes 返回哈夫曼编码后的byte[]
	 * @return
	 */
	private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {
		StringBuilder stringBuilder = new StringBuilder();
		//把需要压缩的byte数组处理成一个二进制的字符串
		//stringBuilder 就是 形式如 "10101001101111011110..."
		for(byte b:bytes) {
			stringBuilder.append(huffmanCodes.get(b)); 
		}
		System.out.println("stringBuilder(就是哈夫曼编码后的串)=" + stringBuilder);
		//定义就是需要多少个byte来存储，这样就可以定义byte[] 的大小了
		//也可以一句话 len = (stringBuilder.length()+7) / 8
		int len; 
		if (stringBuilder.length() % 8 == 0) {
			len = stringBuilder.length() / 8;
		} else {
			len = stringBuilder.length() / 8 + 1;
		}
		//用于存储压缩后的byte
		byte[] by = new byte[len];
		//记录新byte的位置
		int index = 0;
		for(int i=0;i<stringBuilder.length();i+=8) {
			String strByte;
			if(i+8>stringBuilder.length()) {
				strByte = stringBuilder.substring(i);
			}else {
				strByte = stringBuilder.substring(i, i+8);
			}
			byte byt = (byte)Integer.parseInt(strByte, 2);
			by[index]=byt;
			index++;
		}
		return by;
	}

	/**
	 * 
	 * @param bytes byte数组，就是内容字符串对应的byte数组
	 * @return List<Node> 形式如 {Node{'i', 3}, Node{'a', 5}...}
	 */
	private static List<Node> getNodes(byte[] bytes) {
		List<Node> nodes = new ArrayList<>();
		//存储每一个byte出现了多少次。
		Map<Byte, Integer> counts = new HashMap<>();
		//统计每一个byte出现的次数
		for(byte b:bytes) {
			Integer count = counts.get(b);
			if(count==null) {
				counts.put(b, 1);
			}else {
				counts.put(b, count+1);
			}
		}
		//把每一个键值对转为一个node对象,并加入到 nodes集合
		for(Map.Entry<Byte, Integer> entry:counts.entrySet()) {
			nodes.add(new Node(entry.getKey(), entry.getValue()));
		}
		return nodes;
	}
	
	/**
	 * 创建赫夫曼树
	 * @param nodes 传入的是一个node集合
	 * @return 返回赫夫曼树的根节点
	 */
	private static Node createHuffmanTree(List<Node> nodes) {
		
		// 循环处理，
		while (nodes.size() > 1) { // 保证可以get(0) 和 get(1)
			// 排序
			// 说明
			// 1. 需要nodes 集合存放的对象实现 Comparable接口
			Collections.sort(nodes);
			// 取出来权值最小的两个二叉树
			Node left = nodes.get(0);
			// 取出最权值次小的二叉树, 作为新的二叉树的右子树
			Node right = nodes.get(1);
			// 创建一颗新的二叉树 ， 新的节点 data 没有，权值为两颗子树权值和
			Node parent = new Node(null, left.weight + right.weight);
			// 把之前取出来的两颗二叉树设置为新创建的二叉树的子树
			parent.left = left;
			parent.right = right;
			// 把取出来的两个二叉树移除
			nodes.remove(left);
			nodes.remove(right);
			// 放入原来的二叉树集合中
			nodes.add(parent);
		}
		// 返回的节点就是赫夫曼树的根节点.
		return nodes.get(0);
	}


}

//实现 Comparable<Node> 接口是让Node 对象可以进行排序
class Node implements Comparable<Node> {
	Byte data;  // 数据本身 , char 本质就是 Byte
	int weight; // 可以理解成权值, 即字符出现的次数
	Node left;
	Node right;

	public Node(Byte data, int weight) {
		this.data = data;
		this.weight = weight;
	}

	
	@Override
	public int compareTo(Node node) {
		//这样写是从小到大排序
		return this.weight - node.weight;
	}

	
	
	@Override
	public String toString() {
		return "Node [data=" + data + ", weight=" + weight + "]";
	}


	// 前序遍历
	public void preOrder() {
		System.out.println(this);// 先输出父节点
		if (this.left != null) {
			this.left.preOrder();
		}
		if (this.right != null) {
			this.right.preOrder();
		}
	}
}
```

#### 最佳实践-数据解压(使用赫夫曼编码解码)

使用赫夫曼编码来解码数据，具体要求是

1. 前面我们得到了赫夫曼编码和对应的编码byte[] , 即:[-88, -65, -56, -65, -56, -65, -55, 77  
, -57, 6, -24, -14, -117, -4, -60, -90, 28]  
2. 现在要求使用赫夫曼编码， 进行解码，又  
重新得到原来的字符串"i like like likejava do you like a java"

**思路**：解码过程，就是编码的一个逆向操作。

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/521:26
 * @comment
 */
@SuppressWarnings("all")
public class HuffmanTreeCode {
    public static void main(String[] args) {
        String content = "i like like like java do you like a java";
        byte[] contentBytes = content.getBytes();
        System.out.println(contentBytes.length);//40
        byte[] huffmanCodeBytes = huffmanZip(contentBytes);
        System.out.println("压缩后的结果是：" + Arrays.toString(huffmanCodeBytes));

        byte[] sourceBytes = decode(huffmanCodes, huffmanCodeBytes);

        System.out.println("原来的字符串" + new String(sourceBytes));

        //分步过程
        /*
        List<Node01> list = getNodes(contentBytes);
        System.out.println(list);

        System.out.println("----------------赫夫曼树----------------");
        Node01 huffmanTree = createHuffmanTree(list);
        System.out.println("前序遍历");
        preOrder(huffmanTree);

        //测试是否生成了对应的赫夫曼编码
        Map<Byte, String> codes = getCodes(huffmanTree);
        System.out.println("生成的赫夫曼编码表："+codes);
        byte[] huffmanCodeBytes = zip(contentBytes,huffmanCodes);
        System.out.println("huffmanCodeBytes: "+Arrays.toString(huffmanCodeBytes));
         */
    }

    /**
     * 编写一个方法，完成对压缩数据的解码
     *
     * @param huffmanCodes 赫夫曼编码表 map
     * @param huffmanBytes 赫夫曼编码得到的字节数组
     * @return 就是原来的字符串对应的数组
     */
    private static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < huffmanBytes.length; i++) {
            byte b = huffmanBytes[i];
            boolean flag = (i == huffmanBytes.length - 1);
            stringBuilder.append(byteToBitString(!flag, b));
        }
//        System.out.println("赫夫曼字节数组对应的二进制字符串："+stringBuilder.toString());
        //把字符串按照指定的赫夫曼编码进行解码
        //把赫夫曼编码表进行调换，因为反向查询 a -> 100 100 -> a
        Map<String, Byte> map = new HashMap<>();
        for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
            map.put(entry.getValue(), entry.getKey());
        }

        //创建集合，存放byte
        List<Byte> list = new ArrayList<>();
        for (int i = 0; i < stringBuilder.length(); ) {
            int count = 1;//小的计数器
            boolean flag = true;
            Byte b = null;
            while (flag) {
                //先取出一个'1','0'
                String key = stringBuilder.substring(i, i + count);//i不动，让count移动 匹配字符
                b = map.get(key);
                if (b == null) {//b == null说明没有匹配到
                    count++;
                } else {//说明 匹配到了
                    flag = false;
                }
            }
            list.add(b);
            i += count;//i 直接移动到count位置
        }
        //当for循环结束后list中就存放了所有的字符 i like like like java do you like a java
        //把list中 的数据放入byte数组并返回
        byte[] b = new byte[list.size()];
        for (int i = 0; i < b.length; i++) {
            b[i] = list.get(i);
        }
        return b;
    }

    /**
     * 将一个byte转成一个二进制的字符串
     *
     * @param flag 标志是否需要补高位如果是true，需要，false，不需要
     * @param b    传入的byte
     * @return b 对应的二进制的字符串(注意是按补码返回)
     */
    private static String byteToBitString(boolean flag, byte b) {
        //使用变量保存b,将b转换成int
        int temp = b;
        //判断是正数则需要补高位
        if (flag) {
            temp |= 256;
            //说明：按位与 256 1 0000 0000 | 0000 0001 => 1 0000 0001
        }
        //返回的是temp对应的二进制的补码
        String str = Integer.toBinaryString(temp);
        if (flag) {
            return str.substring(str.length() - 8);
        } else {
            return str;
        }
    }

    //使用一个方法，将前面的方法封装起来，便于我们的调用

    /**
     * @param bytes 原始的字符串对应的字节数组
     * @return 是经过赫夫曼编码处理后的字节数组（压缩后的数组）
     */
    private static byte[] huffmanZip(byte[] bytes) {
        List<Node01> nodes = getNodes(bytes);
        //根据nodes创建的赫夫曼树
        Node01 huffmanTreeRoot = createHuffmanTree(nodes);
        //对应的赫夫曼编码(根据 赫夫曼树)
        Map<Byte, String> huffmanCodes = getCodes(huffmanTreeRoot);
        //根据生成的赫夫曼编码，压缩得到压缩后的赫夫曼编码字节数组
        byte[] huffmanCodeBytes = zip(bytes, huffmanCodes);
        return huffmanCodeBytes;
    }

    //编写一个方法，将字符串对应的byte[] 数组，通过生成的赫夫曼编码表，返回一个赫夫曼编码 压缩后的byte[]

    /**
     * @param bytes        这时原始的字符串对应的 byte[]
     * @param huffmanCodes 生成的赫夫曼编码map
     * @return 返回赫夫曼编码处理后的 byte[]
     * 举例： String content = "i like like like java do you like a java"; =》 byte[] contentBytes = content.getBytes();
     * 返回的是 字符串 "1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100"
     * => 对应的 byte[] huffmanCodeBytes  ，即 8位对应一个 byte,放入到 huffmanCodeBytes
     * huffmanCodeBytes[0] =  10101000(补码) => byte  [推导  10101000=> 10101000 - 1 => 10100111(反码)=> 11011000= -88 ]
     * huffmanCodeBytes[1] = -88
     */
    private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {

        //1.利用 huffmanCodes 将  bytes 转成  赫夫曼编码对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        //遍历bytes 数组
        for (byte b : bytes) {
            stringBuilder.append(huffmanCodes.get(b));
        }

        //System.out.println("测试 stringBuilder~~~=" + stringBuilder.toString());

        //将 "1010100010111111110..." 转成 byte[]

        //统计返回  byte[] huffmanCodeBytes 长度
        //一句话 int len = (stringBuilder.length() + 7) / 8;
        int len;
        if (stringBuilder.length() % 8 == 0) {
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建 存储压缩后的 byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;//记录是第几个byte
        for (int i = 0; i < stringBuilder.length(); i += 8) { //因为是每8位对应一个byte,所以步长 +8
            String strByte;
            if (i + 8 > stringBuilder.length()) {//不够8位
                strByte = stringBuilder.substring(i);
            } else {
                strByte = stringBuilder.substring(i, i + 8);
            }
            //将strByte 转成一个byte,放入到 huffmanCodeBytes
            huffmanCodeBytes[index] = (byte) Integer.parseInt(strByte, 2);
            index++;
        }
        return huffmanCodeBytes;
    }

    //生成赫夫曼树对应的赫夫曼编码
    //思路：
    //1.将赫夫曼编码表存放在Map<Byte,String> 形式
    //{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011} 等等[形式]
    static Map<Byte, String> huffmanCodes = new HashMap<>();
    //2.在生成赫夫曼编码表时，需要去拼接路径，定义一个StringBuilder，存储某个叶子节点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    private static Map<Byte, String> getCodes(Node01 root) {
        if (root == null) {
            return null;
        }
        //处理root的左子树
        getCodes(root.left, "0", stringBuilder);
        //处理root的右子树
        getCodes(root.right, "1", stringBuilder);
        return huffmanCodes;
    }

    /**
     * 功能：将传入的Node节点的所有叶子节点的赫夫曼编码存放到，并放入到huffmanCodes集合
     *
     * @param node          传入节点
     * @param code          路径：左子节点是0，右子节点是1
     * @param stringBuilder 用于拼接路径
     */
    private static void getCodes(Node01 node, String code, StringBuilder stringBuilder) {
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code，加入到stringBuilder2中
        stringBuilder2.append(code);
        if (node != null) {
            //判断当前node,是叶子节点还是非叶子节点
            if (node.data == null) {
                //非叶子节点
                //递归处理
                //向左递归
                getCodes(node.left, "0", stringBuilder2);
                //向右递归
                getCodes(node.right, "1", stringBuilder2);
            } else {
                //说明是叶子节点
                //表示找到了某个叶子节点的最后
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }

    //前序遍历
    private static void preOrder(Node01 root) {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("赫夫曼树为空");
        }
    }

    /**
     * @param bytes bytes 接受字节数组
     * @return 返回的就是List形式
     */
    private static List<Node01> getNodes(byte[] bytes) {
        //1.创建一个ArrayList
        List<Node01> list = new ArrayList<>();
        //遍历list，统计每一个byte出现的次数 -> map[key,value]
        Map<Byte, Integer> map = new HashMap<>();
        for (byte b : bytes) {
            Integer count = map.get(b);
            //Map还没有这个字符数据，第一次则执行如下
            if (count == null) {
                map.put(b, 1);
            } else {//已经存在了执行如下
                map.put(b, count + 1);
            }
        }

        //把每一个键值对转成一个Node对象，并加入list集合
        //遍历map
        for (Map.Entry<Byte, Integer> entry : map.entrySet()) {
            list.add(new Node01(entry.getKey(), entry.getValue()));
        }

        return list;
    }

    //通过List创建赫夫曼树
    private static Node01 createHuffmanTree(List<Node01> list) {

        while (list.size() > 1) {
            //排序，从小到大
            Collections.sort(list);
            //取出第一棵最小的二叉树
            Node01 leftNode = list.get(0);
            //取出第二棵最小的二叉树
            Node01 rightNode = list.get(1);
            //创建一颗新的二叉树，它的根节点 没有data，只有权值
            Node01 parent = new Node01(null, leftNode.weight + rightNode.weight);
            parent.left = leftNode;
            parent.right = rightNode;
            //将处理过的两颗二叉树从list中移除
            list.remove(leftNode);
            list.remove(rightNode);
            //将新的二叉树加入到list集合中
            list.add(parent);
        }
        return list.get(0);
    }
}

@SuppressWarnings("all")
class Node01 implements Comparable<Node01> {
    Byte data;
    int weight;
    Node01 left;
    Node01 right;

    public Node01(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node01 o) {
        return this.weight - o.weight;
    }

    public void preOrder() {
        System.out.println(this);
        if (this.left != null) {
            this.left.preOrder();
        }
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node[data = " + data + " , weight = " + weight + "]";
    }
}
```

#### 最佳实践-文件压缩

我们学习了通过赫夫曼编码对一个字符串进行编码和解码, 下面我们来完成对文件的压缩和解压， 具体要求：给你一个图片文件，要求对其进行无损压缩, 看看压缩效果如何。

**思路**：读取文件-> 得到赫夫曼编码表 -> 完成压缩

代码实现：

```java
/**
     * 编写方法，将一个文件进行压缩
     * @param srcFile 你传入的希望压缩的文件的全路径
     * @param dstFile 我们压缩后将压缩文件放到哪个目录
     */
private static void zipFile(String srcFile,String dstFile){
   //创建输出流
   OutputStream os = null;
   ObjectOutputStream oos = null;
   //创建文件的输入流
   FileInputStream is = null;
   try{
      //创建文件的输入流
      is = new FileInputStream(srcFile);
      //创建一个和源文件大小一样的byte[]
      byte[] b = new byte[is.available()];
      //读取文件
      is.read(b);
      //获取到文件对应的赫夫曼编码表
      //直接对原文件进行压缩
      byte[] huffmanBytes = huffmanZip(b);
      //创建文件的输出流,存放压缩文件
      os = new FileOutputStream(dstFile);
      //创建一个和文件输出流关联的ObjectOutputStream
      oos = new ObjectOutputStream(os);
      //把赫夫曼编码后的字节数组写入压缩文件
      oos.writeObject(huffmanBytes);
      //以对象流形式写入赫夫曼编码，为了解压(恢复源文件)时使用
      //注意：一定要把赫夫曼编码，写入压缩文件
      oos.writeObject(huffmanCodes);
   }catch(IOException e){
      System.out.println(e.getMessage());
   }finally{
      //关闭流，释放资源
      try{
         if(is != null){
            is.close();
            os.close();
            oos.close();
         }
      }catch(IOException e){
         System.out.println(e.getMessage());
      }
   }
}
```

完整代码：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/521:26
 * @comment
 */
@SuppressWarnings("all")
public class HuffmanTreeCode {
    public static void main(String[] args) {
        //测试压缩文件
        String srcFile = "E://src.bmp";
        String dstFile = "E://dst.zip";
        zipFile(srcFile,dstFile);
        System.out.println("压缩文件OK");
    }

    /**
     * 编写方法，将一个文件进行压缩
     * @param srcFile 你传入的希望压缩的文件的全路径
     * @param dstFile 我们压缩后将压缩文件放到哪个目录
     */
    private static void zipFile(String srcFile,String dstFile){
        //创建输出流
        OutputStream os = null;
        ObjectOutputStream oos = null;
        //创建文件的输入流
        FileInputStream is = null;
        try{
            //创建文件的输入流
            is = new FileInputStream(srcFile);
            //创建一个和源文件大小一样的byte[]
            byte[] b = new byte[is.available()];
            //读取文件
            is.read(b);
            //获取到文件对应的赫夫曼编码表
            //直接对原文件进行压缩
            byte[] huffmanBytes = huffmanZip(b);
            //创建文件的输出流,存放压缩文件
            os = new FileOutputStream(dstFile);
            //创建一个和文件输出流关联的ObjectOutputStream
            oos = new ObjectOutputStream(os);
            //把赫夫曼编码后的字节数组写入压缩文件
            oos.writeObject(huffmanBytes);
            //以对象流形式写入赫夫曼编码，为了解压(恢复源文件)时使用
            //注意：一定要把赫夫曼编码，写入压缩文件
            oos.writeObject(huffmanCodes);
        }catch(IOException e){
            System.out.println(e.getMessage());
        }finally{
            //关闭流，释放资源
            try{
                if(is != null){
                    is.close();
                    os.close();
                    oos.close();
                }
            }catch(IOException e){
                System.out.println(e.getMessage());
            }
        }
    }

    /**
     * 编写一个方法，完成对压缩数据的解码
     *
     * @param huffmanCodes 赫夫曼编码表 map
     * @param huffmanBytes 赫夫曼编码得到的字节数组
     * @return 就是原来的字符串对应的数组
     */
    private static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < huffmanBytes.length; i++) {
            byte b = huffmanBytes[i];
            boolean flag = (i == huffmanBytes.length - 1);
            stringBuilder.append(byteToBitString(!flag, b));
        }
//        System.out.println("赫夫曼字节数组对应的二进制字符串："+stringBuilder.toString());
        //把字符串按照指定的赫夫曼编码进行解码
        //把赫夫曼编码表进行调换，因为反向查询 a -> 100 100 -> a
        Map<String, Byte> map = new HashMap<>();
        for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
            map.put(entry.getValue(), entry.getKey());
        }

        //创建集合，存放byte
        List<Byte> list = new ArrayList<>();
        for (int i = 0; i < stringBuilder.length(); ) {
            int count = 1;//小的计数器
            boolean flag = true;
            Byte b = null;
            while (flag) {
                //先取出一个'1','0'
                String key = stringBuilder.substring(i, i + count);//i不动，让count移动 匹配字符
                b = map.get(key);
                if (b == null) {//b == null说明没有匹配到
                    count++;
                } else {//说明 匹配到了
                    flag = false;
                }
            }
            list.add(b);
            i += count;//i 直接移动到count位置
        }
        //当for循环结束后list中就存放了所有的字符 i like like like java do you like a java
        //把list中 的数据放入byte数组并返回
        byte[] b = new byte[list.size()];
        for (int i = 0; i < b.length; i++) {
            b[i] = list.get(i);
        }
        return b;
    }

    /**
     * 将一个byte转成一个二进制的字符串
     *
     * @param flag 标志是否需要补高位如果是true，需要，false，不需要
     * @param b    传入的byte
     * @return b 对应的二进制的字符串(注意是按补码返回)
     */
    private static String byteToBitString(boolean flag, byte b) {
        //使用变量保存b,将b转换成int
        int temp = b;
        //判断是正数则需要补高位
        if (flag) {
            temp |= 256;
            //说明：按位与 256 1 0000 0000 | 0000 0001 => 1 0000 0001
        }
        //返回的是temp对应的二进制的补码
        String str = Integer.toBinaryString(temp);
        if (flag) {
            return str.substring(str.length() - 8);
        } else {
            return str;
        }
    }

    //使用一个方法，将前面的方法封装起来，便于我们的调用

    /**
     * @param bytes 原始的字符串对应的字节数组
     * @return 是经过赫夫曼编码处理后的字节数组（压缩后的数组）
     */
    private static byte[] huffmanZip(byte[] bytes) {
        List<Node01> nodes = getNodes(bytes);
        //根据nodes创建的赫夫曼树
        Node01 huffmanTreeRoot = createHuffmanTree(nodes);
        //对应的赫夫曼编码(根据 赫夫曼树)
        Map<Byte, String> huffmanCodes = getCodes(huffmanTreeRoot);
        //根据生成的赫夫曼编码，压缩得到压缩后的赫夫曼编码字节数组
        byte[] huffmanCodeBytes = zip(bytes, huffmanCodes);
        return huffmanCodeBytes;
    }

    //编写一个方法，将字符串对应的byte[] 数组，通过生成的赫夫曼编码表，返回一个赫夫曼编码 压缩后的byte[]

    /**
     * @param bytes        这时原始的字符串对应的 byte[]
     * @param huffmanCodes 生成的赫夫曼编码map
     * @return 返回赫夫曼编码处理后的 byte[]
     * 举例： String content = "i like like like java do you like a java"; =》 byte[] contentBytes = content.getBytes();
     * 返回的是 字符串 "1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100"
     * => 对应的 byte[] huffmanCodeBytes  ，即 8位对应一个 byte,放入到 huffmanCodeBytes
     * huffmanCodeBytes[0] =  10101000(补码) => byte  [推导  10101000=> 10101000 - 1 => 10100111(反码)=> 11011000= -88 ]
     * huffmanCodeBytes[1] = -88
     */
    private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {

        //1.利用 huffmanCodes 将  bytes 转成  赫夫曼编码对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        //遍历bytes 数组
        for (byte b : bytes) {
            stringBuilder.append(huffmanCodes.get(b));
        }

        //System.out.println("测试 stringBuilder~~~=" + stringBuilder.toString());

        //将 "1010100010111111110..." 转成 byte[]

        //统计返回  byte[] huffmanCodeBytes 长度
        //一句话 int len = (stringBuilder.length() + 7) / 8;
        int len;
        if (stringBuilder.length() % 8 == 0) {
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建 存储压缩后的 byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;//记录是第几个byte
        for (int i = 0; i < stringBuilder.length(); i += 8) { //因为是每8位对应一个byte,所以步长 +8
            String strByte;
            if (i + 8 > stringBuilder.length()) {//不够8位
                strByte = stringBuilder.substring(i);
            } else {
                strByte = stringBuilder.substring(i, i + 8);
            }
            //将strByte 转成一个byte,放入到 huffmanCodeBytes
            huffmanCodeBytes[index] = (byte) Integer.parseInt(strByte, 2);
            index++;
        }
        return huffmanCodeBytes;
    }

    //生成赫夫曼树对应的赫夫曼编码
    //思路：
    //1.将赫夫曼编码表存放在Map<Byte,String> 形式
    //{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011} 等等[形式]
    static Map<Byte, String> huffmanCodes = new HashMap<>();
    //2.在生成赫夫曼编码表时，需要去拼接路径，定义一个StringBuilder，存储某个叶子节点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    private static Map<Byte, String> getCodes(Node01 root) {
        if (root == null) {
            return null;
        }
        //处理root的左子树
        getCodes(root.left, "0", stringBuilder);
        //处理root的右子树
        getCodes(root.right, "1", stringBuilder);
        return huffmanCodes;
    }

    /**
     * 功能：将传入的Node节点的所有叶子节点的赫夫曼编码存放到，并放入到huffmanCodes集合
     *
     * @param node          传入节点
     * @param code          路径：左子节点是0，右子节点是1
     * @param stringBuilder 用于拼接路径
     */
    private static void getCodes(Node01 node, String code, StringBuilder stringBuilder) {
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code，加入到stringBuilder2中
        stringBuilder2.append(code);
        if (node != null) {
            //判断当前node,是叶子节点还是非叶子节点
            if (node.data == null) {
                //非叶子节点
                //递归处理
                //向左递归
                getCodes(node.left, "0", stringBuilder2);
                //向右递归
                getCodes(node.right, "1", stringBuilder2);
            } else {
                //说明是叶子节点
                //表示找到了某个叶子节点的最后
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }

    //前序遍历
    private static void preOrder(Node01 root) {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("赫夫曼树为空");
        }
    }

    /**
     * @param bytes bytes 接受字节数组
     * @return 返回的就是List形式
     */
    private static List<Node01> getNodes(byte[] bytes) {
        //1.创建一个ArrayList
        List<Node01> list = new ArrayList<>();
        //遍历list，统计每一个byte出现的次数 -> map[key,value]
        Map<Byte, Integer> map = new HashMap<>();
        for (byte b : bytes) {
            Integer count = map.get(b);
            //Map还没有这个字符数据，第一次则执行如下
            if (count == null) {
                map.put(b, 1);
            } else {//已经存在了执行如下
                map.put(b, count + 1);
            }
        }

        //把每一个键值对转成一个Node对象，并加入list集合
        //遍历map
        for (Map.Entry<Byte, Integer> entry : map.entrySet()) {
            list.add(new Node01(entry.getKey(), entry.getValue()));
        }

        return list;
    }

    //通过List创建赫夫曼树
    private static Node01 createHuffmanTree(List<Node01> list) {

        while (list.size() > 1) {
            //排序，从小到大
            Collections.sort(list);
            //取出第一棵最小的二叉树
            Node01 leftNode = list.get(0);
            //取出第二棵最小的二叉树
            Node01 rightNode = list.get(1);
            //创建一颗新的二叉树，它的根节点 没有data，只有权值
            Node01 parent = new Node01(null, leftNode.weight + rightNode.weight);
            parent.left = leftNode;
            parent.right = rightNode;
            //将处理过的两颗二叉树从list中移除
            list.remove(leftNode);
            list.remove(rightNode);
            //将新的二叉树加入到list集合中
            list.add(parent);
        }
        return list.get(0);
    }
}

@SuppressWarnings("all")
class Node01 implements Comparable<Node01> {
    Byte data;
    int weight;
    Node01 left;
    Node01 right;

    public Node01(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node01 o) {
        return this.weight - o.weight;
    }

    public void preOrder() {
        System.out.println(this);
        if (this.left != null) {
            this.left.preOrder();
        }
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node[data = " + data + " , weight = " + weight + "]";
    }
}
```

#### 最佳实践——文件解压(文件恢复)

具体要求：将前面压缩的文件，重新恢复成原来的文件。

思路：读取压缩文件(数据和赫夫曼编码表) -> 完成解压(文件恢复)

```java
/**
     * 完成对压缩文件的解压
     * @param zipFile 准备解压的文件
     * @param dstFile 将文件解压到哪个路径
     */
private static void unZipFile(String zipFile,String dstFile){
   InputStream is = null;
   ObjectInputStream ois = null;
   OutputStream os = null;
   try{
      is = new FileInputStream(zipFile);
      ois = new ObjectInputStream(is);
      //读取byte数组 huffmanBytes
      byte[] huffmanBytes = (byte[]) ois.readObject();
      //读取赫夫曼编码表
      Map<Byte,String> huffmanCodes = (Map<Byte,String>) ois.readObject();
      //解码
      byte[] bytes = decode(huffmanCodes,huffmanBytes);
      //将bytes,数组写入到目标文件
      os = new FileOutputStream(dstFile);
      //写出数据到文件中
      os.write(bytes);
   }catch(Exception e){
      System.out.println(e.getMessage());
   }finally{
      //关闭流，释放资源
      try{
         if(os != null){
            os.close();
         }
         if(ois != null){
            ois.close();
         }
         if(is != null){
            is.close();
         }
      }catch(Exception e){
         System.out.println(e.getMessage());
      }
   }
}
```

完整代码：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/521:26
 * @comment
 */
@SuppressWarnings("all")
public class HuffmanTreeCode {
    public static void main(String[] args) {
        //测试压缩文件
        /*
        String srcFile = "E://src.bmp";
        String dstFile = "E://dst.zip";
        zipFile(srcFile,dstFile);
        System.out.println("压缩文件OK");
         */

        //测试解压文件
        String zipFile = "E://dst.zip";
        String dstFile = "E://srcTwo.bmp";
        unZipFile(zipFile,dstFile);
        System.out.println("解压完毕OK");
    }


    /**
     * 完成对压缩文件的解压
     * @param zipFile 准备解压的文件
     * @param dstFile 将文件解压到哪个路径
     */
    private static void unZipFile(String zipFile,String dstFile){
        InputStream is = null;
        ObjectInputStream ois = null;
        OutputStream os = null;
        try{
            is = new FileInputStream(zipFile);
            ois = new ObjectInputStream(is);
            //读取byte数组 huffmanBytes
            byte[] huffmanBytes = (byte[]) ois.readObject();
            //读取赫夫曼编码表
            Map<Byte,String> huffmanCodes = (Map<Byte,String>) ois.readObject();
            //解码
            byte[] bytes = decode(huffmanCodes,huffmanBytes);
            //将bytes,数组写入到目标文件
            os = new FileOutputStream(dstFile);
            //写出数据到文件中
            os.write(bytes);
        }catch(Exception e){
            System.out.println(e.getMessage());
        }finally{
            //关闭流，释放资源
            try{
                if(os != null){
                    os.close();
                }
                if(ois != null){
                    ois.close();
                }
                if(is != null){
                    is.close();
                }
            }catch(Exception e){
                System.out.println(e.getMessage());
            }
        }
    }

    /**
     * 编写方法，将一个文件进行压缩
     * @param srcFile 你传入的希望压缩的文件的全路径
     * @param dstFile 我们压缩后将压缩文件放到哪个目录
     */
    private static void zipFile(String srcFile,String dstFile){
        //创建输出流
        OutputStream os = null;
        ObjectOutputStream oos = null;
        //创建文件的输入流
        FileInputStream is = null;
        try{
            //创建文件的输入流
            is = new FileInputStream(srcFile);
            //创建一个和源文件大小一样的byte[]
            byte[] b = new byte[is.available()];
            //读取文件
            is.read(b);
            //获取到文件对应的赫夫曼编码表
            //直接对原文件进行压缩
            byte[] huffmanBytes = huffmanZip(b);
            //创建文件的输出流,存放压缩文件
            os = new FileOutputStream(dstFile);
            //创建一个和文件输出流关联的ObjectOutputStream
            oos = new ObjectOutputStream(os);
            //把赫夫曼编码后的字节数组写入压缩文件
            oos.writeObject(huffmanBytes);
            //以对象流形式写入赫夫曼编码，为了解压(恢复源文件)时使用
            //注意：一定要把赫夫曼编码，写入压缩文件
            oos.writeObject(huffmanCodes);
        }catch(IOException e){
            System.out.println(e.getMessage());
        }finally{
            //关闭流，释放资源
            try{
                if(is != null){
                    is.close();
                    os.close();
                    oos.close();
                }
            }catch(IOException e){
                System.out.println(e.getMessage());
            }
        }
    }

    /**
     * 编写一个方法，完成对压缩数据的解码
     *
     * @param huffmanCodes 赫夫曼编码表 map
     * @param huffmanBytes 赫夫曼编码得到的字节数组
     * @return 就是原来的字符串对应的数组
     */
    private static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < huffmanBytes.length; i++) {
            byte b = huffmanBytes[i];
            boolean flag = (i == huffmanBytes.length - 1);
            stringBuilder.append(byteToBitString(!flag, b));
        }
//        System.out.println("赫夫曼字节数组对应的二进制字符串："+stringBuilder.toString());
        //把字符串按照指定的赫夫曼编码进行解码
        //把赫夫曼编码表进行调换，因为反向查询 a -> 100 100 -> a
        Map<String, Byte> map = new HashMap<>();
        for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
            map.put(entry.getValue(), entry.getKey());
        }

        //创建集合，存放byte
        List<Byte> list = new ArrayList<>();
        for (int i = 0; i < stringBuilder.length(); ) {
            int count = 1;//小的计数器
            boolean flag = true;
            Byte b = null;
            while (flag) {
                //先取出一个'1','0'
                String key = stringBuilder.substring(i, i + count);//i不动，让count移动 匹配字符
                b = map.get(key);
                if (b == null) {//b == null说明没有匹配到
                    count++;
                } else {//说明 匹配到了
                    flag = false;
                }
            }
            list.add(b);
            i += count;//i 直接移动到count位置
        }
        //当for循环结束后list中就存放了所有的字符 i like like like java do you like a java
        //把list中 的数据放入byte数组并返回
        byte[] b = new byte[list.size()];
        for (int i = 0; i < b.length; i++) {
            b[i] = list.get(i);
        }
        return b;
    }

    /**
     * 将一个byte转成一个二进制的字符串
     *
     * @param flag 标志是否需要补高位如果是true，需要，false，不需要
     * @param b    传入的byte
     * @return b 对应的二进制的字符串(注意是按补码返回)
     */
    private static String byteToBitString(boolean flag, byte b) {
        //使用变量保存b,将b转换成int
        int temp = b;
        //判断是正数则需要补高位
        if (flag) {
            temp |= 256;
            //说明：按位与 256 1 0000 0000 | 0000 0001 => 1 0000 0001
        }
        //返回的是temp对应的二进制的补码
        String str = Integer.toBinaryString(temp);
        if (flag) {
            return str.substring(str.length() - 8);
        } else {
            return str;
        }
    }

    //使用一个方法，将前面的方法封装起来，便于我们的调用

    /**
     * @param bytes 原始的字符串对应的字节数组
     * @return 是经过赫夫曼编码处理后的字节数组（压缩后的数组）
     */
    private static byte[] huffmanZip(byte[] bytes) {
        List<Node01> nodes = getNodes(bytes);
        //根据nodes创建的赫夫曼树
        Node01 huffmanTreeRoot = createHuffmanTree(nodes);
        //对应的赫夫曼编码(根据 赫夫曼树)
        Map<Byte, String> huffmanCodes = getCodes(huffmanTreeRoot);
        //根据生成的赫夫曼编码，压缩得到压缩后的赫夫曼编码字节数组
        byte[] huffmanCodeBytes = zip(bytes, huffmanCodes);
        return huffmanCodeBytes;
    }

    //编写一个方法，将字符串对应的byte[] 数组，通过生成的赫夫曼编码表，返回一个赫夫曼编码 压缩后的byte[]

    /**
     * @param bytes        这时原始的字符串对应的 byte[]
     * @param huffmanCodes 生成的赫夫曼编码map
     * @return 返回赫夫曼编码处理后的 byte[]
     * 举例： String content = "i like like like java do you like a java"; =》 byte[] contentBytes = content.getBytes();
     * 返回的是 字符串 "1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100"
     * => 对应的 byte[] huffmanCodeBytes  ，即 8位对应一个 byte,放入到 huffmanCodeBytes
     * huffmanCodeBytes[0] =  10101000(补码) => byte  [推导  10101000=> 10101000 - 1 => 10100111(反码)=> 11011000= -88 ]
     * huffmanCodeBytes[1] = -88
     */
    private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {

        //1.利用 huffmanCodes 将  bytes 转成  赫夫曼编码对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        //遍历bytes 数组
        for (byte b : bytes) {
            stringBuilder.append(huffmanCodes.get(b));
        }

        //System.out.println("测试 stringBuilder~~~=" + stringBuilder.toString());

        //将 "1010100010111111110..." 转成 byte[]

        //统计返回  byte[] huffmanCodeBytes 长度
        //一句话 int len = (stringBuilder.length() + 7) / 8;
        int len;
        if (stringBuilder.length() % 8 == 0) {
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建 存储压缩后的 byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;//记录是第几个byte
        for (int i = 0; i < stringBuilder.length(); i += 8) { //因为是每8位对应一个byte,所以步长 +8
            String strByte;
            if (i + 8 > stringBuilder.length()) {//不够8位
                strByte = stringBuilder.substring(i);
            } else {
                strByte = stringBuilder.substring(i, i + 8);
            }
            //将strByte 转成一个byte,放入到 huffmanCodeBytes
            huffmanCodeBytes[index] = (byte) Integer.parseInt(strByte, 2);
            index++;
        }
        return huffmanCodeBytes;
    }

    //生成赫夫曼树对应的赫夫曼编码
    //思路：
    //1.将赫夫曼编码表存放在Map<Byte,String> 形式
    //{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011} 等等[形式]
    static Map<Byte, String> huffmanCodes = new HashMap<>();
    //2.在生成赫夫曼编码表时，需要去拼接路径，定义一个StringBuilder，存储某个叶子节点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    private static Map<Byte, String> getCodes(Node01 root) {
        if (root == null) {
            return null;
        }
        //处理root的左子树
        getCodes(root.left, "0", stringBuilder);
        //处理root的右子树
        getCodes(root.right, "1", stringBuilder);
        return huffmanCodes;
    }

    /**
     * 功能：将传入的Node节点的所有叶子节点的赫夫曼编码存放到，并放入到huffmanCodes集合
     *
     * @param node          传入节点
     * @param code          路径：左子节点是0，右子节点是1
     * @param stringBuilder 用于拼接路径
     */
    private static void getCodes(Node01 node, String code, StringBuilder stringBuilder) {
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code，加入到stringBuilder2中
        stringBuilder2.append(code);
        if (node != null) {
            //判断当前node,是叶子节点还是非叶子节点
            if (node.data == null) {
                //非叶子节点
                //递归处理
                //向左递归
                getCodes(node.left, "0", stringBuilder2);
                //向右递归
                getCodes(node.right, "1", stringBuilder2);
            } else {
                //说明是叶子节点
                //表示找到了某个叶子节点的最后
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }

    //前序遍历
    private static void preOrder(Node01 root) {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("赫夫曼树为空");
        }
    }

    /**
     * @param bytes bytes 接受字节数组
     * @return 返回的就是List形式
     */
    private static List<Node01> getNodes(byte[] bytes) {
        //1.创建一个ArrayList
        List<Node01> list = new ArrayList<>();
        //遍历list，统计每一个byte出现的次数 -> map[key,value]
        Map<Byte, Integer> map = new HashMap<>();
        for (byte b : bytes) {
            Integer count = map.get(b);
            //Map还没有这个字符数据，第一次则执行如下
            if (count == null) {
                map.put(b, 1);
            } else {//已经存在了执行如下
                map.put(b, count + 1);
            }
        }

        //把每一个键值对转成一个Node对象，并加入list集合
        //遍历map
        for (Map.Entry<Byte, Integer> entry : map.entrySet()) {
            list.add(new Node01(entry.getKey(), entry.getValue()));
        }

        return list;
    }

    //通过List创建赫夫曼树
    private static Node01 createHuffmanTree(List<Node01> list) {

        while (list.size() > 1) {
            //排序，从小到大
            Collections.sort(list);
            //取出第一棵最小的二叉树
            Node01 leftNode = list.get(0);
            //取出第二棵最小的二叉树
            Node01 rightNode = list.get(1);
            //创建一颗新的二叉树，它的根节点 没有data，只有权值
            Node01 parent = new Node01(null, leftNode.weight + rightNode.weight);
            parent.left = leftNode;
            parent.right = rightNode;
            //将处理过的两颗二叉树从list中移除
            list.remove(leftNode);
            list.remove(rightNode);
            //将新的二叉树加入到list集合中
            list.add(parent);
        }
        return list.get(0);
    }
}

@SuppressWarnings("all")
class Node01 implements Comparable<Node01> {
    Byte data;
    int weight;
    Node01 left;
    Node01 right;

    public Node01(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node01 o) {
        return this.weight - o.weight;
    }

    public void preOrder() {
        System.out.println(this);
        if (this.left != null) {
            this.left.preOrder();
        }
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node[data = " + data + " , weight = " + weight + "]";
    }
}
```

#### 赫夫曼编码压缩文件注意事项

1.  如果文件本身就是经过压缩处理的，那么使用赫夫曼编码再压缩效率不会有明显变化，比如视频，ppt，等等文件
2.  赫夫曼编码是按字节来处理的，因此可以处理所有的文件(二进制，文本文件)
3.  如果一个文件中的内容，重复的数据不多，压缩效果也不会很明显

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

## 多路查找树

 [二叉树与B树](#二叉树与B树)  

 [树](#树) 

 [B 树，B+ 树，B`*` 树](#B树，B+ 树，B`*`树) 

---

​																				  		**钢达姆机器人**  

![钢达姆机器人](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/%E9%92%A2%E8%BE%BE%E5%A7%86%E6%9C%BA%E5%99%A8%E4%BA%BA.png)



### 二叉树与B树

二叉树的问题分析

二叉树的操作效率较高，但是也存在问题，请看下面的二叉树

![image-20230711154532431](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711154532431.png)

层数：5

节点数量：31

-  计算方式：2 ^ 5^层数^ - 1 = 31

1.  ==二叉树需要加载到内存的==，如果二叉树的==节点少==，==没==有==什么问题==，但是如果二叉树的==节点很多== (比如 1 亿)，就==存在==如下==问题==：
2.  **问题1**：在==构建二叉树时==，需要==多次进行 `i/o` 操作== (海量数据存在==数据库==或==文件==中)，节点海量，==构建二叉树时，速度有影响==。
3.  **问题2**：==节点海量==，也会造成二叉树的==高度很大==，会==降低操作速度==。

#### 多叉树

1.  在<font style="color:red">二叉树中</font>，每个节点有<font style="color:red">数据项</font>，<font style="color:red">最多有两个子节点</font>。如果==允许每个节点可以有更多的数据项==和==更多的子节点==，就是==多叉树== (multiway tree)
2.  后面讲解的 2 - 3 树，2 - 3 - 4 树就是多叉树，==多叉树通过重新组织节点，减少树的高度，能对二叉树进行优化==。
3.  举例说明 (下面 2 - 3 树就是一颗多叉树)

![image-20230711155922937](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711155922937.png)

多叉树中，如果一个节点中存在了三个分叉则称为 ==3 节点==。如果是两个分叉则称为 ==2 节点==。

#### B树的基本介绍

B树通过重新组织节点，降低树的高度，并减少 `i/o` 读写次数来提升效率。

![image-20230711161106696](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711161106696.png)

上图说明：

-  一个 **○** 代表一个数项

-  一整个 **○ ○ ○** 的集合代表一个节点

**优点理解**：

-  降低树的高度：

   可以看到，一个节点中有很多数据项，就能大大减少节点数量，从而降低树的高度

-  减少 `I/O` 读写次数

   文件系统及数据库系统的设计者利用了磁盘预读原理，将一个节点的大小设为等于一个页 (页的大小通常为 4K)，这样每个节点只需要一次 `i/o` 就可以完全载入。

   这样说，你可能没有概念，举个例子：将树的度 M 设置为 1024 ，在 600 亿个元素中最多只需要4次 `i/o` 操作就可以读取到想要的元素，B树 (B+) 宽泛应用于文件存储系统以及数据库系统中。

   什么是 **度**？

-  节点的度：

   一个节点下的子树节点个数就是 节点的度。

-  树的度：

   指一颗树中 ，节点的度最大的哪一个值。

### 树

#### 2 - 3 树基本介绍

**2 - 3 树 是最简单的 B树结构，具有如下特点**：

1.  ==2 - 3 树的所有叶子节点都在同一层== (**只要是B树都满足这个条件，就是满树**)

2.  有两个子节点的节点叫二节点，二节点要么没有子节点，要么有两个子节点

    不能只有一个节点，==说得通俗点就是不能比父节点的节点还少== 

3.  有三个子节点的节点叫三节点，三节点要么没有子节点，要么有三个子节点

    不能有二个子节点，一个子节点，==说得通俗点就是不能比父节点的节点还少== 

4.  ==2 - 3 树==是==由二节点==和==三节点构成的树==。

##### 2-3 树构建图解

对数列 {16, 24, 12, 32, 14, 26, 34, 10, 8, 28, 38, 20} 构建成一个 2-3树，那么它构建的规则要满足前面说的特点。下面进行图解后，你就明白，上面的特点是如何限制的。

有几个额外的注意事项：

1.  一个节点中，最多只允许放 2 个数据。
2.  构建的树必须是有序的，也就是按照二叉排序(BST)的要求构建有序的树

下面是图解步骤：

1.  添加 16,24

![image-20230711173129240](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711173129240.png)

添加 16 时，没有数据，直接新建一个节点，放进去。

添加24时，发现有一个节点了，并且比 16 大， 此时该节点中只有一个数据，则将24放在 16 的右边。

2.  添加 12

![image-20230711173239395](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711173239395.png)

此时会发现，12 比 16 小，本来应该放在  16 的左边。此时发现这个节点 **已经有两个数据了**，那么就只能放在 **左子节点**。

如果直接将 12 放到  `16 , 24` 的左节点，就会破坏 2-3树的条件：2  节点 ，要么没有子节点，要么有两个节点。

那么此时就只能将 `16,24`  这个节点进行拆分。如上图： 24  变成 16 的右节点，12 变成  16  的左节点。

这时就满足了 2-3树 的特性。

3.  添加 32

这个就简单了，以现在的树结构，可以直接添加到 24 的 右边 ，变成 `24,32` 

![image-20230711173649665](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711173649665.png)

4.  添加 14

这个也简单，直接添加到 12 的右边，变成 `12,14` 

![image-20230711173735064](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711173735064.png)

5.  添加 26

此时应该添加到 `24,32` 的中间，由于一个节点只能添加两个数据，那么就需要拆分。

![image-20230711173823070](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711173823070.png)

为了满足B树特点，发现上层的 `16` 只有一个数，那么就补足它。组成 `16,26`。

因为此时 `24,32` **这个节点，不满足 BST 的排序了**，24是小于 26的。只能 32 满足。

**拆完上层，再拆本层**：由于 24 介于 `16,24` 之间，**则将它安排在 3 节点中的中间节点**， `24,32` 把 24 拆分出去了，只剩下 `32` ，此时完全满足B树的特点

6.  添加 34

此时就简单了，添加到 `32,34` 中。

![image-20230711174145067](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711174145067.png)

7.  添加 10

此时应该添加到 `12,14` 的左侧，但是不满足条件：一个节点最多只能装 2 个数据。

放到 `12,14` 的左节点，也不满足条件：所有叶子节点必须在同一层，也不满足 2-3树 节点的 数量要求。

那么此时就需要拆分，先看它们上层 `16,26` 是满 的，如何做呢？看下图：

![image-20230711174344071](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711174344071.png)

左侧的拆分图，上面我们分析过了，不满足B树要求，那么就需要拆分成右图这样：

-  

   1.  将 `12,14` 中的14 拆分成 右子节点 ，10 挂在 左节点。

   2.  此时不满足 B树要求的，则将 `16,26` 中的 26 拆分成 右子节点。

   3.  `24` 这个节点由于上层被拆分了，不满足在中间节点了。调整它的位置

   4.  原来的 `32,34` 节点调整为 `16` 的有节点。

8.  添加8

    此时很简单，组成 `8,10` 即可

    ![image-20230711174809201](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711174809201.png)

9.  添加 28

![image-20230711174827823](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711174827823.png)

这里有个小问题，此时 28 不是应该加在 `26,28` 吗？难道说还有一个规则？

-  **只有一个数据的节点**，下面只允许 **最多右 2 个节点，要么没有** 
-  **有 2 个数据的节点**，下面只允许 **最多 3 个节点，要么没有** 

10.  添加 38

此时就简单，直接组成 `34,38` 

![image-20230711175119632](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711175119632.png)

11.  添加 20

这个也简单，直接组成 `20,24` 

![image-20230711175153140](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711175153140.png)

##### 2-3树添加规则总结

满足如下特点：

1.  所有  ==叶子节点==  都在同一层

    只要是 B 树都满足这个条件，就是==满树==。

2.  有两个子节点的节点 叫  ==二节点==

    二节点要么  ==没有子节点== ，要么  ==必须有两个子节点==。

3.  有三个子节点的节点叫  ==三节点==

    三节点要么  ==没有子节点==，要么  ==必须有三个子节点==。

4.  2-3树 是由 ==二节点==  和  ==三节点== 构成的树

5.  构建的树，要满足二叉排序树（BST）的顺序。

6.  ==一个节点中，最多只允许放 2  个数据==。

7.  ==只有一个数据的节点==，下面只允许 ==最多有 2 个节点，要么没有== 

8.  ==有 2 个数据的节点==，下面只允许 ==最多有 3 个节点，要么没有== 

其它说明

除了23树，还有234树等，概念和23树类似，也是一种B树。如图：

![image-20230711190603750](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711190603750.png)

### B树，B+ 树，B`*`树

#### B树的介绍

**B-tree** 树即 ==B树==，**B**即==Balanced==，平衡的意思。有人把B-tree翻译成B-树，容易让人产生==误解==。会以为B-树是一种树，而B树又是另一种树。实际上，==B-tree就是指的B树==。

前面已经介绍了 2-3树 和 2-3-4树，它们就是B树(英语: B-tree 也写成 B-树)，这里我们再做一个说明，我们在学习MySQL时，经常听到说某种类型的索引是基于B树或者B+树的，如图：

![image-20230711191024778](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711191024778.png)

**B树的说明**:

1.  B树的阶：节点的最多子节点个数。比如2-3树的阶是3，2-3-4树的阶是4
2.  B-树的搜索，从根结点开始，对结点内的关键字（有序）序列进行二分查找，如果命中则结束，否则进入查询关键字所属范围的儿子结点；重复，直到所对应的儿子指针为空，或已经是叶子结点
3.  关键字集合分布在整颗树中, 即叶子节点和非叶子节点都存放数据.
4.  搜索有可能在非叶子结点结束
5.  其搜索性能等价于在关键字全集内做一次二分查找

#### B+树的介绍

B+树是B树的变体，也是一种多路搜索树。

![image-20230711194503904](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711194503904.png)

**B+树的说明**:

1.  B+树的搜索与B树也基本相同，区别是B+树只有达到叶子结点才命中（B树可以在非叶子结点命中），其性能也等价于在关键字全集做一次二分查找
2.  所有**关键字都出现在叶子结点的链表中**（即数据只能在叶子节点【也叫稠密索引】），且链表中的关键字(数据)恰好是有序的。
3.  不可能在非叶子结点命中
4.  非叶子结点相当于是叶子结点的索引（稀疏索引），叶子结点相当于是存储（关键字）数据的数据层
5.  更适合文件索引系统
6.  B树和B+树各有自己的应用场景，不能说B+树**完全比**B树好，反之亦然.

#### B树的介绍

B*树是B+树的变体，在B+树的非根和非叶子结点再增加指向兄弟的指针。

![image-20230711195028517](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711195028517.png)

**B树的说明**:

1.  B*树定义了非叶子结点关键字个数至少为(2/3)*M，即块的最低使用率为2/3，而B+树的块的最低使用率为B+树的1/2。
2.  从第1个特点我们可以看出，B*树分配新结点的概率比B+树要低，空间使用率更高

## 图

**为什么要有图** 

1.  前面我们学了线性表 和 树
2.  线性表局限于一个直接前驱和一个直接后继的关系
3.  树也只能有一个直接前驱也就是父节点
4.  当我们需要表示多对多的关系时，这里我们就用到了图

### 图的举例说明

图是一种==数据结构==，其中节点可以具有==零==个或==多==个==相邻元素==。两个节点之间的连接称为 ==边==。节点也可以称为==顶点==。如图：

![image-20230711200325089](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711200325089.png)



### 图常用的概念

1.  顶点 (vertex)
2.  边 (edge)
3.  路径
4.  无向图 (下图)

![image-20230711200430332](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711200430332.png)

-  **无向图**：顶点之间的连接==没有方向==，比如 A - B，既可以是 A - B 也可以 B - A

-  **路径**：比如从 D - C 的路径有
1.  D - B - C
   
2.  D - A - B - C

5.  有向图 (下图)

    ![image-20230711200958618](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711200958618.png)

    -  **有向图**：顶点之间的连接==有方向==，比如 A - B ， 只能是 A -> B 不能是 B -> A。

6.  带权图

![image-20230711201041513](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711201041513.png)

-  **带权图**：这种边带权值的图也叫==网==。

### 图的表示方式

**图的表示方式有两种**：

-  **==二维数组==表示**(邻接矩阵)；

-  **==链表==表示**(邻接表)；

#### 邻接矩阵

邻接矩阵是表示图形中顶点之间相邻关系的矩阵，对于n个顶点的图而言，矩阵是row和col表示的是1...n个点。

![image-20230711202214098](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711202214098.png)

#### 邻接表

1.  邻接矩阵需要为每个顶点都分配n个边的空间，其实有很多边都是不存在的，会造成空间的一定损失。
2.  邻接表的实现只关心存在的边，不关心不存在的边。因此没有空间浪费，邻接表由数组+链表组成

![image-20230711202425883](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711202425883.png)

**说明**：

1.  标号为 0 的节点的相关联的节点为 1  2  3  4
2.  标号为 1 的节点的相关联节点为 0  4
3.  标号为  2  的节点的相关联节点为 0  4  5
4.  ....

#### 图的快速入门案例

1 **要求**：代码实现如下图结构

![image-20230711211516098](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711211516098.png)



2 **思路分析** 

1.  每一个==顶点==需要用一个==容器来装==，这里使用简单的 ==String 类型来表示== A,B ... 等==节点== 
2.  这些所有的==顶点==，我们用一个==List来存储== 
3.  它对应的==矩阵使用==一个==二维数组==来==表示==，==节点之间的关系== 

3 **代码实现** 

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1120:41
 * @comment
 */
public class Graph {
    private ArrayList<String> vertexList;//存储顶点集合
    private int[][] edges;//存储图对应的邻接矩阵
    private int numOfEdges;//表示边的数目

    public Graph(int n) {
        edges = new int[n][n];
        vertexList = new ArrayList<>(n);
        numOfEdges = 0;
    }

    //图中常用的方法
    //返回节点个数
    public int getNumOfVertex() {
        return vertexList.size();
    }

    //得到边的数目
    public int getNumEdges(int i) {
        return numOfEdges;
    }

    //返回节点i对应下标的值 (0 -> "A") (1 -> "B") (2 -> "C")
    public String getVertexByIndex(int i){
        return vertexList.get(i);
    }

    //返回v1和v2的权值
    public int getWeight(int v1,int v2){
        return edges[v1][v2];
    }

    //显示图对应的矩阵
    public void showGraph(){
        for(int[] temp:edges)
            System.err.println(Arrays.toString(temp));
    }

    /**
     * 插入节点
     *
     * @param vertex
     */
    public void insertVertex(String vertex) {
        vertexList.add(vertex);
    }

    /**
     * 添加边
     *
     * @param v1     表示点的下标,即是第几个顶点
     * @param v2     第二个顶点对应的下标
     * @param weight 表示关联
     */
    public void insertEdge(int v1, int v2, int weight) {
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }

    public static void main(String[] args) {
        int n = 5;
        String[] vertexValue = {"A","B","C","D","E"};
        //创建图对象
        Graph graph = new Graph(n);
        //循环的添加顶点
        for(String value:vertexValue)
            graph.insertVertex(value);
        //添加边
        graph.insertEdge(0,1,1);
        graph.insertEdge(0,2,1);
        graph.insertEdge(1,2,1);
        graph.insertEdge(1,3,1);
        graph.insertEdge(1,4,1);
        //输出邻接矩阵
        graph.showGraph();
    }
}
```

结果：

```
[0, 1, 1, 0, 0]
[1, 0, 1, 1, 1]
[1, 1, 0, 0, 0]
[0, 1, 0, 0, 0]
[0, 1, 0, 0, 0]
```

### 图的深度优先遍历介绍

#### 图遍历介绍

所谓图的遍历，即是对节点的访问。一个图有那么多个节点，如何遍历这些节点，需要特定策略，一般有两种访问策略：

1.  深度优先遍历
2.  广度优先遍历

#### 深度优先遍历基本思想

**图的深度优先搜索** (Depth First Search)

1.  深度优先遍历，从初始访问节点出发，初始访问节点可能有多个邻接节点，深度优先遍历的策略就是首先访问第一个邻接节点，然后再以这个被访问的邻接节点作为初始节点，访问它的第一个邻接节点，可以这样理解：每次都在访问完==当前节点==后首先访问==当前节点的第一个邻接节点==。
2.  我们可以看到，这样的访问策略是优先往==纵向挖掘深入==，而不是对一个节点的所有邻接节点进行横向访问。
3.  显然，深度优先搜索是一个==递归的过程==。

##### 深度优先遍历算法步骤

1.  访问初始节点v，并标记节点v为已访问。
2.  查找节点v的第一个邻接节点w。
3.  若w存在，则继续执行第4步，如果w不存在，则回到第1步，将从v的下一个节点继续。
4.  若w未被访问，对w进行深度优先遍历递归 (即把w当做另一个v，然后进行步骤123)。
5.  查找节点v 的w邻接节点的下一个邻接节点，转到步骤3。

**看一个具体案例分析**：

**要求**：对下图进行深度优先搜索，从A开始遍历

![image-20230711220409405](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711220409405.png)

![image-20230712165116264](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230712165116264.png)

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1120:41
 * @comment
 */
public class Graph {
    private ArrayList<String> vertexList;//存储顶点集合
    private int[][] edges;//存储图对应的邻接矩阵
    private int numOfEdges;//表示边的数目
    //记录某个节点是否被访问
    private boolean[] isVisited;

    public Graph(int n) {
        edges = new int[n][n];
        vertexList = new ArrayList<>(n);
        numOfEdges = 0;
        isVisited = new boolean[n];
    }

    /**
     * 得到第一个邻接节点的下标 w
     *
     * @param index
     * @return 如果存在就返回对应的下标，否则返回-1
     */
    public int getFirstNeighbor(int index) {
        for (int j = 0; j < vertexList.size(); j++)
            if (edges[index][j] > 0)
                return j;
        return -1;
    }

    /**
     * 根据前一个邻接节点的下标来获取下一个邻接节点
     *
     * @param v1
     * @param v2
     * @return
     */
    public int getNextNeighbor(int v1, int v2) {
        for (int j = v2 + 1; j < vertexList.size(); j++)
            if (edges[v1][j] > 0)
                return j;
        return -1;
    }

    /**
     * 深度优先遍历
     *
     * @param isVisited
     * @param i         第一次就是0
     */
    private void dfs(boolean[] isVisited, int i) {
        //首先我们访问该节点，输出
        System.out.println(getValueByIndex(i));
        //将节点设置为已经访问
        isVisited[i] = true;
        //查找节点i的第一个邻接节点w
        int w = getFirstNeighbor(i);
        while (w != -1) {
            if (!isVisited[w])
                dfs(isVisited, w);
            //如果w节点已经被访问过
            w = getNextNeighbor(i, w);
        }
    }

    //对dfs进行一个重载，遍历我们所有的节点，并进行dfs
    public void dfs(){
        //遍历所有的节点，进行dfs[回溯]
        for(int i = 0;i < getNumOfVertex();i ++)
            if(!isVisited[i])
                dfs(isVisited,i);
    }

    //图中常用的方法
    //返回节点个数
    public int getNumOfVertex() {
        return vertexList.size();
    }

    //得到边的数目
    public int getNumEdges(int i) {
        return numOfEdges;
    }

    //返回节点i对应下标的值 (0 -> "A") (1 -> "B") (2 -> "C")
    public String getValueByIndex(int i) {
        return vertexList.get(i);
    }

    //返回v1和v2的权值
    public int getWeight(int v1, int v2) {
        return edges[v1][v2];
    }

    //显示图对应的矩阵
    public void showGraph() {
        for (int[] temp : edges)
            System.err.println(Arrays.toString(temp));
    }

    /**
     * 插入节点
     *
     * @param vertex
     */
    public void insertVertex(String vertex) {
        vertexList.add(vertex);
    }

    /**
     * 添加边
     *
     * @param v1     表示点的下标,即是第几个顶点
     * @param v2     第二个顶点对应的下标
     * @param weight 表示关联
     */
    public void insertEdge(int v1, int v2, int weight) {
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }

    public static void main(String[] args) {
        int n = 5;
        String[] vertexValue = {"A", "B", "C", "D", "E"};
        //创建图对象
        Graph graph = new Graph(n);
        //循环的添加顶点
        for (String value : vertexValue)
            graph.insertVertex(value);
        //添加边
        graph.insertEdge(0, 1, 1);
        graph.insertEdge(0, 2, 1);
        graph.insertEdge(1, 2, 1);
        graph.insertEdge(1, 3, 1);
        graph.insertEdge(1, 4, 1);
        //输出邻接矩阵
        graph.showGraph();

        System.out.println("-----------------深度遍历-----------------");
        graph.dfs();
    }
}
```

结果：

```
[0, 1, 1, 0, 0]
[1, 0, 1, 1, 1]
[1, 1, 0, 0, 0]
[0, 1, 0, 0, 0]
[0, 1, 0, 0, 0]
-----------------深度遍历-----------------
A
B
C
D
E
```

### 图的广度优先遍历

##### 广度优先遍基本思想

图的广度优先搜索 (Broad First Search)。

类似于一个分层搜索的过程，广度优先遍历需要使用一个队列以保持访问过的节点的顺序，以便按这个顺序来访问这些节点的邻接节点。

##### 广度优先遍历算法步骤

1.  访问初始节点v并标记节点v为已访问。
2.  节点v入队列
3.  当队列非空时，继续执行，否则算法结束。
4.  出队列，取得对头节点u。
5.  查找节点u的第一个邻接节点w。
6.  若节点u的邻接节点w不存在，则转到步骤3；否则循环执行以下三个步骤
    1.  若节点w尚未被访问，则访问节点w并标记为已访问。
    2.  节点w入队列
    3.  查找节点u的继w邻接节点后的下一个邻接节点w，转到步骤6。

##### 广度优先遍历举例说明

![image-20230711221219958](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230711221219958.png)

![image-20230712171034456](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230712171034456.png)

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1120:41
 * @comment
 */
public class Graph {
    private ArrayList<String> vertexList;//存储顶点集合
    private int[][] edges;//存储图对应的邻接矩阵
    private int numOfEdges;//表示边的数目
    //定义给数组boolean[],记录某个节点是否被访问
    private boolean[] isVisited;

    public Graph(int n) {
        edges = new int[n][n];
        vertexList = new ArrayList<>(n);
        numOfEdges = 0;
        isVisited = new boolean[n];
    }

    /**
     * 得到第一个邻接节点的下标 w
     *
     * @param index
     * @return 如果存在就返回对应的下标，否则返回-1
     */
    public int getFirstNeighbor(int index) {
        for (int j = 0; j < vertexList.size(); j++)
            if (edges[index][j] > 0)
                return j;
        return -1;
    }

    /**
     * 根据前一个邻接节点的下标来获取下一个邻接节点
     *
     * @param v1
     * @param v2
     * @return
     */
    public int getNextNeighbor(int v1, int v2) {
        //+1 略过自己(不算自己)
        for (int j = v2 + 1; j < vertexList.size(); j++)
            if (edges[v1][j] > 0)
                return j;
        return -1;
    }

    //对一个节点进行广度优先遍历
    private void bfs(boolean[] isVisited, int i) {
        //表示队列头节点对应的下标
        int u;
        //邻接节点w
        int w;
        //队列，记录节点访问的顺序
        LinkedList queue = new LinkedList();
        //访问节点，输出节点的信息
        System.out.println(getValueByIndex(i));
        //标记为已访问
        isVisited[i] = true;
        //将节点加入到队列
        queue.addLast(i);
        while (!queue.isEmpty()) {
            //取出队列头节点下标
            u = (int) queue.removeFirst();
            //得到第一个邻接节点的下标w
            w = getFirstNeighbor(u);
            while (w != -1) {
                if (!isVisited[w]) {
                    System.out.println(getValueByIndex(w));
                    //标记已访问
                    isVisited[w] = true;
                    //加入队列
                    queue.addLast(w);
                }
                //以u为前驱节点找w后面的下一个邻接节点
                w = getNextNeighbor(u, w);
            }
        }
    }

    //遍历所有节点，都进行广度优先搜索
    public void bfs() {
        for (int i = 0; i < getNumOfVertex(); i++)
            if (!isVisited[i])
                bfs(isVisited, i);
    }

    //图中常用的方法
    //返回节点个数
    public int getNumOfVertex() {
        return vertexList.size();
    }

    //得到边的数目
    public int getNumEdges(int i) {
        return numOfEdges;
    }

    //返回节点i对应下标的值 (0 -> "A") (1 -> "B") (2 -> "C")
    public String getValueByIndex(int i) {
        return vertexList.get(i);
    }

    //返回v1和v2的权值
    public int getWeight(int v1, int v2) {
        return edges[v1][v2];
    }

    //显示图对应的矩阵
    public void showGraph() {
        for (int[] temp : edges)
            System.err.println(Arrays.toString(temp));
    }

    /**
     * 插入节点
     *
     * @param vertex
     */
    public void insertVertex(String vertex) {
        vertexList.add(vertex);
    }

    /**
     * 添加边
     *
     * @param v1     表示点的下标,即是第几个顶点
     * @param v2     第二个顶点对应的下标
     * @param weight 表示关联
     */
    public void insertEdge(int v1, int v2, int weight) {
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }

    public static void main(String[] args) {
        int n = 5;
        String[] vertexValue = {"A", "B", "C", "D", "E"};
        //创建图对象
        Graph graph = new Graph(n);
        //循环的添加顶点
        for (String value : vertexValue)
            graph.insertVertex(value);
        //添加边
        graph.insertEdge(0, 1, 1);
        graph.insertEdge(0, 2, 1);
        graph.insertEdge(1, 2, 1);
        graph.insertEdge(1, 3, 1);
        graph.insertEdge(1, 4, 1);
        //输出邻接矩阵
        graph.showGraph();

        System.out.println("-----------------广度遍历-----------------");
        graph.bfs();
    }
}
```

结果：

```
[0, 1, 1, 0, 0]
[1, 0, 1, 1, 1]
[1, 1, 0, 0, 0]
[0, 1, 0, 0, 0]
[0, 1, 0, 0, 0]
-----------------广度遍历-----------------
A
B
C
D
E
```

完整代码

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1120:41
 * @comment
 */
@SuppressWarnings("all")
public class Graph {
    private ArrayList<String> vertexList;//存储顶点集合
    private int[][] edges;//存储图对应的邻接矩阵
    private int numOfEdges;//表示边的数目
    //定义给数组boolean[],记录某个节点是否被访问
    private boolean[] isVisited;

    public Graph(int n) {
        edges = new int[n][n];
        vertexList = new ArrayList<>(n);
        numOfEdges = 0;
    }

    /**
     * 得到第一个邻接节点的下标 w
     *
     * @param index
     * @return 如果存在就返回对应的下标，否则返回-1
     */
    public int getFirstNeighbor(int index) {
        for (int j = 0; j < vertexList.size(); j++)
            if (edges[index][j] > 0)
                return j;
        return -1;
    }

    /**
     * 根据前一个邻接节点的下标来获取下一个邻接节点
     *
     * @param v1
     * @param v2
     * @return
     */
    public int getNextNeighbor(int v1, int v2) {
        //+1 略过自己(不算自己)
        for (int j = v2 + 1; j < vertexList.size(); j++)
            if (edges[v1][j] > 0)
                return j;
        return -1;
    }

    /**
     * 深度优先遍历
     *
     * @param isVisited
     * @param i         第一次就是0
     */
    private void dfs(boolean[] isVisited, int i) {
        //首先我们访问该节点，输出
        System.out.println(getValueByIndex(i));
        //将节点设置为已经访问
        isVisited[i] = true;
        //查找节点i的第一个邻接节点w
        int w = getFirstNeighbor(i);
        while (w != -1) {
            if (!isVisited[w])
                dfs(isVisited, w);
            //如果w节点已经被访问过
            w = getNextNeighbor(i, w);
        }
    }

    //对dfs进行一个重载，遍历我们所有的节点，并进行dfs
    public void dfs() {
        isVisited = new boolean[vertexList.size()];
        //遍历所有的节点，进行dfs[回溯]
        for (int i = 0; i < getNumOfVertex(); i++)
            if (!isVisited[i])
                dfs(isVisited, i);
    }

    //对一个节点进行广度优先遍历
    private void bfs(boolean[] isVisited, int i) {
        //表示队列头节点对应的下标
        int u;
        //邻接节点w
        int w;
        //队列，记录节点访问的顺序
        LinkedList queue = new LinkedList();
        //访问节点，输出节点的信息
        System.out.println(getValueByIndex(i));
        //标记为已访问
        isVisited[i] = true;
        //将节点加入到队列
        queue.addLast(i);
        while (!queue.isEmpty()) {
            //取出队列头节点下标
            u = (int) queue.removeFirst();
            //得到第一个邻接节点的下标w
            w = getFirstNeighbor(u);
            while (w != -1) {
                if (!isVisited[w]) {
                    System.out.println(getValueByIndex(w));
                    //标记已访问
                    isVisited[w] = true;
                    //加入队列
                    queue.addLast(w);
                }
                //以u为前驱节点找w后面的下一个邻接节点
                w = getNextNeighbor(u, w);
            }
        }
    }

    //遍历所有节点，都进行广度优先搜索
    public void bfs() {
        isVisited = new boolean[vertexList.size()];
        for (int i = 0; i < getNumOfVertex(); i++)
            if (!isVisited[i])
                bfs(isVisited, i);
    }

    //图中常用的方法
    //返回节点个数
    public int getNumOfVertex() {
        return vertexList.size();
    }

    //得到边的数目
    public int getNumEdges(int i) {
        return numOfEdges;
    }

    //返回节点i对应下标的值 (0 -> "A") (1 -> "B") (2 -> "C")
    public String getValueByIndex(int i) {
        return vertexList.get(i);
    }

    //返回v1和v2的权值
    public int getWeight(int v1, int v2) {
        return edges[v1][v2];
    }

    //显示图对应的矩阵
    public void showGraph() {
        for (int[] temp : edges)
            System.err.println(Arrays.toString(temp));
    }

    /**
     * 插入节点
     *
     * @param vertex
     */
    public void insertVertex(String vertex) {
        vertexList.add(vertex);
    }

    /**
     * 添加边
     *
     * @param v1     表示点的下标,即是第几个顶点
     * @param v2     第二个顶点对应的下标
     * @param weight 表示关联
     */
    public void insertEdge(int v1, int v2, int weight) {
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }

    public static void main(String[] args) {
        int n = 5;

        String[] vertexValue = {"A", "B", "C", "D", "E"};
        //创建图对象
        Graph graph = new Graph(n);
        //循环的添加顶点
        for (String value : vertexValue)
            graph.insertVertex(value);
        //添加边
        graph.insertEdge(0, 1, 1);
        graph.insertEdge(0, 2, 1);
        graph.insertEdge(1, 2, 1);
        graph.insertEdge(1, 3, 1);
        graph.insertEdge(1, 4, 1);

        //输出邻接矩阵
        graph.showGraph();

        System.out.println("-----------------深度遍历-----------------");
        graph.dfs();

        System.out.println("-----------------广度遍历-----------------");
        graph.bfs();
    }
}
```

结果

```
[0, 1, 1, 0, 0, 0, 0, 0]
[1, 0, 1, 1, 1, 0, 0, 0]
[1, 1, 0, 0, 0, 0, 0, 0]
[0, 1, 0, 0, 0, 0, 0, 0]
[0, 1, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 0]
-----------------深度遍历-----------------
1
2
3
4
5
6
7
8
-----------------广度遍历-----------------
1
2
3
4
5
6
7
8
```

### 图的==深度优先==<strong style="color:red"> VS </strong>==广度优先== 

由于前面讲解的顶点较少，恰好输出顺序一致，现在来对比下多一点的顶点，如下图：

![image-20230712174413248](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230712174413248.png)

```java
graph.insertEdge(0, 1, 1);
graph.insertEdge(0, 2, 1);
graph.insertEdge(1, 3, 1);
graph.insertEdge(1, 4, 1);
graph.insertEdge(3, 7, 1);
graph.insertEdge(4, 7, 1);
graph.insertEdge(2, 5, 1);
graph.insertEdge(2, 6, 1);
graph.insertEdge(5, 6, 1);
```

1.  ==深度优先遍历==顺序为 `1->2->4->8->5->3->6->7` 
2.  ==广度优先遍历==顺序为：`1->2->3->4->5->6->7->8` 

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/1120:41
 * @comment
 */
@SuppressWarnings("all")
public class Graph {
    private ArrayList<String> vertexList;//存储顶点集合
    private int[][] edges;//存储图对应的邻接矩阵
    private int numOfEdges;//表示边的数目
    //定义给数组boolean[],记录某个节点是否被访问
    private boolean[] isVisited;

    public Graph(int n) {
        edges = new int[n][n];
        vertexList = new ArrayList<>(n);
        numOfEdges = 0;
    }

    /**
     * 得到第一个邻接节点的下标 w
     *
     * @param index
     * @return 如果存在就返回对应的下标，否则返回-1
     */
    public int getFirstNeighbor(int index) {
        for (int j = 0; j < vertexList.size(); j++)
            if (edges[index][j] > 0)
                return j;
        return -1;
    }

    /**
     * 根据前一个邻接节点的下标来获取下一个邻接节点
     *
     * @param v1
     * @param v2
     * @return
     */
    public int getNextNeighbor(int v1, int v2) {
        //+1 略过自己(不算自己)
        for (int j = v2 + 1; j < vertexList.size(); j++)
            if (edges[v1][j] > 0)
                return j;
        return -1;
    }

    /**
     * 深度优先遍历
     *
     * @param isVisited
     * @param i         第一次就是0
     */
    private void dfs(boolean[] isVisited, int i) {
        //首先我们访问该节点，输出
        System.out.println(getValueByIndex(i));
        //将节点设置为已经访问
        isVisited[i] = true;
        //查找节点i的第一个邻接节点w
        int w = getFirstNeighbor(i);
        while (w != -1) {
            if (!isVisited[w])
                dfs(isVisited, w);
            //如果w节点已经被访问过
            w = getNextNeighbor(i, w);
        }
    }

    //对dfs进行一个重载，遍历我们所有的节点，并进行dfs
    public void dfs() {
        isVisited = new boolean[vertexList.size()];
        //遍历所有的节点，进行dfs[回溯]
        for (int i = 0; i < getNumOfVertex(); i++)
            if (!isVisited[i])
                dfs(isVisited, i);
    }

    //对一个节点进行广度优先遍历
    private void bfs(boolean[] isVisited, int i) {
        //表示队列头节点对应的下标
        int u;
        //邻接节点w
        int w;
        //队列，记录节点访问的顺序
        LinkedList queue = new LinkedList();
        //访问节点，输出节点的信息
        System.out.println(getValueByIndex(i));
        //标记为已访问
        isVisited[i] = true;
        //将节点加入到队列
        queue.addLast(i);
        while (!queue.isEmpty()) {
            //取出队列头节点下标
            u = (int) queue.removeFirst();
            //得到第一个邻接节点的下标w
            w = getFirstNeighbor(u);
            while (w != -1) {
                if (!isVisited[w]) {
                    System.out.println(getValueByIndex(w));
                    //标记已访问
                    isVisited[w] = true;
                    //加入队列
                    queue.addLast(w);
                }
                //以u为前驱节点找w后面的下一个邻接节点
                w = getNextNeighbor(u, w);
            }
        }
    }

    //遍历所有节点，都进行广度优先搜索
    public void bfs() {
        isVisited = new boolean[vertexList.size()];
        for (int i = 0; i < getNumOfVertex(); i++)
            if (!isVisited[i])
                bfs(isVisited, i);
    }

    //图中常用的方法
    //返回节点个数
    public int getNumOfVertex() {
        return vertexList.size();
    }

    //得到边的数目
    public int getNumEdges(int i) {
        return numOfEdges;
    }

    //返回节点i对应下标的值 (0 -> "A") (1 -> "B") (2 -> "C")
    public String getValueByIndex(int i) {
        return vertexList.get(i);
    }

    //返回v1和v2的权值
    public int getWeight(int v1, int v2) {
        return edges[v1][v2];
    }

    //显示图对应的矩阵
    public void showGraph() {
        for (int[] temp : edges)
            System.err.println(Arrays.toString(temp));
    }

    /**
     * 插入节点
     *
     * @param vertex
     */
    public void insertVertex(String vertex) {
        vertexList.add(vertex);
    }

    /**
     * 添加边
     *
     * @param v1     表示点的下标,即是第几个顶点
     * @param v2     第二个顶点对应的下标
     * @param weight 表示关联
     */
    public void insertEdge(int v1, int v2, int weight) {
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }

    public static void main(String[] args) {
        int n = 8;

        String[] vertexValue = {"1", "2", "3", "4", "5","6","7","8"};

        //创建图对象
        Graph graph = new Graph(n);
        //循环的添加顶点
        for (String value : vertexValue)
            graph.insertVertex(value);
        //添加边
        graph.insertEdge(0,1,1);
        graph.insertEdge(0,2,1);
        graph.insertEdge(1,3,1);
        graph.insertEdge(1,4,1);
        graph.insertEdge(3,7,1);
        graph.insertEdge(4,7,1);
        graph.insertEdge(2,5,1);
        graph.insertEdge(2,6,1);
        graph.insertEdge(5,6,1);
        //输出邻接矩阵
        graph.showGraph();

        System.out.println("-----------------深度遍历-----------------");
        graph.dfs();

        System.out.println("-----------------广度遍历-----------------");
        graph.bfs();
    }
}
```

**结果**：

-  深度优先遍历

```
[0, 1, 1, 0, 0, 0, 0, 0]
[1, 0, 0, 1, 1, 0, 0, 0]
[1, 0, 0, 0, 0, 1, 1, 0]
[0, 1, 0, 0, 0, 0, 0, 1]
[0, 1, 0, 0, 0, 0, 0, 1]
[0, 0, 1, 0, 0, 0, 1, 0]
[0, 0, 1, 0, 0, 1, 0, 0]
[0, 0, 0, 1, 1, 0, 0, 0]
-----------------深度遍历-----------------
1
2
4
8
5
3
6
7
```

-  广度优先遍历

```
[0, 1, 1, 0, 0, 0, 0, 0]
[1, 0, 0, 1, 1, 0, 0, 0]
[1, 0, 0, 0, 0, 1, 1, 0]
[0, 1, 0, 0, 0, 0, 0, 1]
[0, 1, 0, 0, 0, 0, 0, 1]
[0, 0, 1, 0, 0, 0, 1, 0]
[0, 0, 1, 0, 0, 1, 0, 0]
[0, 0, 0, 1, 1, 0, 0, 0]
-----------------广度遍历-----------------
1
2
3
4
5
6
7
8
```

## 程序员常用的10中算法

### 二分查找(非递归)

#### 二分查找(非递归)介绍

前面我们讲过了二分查找算法，是使用递归的方式(非递归也写了因为会所以提前写了)。下面我们讲解二分查找算法的非递归方式。

二分查找法只适用于从有序的数列中进行查找(比如数组和字母等)，将数列排序后再进行查找。

二分查找法的运行时间为对数数据O(log~2~N)，即查找到需要的目标位置最多只需要log~2~N步，假设从[0,99]的队列(100个数，即n=100)中寻到目标数30，则需要查找步数为log~2~100，即最多需要查找7次(2^6 < 100 < 2 ^ 7)

#### 代码实现

数组{1,3, 8, 10, 11, 67, 100}，编程实现二分查找，要求使用非递归的方式完成。

思路分析：

判断中间值mid来进行折半查找操作

![image-20230714153923559](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307141539864.png)

代码实现：

```java
public class BinarySearchNoRecur {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5};
        int i = searchErFen(arr,3);
        System.out.println(i);
    }

    /**
     * 二分查找的非递归实现
     * @param arr 目标数组,数组有序且升序排序
     * @param value 要查找的目标值
     * @return 返回目标值的下表
     */
    public static int searchErFen(int[] arr,int value){
        //起始指针
        int left = 0;
        //末尾指针
        int last = arr.length;
        //循环
        while(left <= last){
            //每次执行折半处理 计算出 数列的中间位置进行查找目标值
            int mid = (left + last) / 2;
            //判断中间的值是否小于目标值
            if(arr[mid] < value)
                //如果中间值小于目标值则将指针向前移动
                last = mid - 1;
            //判断中间值是否大于目标值
            else if(arr[mid] > value)
                //如果中间值大于了目标值则将指针向后移动
                left = mid + 1;
            else
                //两者都没有执行则说明找到了目标值直接返回
                return mid;
        }
        //如果没有找到则返回-1
        return -1;
    }
}
```

### 分治算法

#### 分治算法介绍

分治法是一种很重要的算法。字面上的解释是"分而治之"，就是把一个复杂的问题分成两个或更多的相同或相似的子问题，再把子问题分成更小的子问题 ... 直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并。这个技巧是很多高效算法的基础，如排序算法(<font style="color:blue">快速排序</font>，<font style="color:blue">归并排序</font>)，傅里叶变换(快速傅里叶变化)...

#### 分治算法的基本步骤

**分治法在每一层递归上都有三个步骤**：

1. ==分解==：将原问题分解为若干个规模较小，相互独立。与原问题形式相同的子问题。
2. ==解决==：若子问题规模较小而容易被解决则直接解，否则递归地解各个子问题。
3. ==合并==：将各个子问题的解合并为原问题的解。

**分治(Divide-and-Conquer(P)) 算法设计模式如下**：

![image-20230714155912742](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307141559273.png)

其中 |P| 表示问题P的规模; n0为一阈值，表示当问题p的规模不超过n0时，问题已容易直接接触，不必再继续分解。ADHOC(P)是该分治法中的基本子算法，用于直接解小规模的问题p，因此，当p的规模不超过n0时直接用算法ADHOC(P)求解。算法MERGE(y1,y2...,yk)是该分治法中的合并子算法，用于将p的子问题p1,p2,...pk的相应的解y1,y2,....yk合并为p的解。

#### 分治算法最佳实践——汉诺塔

**汉诺塔的传说** 

汉诺塔：汉诺塔(又称河内塔) 问题是源于印度一个古老传说的益智玩具。大梵天创造世界的时候做了三根金刚石柱子，在一根柱子上从下往上按照大小顺序摆着64片黄金圆盘。大梵天命令婆罗门把圆盘从下面开始大小顺序重新摆放在另一跟柱子上，并且规定，在小圆盘上不能放大圆盘，在三根柱子之间一次只能移动一个圆盘。

加入每秒钟一次，共需多长时间呢？移完这些金片需要 5845.54亿年以上，太阳系的预期寿命据说也就是数百亿年。直的超过了 5845.54亿年，地球上的一切生命，连同梵塔，届宇等，都早已经灰飞烟灭。

代码实现：

```java
@SuppressWarnings("all")
public class Hanoitower {
    public static void main(String[] args) {
        hanoiTower(5,'A','B','C');

    }

    //汉诺塔移动的方法
    public static void hanoiTower(int num, char a, char b, char c) {
        //如果只有一个盘
        if (num == 1)
            System.out.println("第1个盘从 " + a + " -->> " + c);
        else {
            //如果我们有n >= 2情况，我们总是可以看做是两个盘
            //1.最下边的一个盘
            //2.上面的所有盘
            //一,先吧，最上面的所有盘 A->B,移动过程中会使用到c
            hanoiTower(num - 1, a, c, b);
            //二,吧最下面的盘 A -> C
            System.out.println("第" + num + "个盘从 " + a + " -->> " + c);
            //三,吧B塔的所有盘 从 B -> C,移动过程使用到A塔
            hanoiTower(num - 1, b, a, c);
        }
    }
}
```

### 动态规划算法

应用场景——背包问题

背包问题：有一个背包，容量为4磅，现有如下物品。

| 物品    | 重量 | 价格 |
| ------- | ---- | ---- |
| 吉他(G) | 1    | 1500 |
| 音响(S) | 4    | 3000 |
| 电脑(L) | 3    | 2000 |

1. 要求达到的目标为装入的背包的总价值最大，并且重量不超出
2. 要求装入的物品不能重复

#### 动态规划算法介绍

1 动态规划(Dynamic Programming) 算法的核心思想是：将大问题划分为小问题进行解决，从而一步步获取最优解决的处理算法

2 动态规划算法与分治算法类似，其基本思想也是将待求解问题分解成诺干个子问题，先求解子问题，然后从这些子问题的解得到原问题的解。

3 与分治法不同的是，==适合用于动态规划求解的问题，经分解得到子问题往往不是相互独立的==。(即下一个子阶段的求解是建立在上一个子阶段的解的基础上，进行进一步的求解)

4 动态规划可以通过==填表的方式==来逐步推进，得到最优解。

#### 动态规划算法最佳实践——背包问题

**思路分析和图解** 

- 背包问题主要是指一个给定容量的背包，若干具有一定价值和重量的物品，如何选择物品放入背包使用物品的价值最大。其中又分 01背包^每件物品都不可重复^ 和 完全背包 (完全背包指的是：每种物品都有无限件可用)
- 这里的问题属于 01背包 ，即每一个物品最多放一个。而==无限背包可以转化为 01背包==。

- 算法的主要思想，利用动态规划来解决。每次遍历到的第i个物品，根据w[i]和v[i]来确定是否需要将物品放入背包中。即对于给定的n个物品，设v[i]，w[i]分别为第i个物品的价值和重量，C为背包的容量。再令`v[i][j]`表示在前i个物品中能够装入容量为j的背包中的最大价值。则我们有下面的结果：

**图解示意图**：

![image-20230714195929888](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307141959508.png)

1 `v[i][0] = v[0][j] = 0;` 表示 填入表 第一行 和 第一列 是0

2 当 `w[i - 1] > j` 时 ->：`v[i][j] = v[i - 1][j]` 当准备加入新增的商品的容量大于 当前背包的容量时，就直接使用上一个单元格的装入策略 i - 1是因为程序中i是从1开始的数组下标从0开始。

3 当 `j >= w[i]` 时：`v[i][j] = max {v[i - 1][j],v[i - 1][j - w[i]]}` 

当准备加入的新增的商品的容量大小等于当前背包 的容量，转入的方式如下：

`v[i - 1][j]`：就是上一个单元格的装入的最大值

`val[i]`：表示当前商品的价值

`v[i - 1][j - w[i]]`：装入 i - 1 商品，到剩余空间 j - w[i] 的最大值

当`j >= w[i]`时：`v[i][j] = max {v[i - 1][j],val[i] + v[i - 1][j - w[i]]}`;

解释`j >= w[i]`公式的含义：如果新的商品装不进去那么就采用原先的`v[i][j] = v[i - 1][j]`装入策略。如果商品可以装进去那么就要看哪一个更大`v[i][j] = max{v[i - 1][j],val[i] + v[i - 1][j - w[i]]}`是`v[i - 1][j]`更大呢还是`val[i] + v[i - 1][j - w[i]]`把新加的商品加入以后再去加上剩余空间对应的最大值两个的和看哪个更大，取最大的

代码实现：

```java
@SuppressWarnings("all")
public class KnapsackProblem {
    public static void main(String[] args) {
        int[] w = {1, 4, 3};//物品的重量
        int[] val = {1500, 3000, 2000};//物品的价格
        int m = 4;//背包的容量
        int n = val.length;//物品的个数
        //n+1因为第一行中都是0不算第一行,m+1因为第一列都是0不算第一列
        //v[i][j]表示在前i个物品中能够装入容量为j的背包中的最大价值
        int[][] v = new int[n + 1][m + 1];
        //为了记录放入商品的 情况，我们定义一个二维数组
        int[][] path = new int[n + 1][m + 1];
        //初始化第一行和第一列，这里在本程序中，可以不去处理，因为默认是0
        for (int i = 0; i < v.length; i++)
            v[i][0] = 0;//将第一列设置为0
        for (int i = 0; i < v[0].length; i++)
            v[0][i] = 0;//将第一行设置0

        //根据前面得到的公式来动态规划处理i,从1 开始因为第一行不做处理
        for (int i = 1; i < v.length; i++)//i +1 第一行不做处理
            for (int j = 1; j < v[0].length; j++) {//j +1 第一列不做处理
                //公式
                if (w[i - 1] > j)//因为程序是i,是从1开始的，因此原来公式中的w[i] 修改成w[i - 1]
                    v[i][j] = v[i - 1][j];
                else {
                    //说明：
                    //因为i从1开始的，因此公式需要调整成
                    //v[i][j] = Math.max(v[i - 1][j], val[i - 1] + v[i - 1][j - w[i - 1]]);
                    //为了记录商品存放到背包的情况，不能直接的使用上面的公式，需要使用if-else来体现公式
                    if (v[i - 1][j] < val[i - 1] + v[i - 1][j - w[i - 1]]) {
                        v[i][j] = val[i - 1] + v[i - 1][j - w[i - 1]];
                        //吧当前的情况记录到path数组中
                        path[i][j] = 1;
                    } else
                        v[i][j] = v[i - 1][j];
                }
            }

        //输出查看情况
        for (int i = 0; i < v.length; i++) {
            for (int j = 0; j < v[i].length; j++)
                System.out.print(v[i][j] + " ");
            System.out.println();
        }

        System.out.println("================================");
        //输出最后我们是放入的哪些商品
        //
//        for (int i = 0; i < path.length; i++) {
//            for (int j = 0; j < path[i].length; j++)
//                if (path[i][j] == 1)
//                    System.out.printf("第%d个商品放入到背包", i);
//            System.out.println();
//        }

        int i = path.length - 1;//行的最大下标
        int j = path[0].length - 1;//列的最大小标
        while(i > 0 && j > 0){//从path的最后开始找
            if(path[i][j] == 1){
                System.out.printf("第>> %d <<个商品放入到背包\n", i);
                j -= w[i - 1];
            }
            i --;
        }
    }
}
```

结果：

```
0 0 0 0 0 
0 1500 1500 1500 1500 
0 1500 1500 1500 3000 
0 1500 1500 2000 3500 
================================
第>> 3 <<个商品放入到背包
第>> 1 <<个商品放入到背包
```

先看代码，如果看了一会儿还是不太懂就看下图：对代码的图解

![image-20230715105010336](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307151050040.png)

### KMP算法

#### 应用场景——字符串匹配问题

**字符串匹配问题**：

1. 有一个字符串 str = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅谷你好" , 和一个子串 str2 = "尚硅谷你尚硅谷你"
2. **现在要判断 str1 是否包含有 str2** ，如果存在，就返回第一次出现的位置，如果没有则返回 -1

#### 暴力匹配算法

如果用暴力匹配的思路，并假设现在str1匹配到i位置，子串str2匹配到j位置，则有：

1. 如果当前字符匹配成功 (即str1[i] == str2[j])，则i++，j++，继续匹配下一个字符
2. 如果匹配失败 (即str1[i] != str2[j])，令 i = i - (j - 1)，j = 0。相当于每次匹配失败时，i回溯，j被置为0
3. 用暴力方法解决的话就会有大量的回溯，每次只移动一位，若是不匹配，移动到下一位接着判断，浪费了大量的时间。(不可行！)

```sh
       						↓ i = 10
str1 = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好"
str2 = "      尚硅谷你尚硅你"
								↑ j = 6
发现 空格≠你，则 i 被回溯：i = i - (j-1) : 
i = 10 - (6-1) = 10 - 5 = 5
					 ↓ 回溯到这里，i=5		
str1 = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好"
str2 = "        尚硅谷你尚硅你"
				    ↑ j = 0

为什么要回溯到 5 的位置？当 i=4 时，直到 i=10，才不匹配
i 回溯时，相当于匹配失败，只前进 1 个字符串，然后再重新匹配。

================= 如果不回溯 ========================
如果失败的时候不回溯到最前面，直接从失败的地方开始匹配
       						           ↓ i = 17
str1 = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好"
str2 = "                 尚硅谷你尚硅你"
								           ↑ j = 6
那么下一次再继续匹配，则从 18 开始了，就永远都匹配不上：
       						             ↓ i = 18
str1 = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好"
str2 = "                             尚硅谷你尚硅你"
								            			  ↑ j = 6

================= 回溯匹配 - 成功 ========================
       						        ↓ i = 15
str1 = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好"
str2 = "                        尚硅谷你尚硅你"
														↑ j = 6


```

**可以看到**：暴力方法解决会有 ==大量的回溯== ，每次只移动一位，若是不匹配，移动到下一位接着判断匹配，浪费了大量的时间。

**暴力匹配算法实现**：

```java
@SuppressWarnings("all")
public class ViolenceMath {
    public static void main(String[] args) {
        String str1 = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好";
        String str2 = "尚硅谷你尚硅你";
        int index = violenceMath(str1,str2);
        System.out.println("index = "+index);
    }

    //暴力匹配算法实现
    public static int violenceMath(String str1, String str2) {
        char[] s1 = str1.toCharArray();
        char[] s2 = str2.toCharArray();
        int s1Len = s1.length;
        int s2Len = s2.length;
        int i = 0;//i索引指向s1
        int j = 0;//j索引指向s2
        while (i < s1Len && j < s2Len) {//保证匹配时，不越界
            if (s1[i] == s2[j]) {//匹配成功
                i++;
                j++;
            } else {//没有匹配成功
                i = i - (j - 1);
                j = 0;
            }
        }
        //判断是否匹配成功
        if(j == s2Len)
            return i - j;
        else
            return -1;
    }
}
```

结果：

```
index = 15
```

#### KMP算法介绍

1 KMP是一个解决模式串在文本串是否出现过，如果出现过，最早出现的位置的经典算法。

2 knuth-Morris-Pratt字符串查找算法，简称为 "KMP算法"，常用与在一个文本串S内查找一个模式串P的出现位置，这个算法由Donald knuth，Vaughan Pratt，James H. Morris三人于1977年联发表，故取这三人的姓氏命名此算法。

3 MKP方法算法就利用之前判断过信息，通过一个next数组，保存模式串中前后最长公共子序列的长度，每次回溯时，通过next数组找到，前面匹配过的位置，省去了大量的计算时间

4 参考文档：https://www.cnblogs.com/ZuoAndFutureGirl/p/9028287.html

#### KMP算法最佳应用——字符串匹配问题

**字符串匹配问题**：

1 有一个字符串str1 = "BBC ABCDAB ABCDABCDABDE"，和一个子串 str2 = "ABCDABD"

2 现在要判断str1是否包含有str2，如果存在，就返回第一次出现的位置，如果没有则返回-1

3 要求：使用KMP算法完成判断，不能使用简单的暴力匹配算法

**思路分析图解** 

​											部分匹配值表的图解

![image-20230715160040538](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307151600018.png)

​												kmp搜索算法图解

![image-20230715160200447](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307151602724.png)

代码实现：

```java
@SuppressWarnings("all")
public class KMPAlgorithm {
    public static void main(String[] args) {
        String str1 = "BBC ABCDAB ABCDABCDABDE";
        String str2 = "ABCDABCD";
        int[] next = kmpNext(str2);//[0, 0, 0, 0, 1, 2, 0]
        System.out.println(Arrays.toString(next));
        int index = kmpSearch(str1, str2, next);
        System.out.println("index = " + index);//index = 15
    }

    /**
     * kmp搜索算法
     *
     * @param str1 原字符串
     * @param str2 子串
     * @param next 部分匹配表,子串对应的部分匹配表
     * @return 如果是 -1说明没有匹配到，否则返回第一个匹配到的位置
     */
    public static int kmpSearch(String str1, String str2, int[] next) {
        //遍历
        for (int i = 0, j = 0; i < str1.length(); i++) {
            //需要处理str1.charAt(i) != str2.charAt(j)，去调整j的大小
            //Kmp算法核心点
            while (j > 0 && str1.charAt(i) != str2.charAt(j))
                j = next[j - 1];
            if (str1.charAt(i) == str2.charAt(j))
                j++;
            if (j == str2.length())
                return i - j + 1;
        }
        return -1;
    }

    /**
     * 获取到一个字符串(子串) 的部分匹配值表
     *
     * @param dest
     * @return
     */
    public static int[] kmpNext(String dest) {
        //创建一个next 数组保存部分匹配值
        int[] next = new int[dest.length()];
        next[0] = 0; //如果字符串长度为1,部分匹配值就是0
        for (int i = 1, j = 0; i < dest.length(); i++) {
            //当dest.charAt(i) == dest.charAt(j) 我们需要从next[j - 1]获取新的j
            //直到我们发现有dest.charAt(i) == dest.charAt(j)成立才退出
            //这是kmp算法的核心点
            while (j > 0 && dest.charAt(i) != dest.charAt(j))
                //回溯
                j = next[j - 1];
            //当dest.charAt(i) == dest.charAt(j) 满足时，部分匹配值就是 +1
            if (dest.charAt(i) == dest.charAt(j))
                j++;
            next[i] = j;
        }
        return next;
    }
}
```

结果：

```
[0, 0, 0, 0, 1, 2, 3, 4]
index = 11
```

### 贪心算法

#### 应用场景——集合覆盖问题

假设存在下面需要付费的广播台，以及广播台信号可以覆盖的地区。

**如何选择最少的广播台**，让所有的地区都可以接收到信号？

| 广播台 | 覆盖地区               |
| ------ | ---------------------- |
| K1     | "北京", "上海", "天津" |
| K2     | "广州", "北京", "深圳" |
| K3     | "成都", "上海", "杭州" |
| K4     | "上海", "天津"         |
| K5     | "杭州", "大连"         |

#### 贪心算法介绍

1 贪心算法(贪婪算法) 是指在对问题进行求解时，在每一步选择中都采取最好或者最优(即最有利)的选择，从而希望能够导致结果是最好或者最优的算法。

2 贪心算法所得到的结果==不一定是最优的结果==(有时候会是最优解)，但是都是相对近似(接近)最优解的结果。

#### 思路分析：

如何找出覆盖所有地区的广播台的集合呢，使用穷举法实现，列出每个可能的广播台的集合，这被称为幂集。假设总的有n个广播台，则广播台的组合总共有2^n^-1个，假设每秒可以计算10个子集，如图：

| 广播台数量n | 子集总数2ⁿ | 需要的时间 |
| ----------- | ---------- | ---------- |
| 5           | 32         | 3.2秒      |
| 10          | 1024       | 102.4秒    |
| 32          | 4294967296 | 13.6年     |
| 100         | 1.26*100³º | 4x10²³年   |

**由此可见**：在进行组合的场景下，使用组合效率是很低的。

#### 使用贪心算法，效率高：

目前并没有算法可以快速计算得到准备的值，使用贪心算法，则可以得到非常接近的解，并且效率高。选择策略上，因为需要覆盖全部地区的最小集合：

1.  遍历所有的广播电台，找到一个覆盖了最多<font style="color:red">未覆盖的地区</font>的电台(此电台可能包含一些已覆盖的地区，但没有关系)
2.  将这个电台加入到一个集合中(比如ArrayList)，想办法把该电台覆盖的地区在下次比较时去掉。
3.  重复第1步直到覆盖了全部的地区。

图解：

![image-20230715221702246](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307152217484.png)

代码实现：

```java
@SuppressWarnings("all")
public class GreedyAlgorithm {
    public static void main(String[] args) {
        //创建广播电台，放入到HashMap
        HashMap<String, HashSet<String>> broadcasts = new HashMap<String, HashSet<String>>();
        //将各个电台放入到broadcasts
        HashSet<String> hashSet1 = new HashSet<>();
        hashSet1.add("北京");
        hashSet1.add("上海");
        hashSet1.add("天津");
        HashSet<String> hashSet2 = new HashSet<>();
        hashSet2.add("广州");
        hashSet2.add("上海");
        hashSet2.add("深圳");
        HashSet<String> hashSet3 = new HashSet<>();
        hashSet3.add("成都");
        hashSet3.add("上海");
        hashSet3.add("杭州");
        HashSet<String> hashSet4 = new HashSet<>();
        hashSet4.add("上海");
        hashSet4.add("天津");
        HashSet<String> hashSet5 = new HashSet<>();
        hashSet5.add("杭州");
        hashSet5.add("大连");
        //将所有电台加入到Map集合中
        broadcasts.put("K1", hashSet1);
        broadcasts.put("K2", hashSet2);
        broadcasts.put("K3", hashSet3);
        broadcasts.put("K4", hashSet4);
        broadcasts.put("K5", hashSet5);

        HashSet<String> allAreas = new HashSet<>();
        Collections.addAll(allAreas, "北京", "上海", "天津", "广州", "深圳", "成都", "杭州", "大连");

        //创建一个ArrayList存放选择的电台集合
        ArrayList<String> selects = new ArrayList<>();

        //定义临时集合保存在遍历过程中的电台覆盖的地区和当前还没有覆盖的地区的交集
        HashSet<String> tempSet = new HashSet<>();

        //定义maxKey保存在一次遍历过程中，能够覆盖的最多位覆盖的地区对应的电台的Key
        String maxKey = null;
        //如果maxKey不为null则加入到selects中
        while (allAreas.size() != 0) {//如果allAreas不为0,则表示还没有覆盖到所有的地区
            maxKey = null;
            for (String key : broadcasts.keySet()) {
                tempSet.clear();
                HashSet<String> areas = broadcasts.get(key);
                tempSet.addAll(areas);
                //求出tempSet和allAreas集合的交集,交集会赋给tempSet
                tempSet.retainAll(allAreas);
                //如果当前集合包含的未覆盖地区的数量比maxKey指向的集合未覆盖的地区还要多
                //就需要重置maxKey
                if (tempSet.size() > 0 && (maxKey == null || tempSet.size() > broadcasts.get(maxKey).size()))
                    maxKey = key;
            }
            //maxKey != null，就应该将maxKey加入selects
            if (maxKey != null) {
                selects.add(maxKey);
                //将maxKey指向的广播电台覆盖的地区，从allAreas去掉
                allAreas.removeAll(broadcasts.get(maxKey));
            }
        }
        System.out.println("得到的选择结果是: " + selects);//[k1,k2,k3,k5]
    }
}
```

#### 贪心算法注意事项和细节

1 贪心算法所得到的结果不一定是最优的结果(有时候会是最优解)，但是都是相对近似(接近)最优解的结果。

2 比如上题的算法选出的是K1，K2，K3，K5，符合覆盖了全部的地区但是我们发现K2，K3，K4，K5，也可以覆盖全部地区，如果K2的使用成本低于K1那么上题的K1，K2，K3，K5，虽然满足条件，但并不是最优解。

### 普利姆算法

应用场景和问题：

![image-20230716093030683](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307160930413.png)

1 有胜利乡有7个村庄(A,B,C,D,E,F,G)，现在需要修路把7个村庄连通。

2 各个村庄的距离用边线表示(权)，比如A - B 距离 5 公里。

3 问：如何修路保证各个村庄都能连通，并且总的修建公路总里程最短？

**思路**：

​	将10条边，链接即可，但是总的里程数不是最小。

**正确的思路**：

​	就是尽可能的选择少的路线，并且每条路线最小，保证总里程数最少。

#### 最小生成树

修路问题本质就是最小生成树问题，先介绍一下何为最小生成树(Minimum Cost Spanning Tree)，简称MST。

1 给定一个带权的无向连通图，如何选取一棵生成树，使树上所有边上权的总和为最小，这叫最小生成树

2 N个顶点，一定有N-1条边

3 包含全部顶点

4 N-1条边都在图中

5 举例说明(如图)

![image-20230716093418628](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307160934701.png)

6 求最小生成树的算法主要是==普利姆算法==和==克鲁斯卡尔算法== 

#### 普利姆算法介绍

1 普利姆(Prim)算法求最小生成树，也就是在包含n个顶点的连通图中，找出只有(n-1)条边包含所有n个顶点的连通子图，也就是所谓的==极小连通子图== 

2 普利姆的算法如下：

1.  设G=(V,E)是连通网。T=(U,D)是最小生成树。V,U是顶点集合。E,D是边的集合。
2.  若从顶点u开始构造最小生成树，则从集合V中取出顶点u放入集合U中，标记顶点v的visited[u] = 1
3.  若集合U中顶点ui与集合V-U中的顶点vj之间存在边，则寻找这些边中权值最小的边，但不能构成回路，将顶点vj加入集合U中，将边(ui,vj)加入集合D中，标记visited[vj] = 1
4.  重复步骤②，直到U与V相等，即所有顶点都被标记为访问过，此时D中有n-1条边
5.  ==提示==：单独看步骤很难理解，我们通过代码来讲解，比较好理解。

#### 普利姆算法图解

上面的文字描述很难理解，下面是图解分析

![image-20230716102343503](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307161023485.png)

代码实现：

```java
@SuppressWarnings("all")
public class PrimAlgorithm {
    public static void main(String[] args) {
        //测试图是否创建成功
        char[] data = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int vertx = data.length;
        //邻接矩阵的关系使用二维数组表示，10000这个大数，表示两个点不连通
        int[][] weight = {{10000, 5, 7, 10000, 10000, 10000, 2},
                {5, 10000, 10000, 9, 10000, 10000, 3},
                {7, 10000, 10000, 10000, 8, 10000, 10000},
                {10000, 9, 10000, 10000, 10000, 4, 10000},
                {10000, 10000, 8, 10000, 10000, 5, 4},
                {10000, 10000, 10000, 4, 5, 10000, 6},
                {2, 3, 10000, 10000, 4, 6, 10000}};
        //创建MGraph对象
        MGraph mGraph = new MGraph(vertx);
        //创建一个MiniTree对象
        MiniTree minTree = new MiniTree();
        minTree.createGraph(mGraph, vertx, data, weight);
        //输出
        minTree.showGraph(mGraph);
        //测试普利姆算法
        minTree.prim(mGraph,0);
    }
}

//创建最小生成树
@SuppressWarnings("all")
class MiniTree {

    /**
     * 创建图的邻接矩阵
     *
     * @param graph  图对象
     * @param vertx  图对应的顶点个数
     * @param data   图的各个顶点的值
     * @param weight 图的邻接矩阵
     */
    public void createGraph(MGraph graph, int vertx, char[] data, int[][] weight) {
        int i, j;
        for (i = 0; i < vertx; i++) {//顶点
            graph.data[i] = data[i];
            for (j = 0; j < vertx; j++)
                graph.weight[i][j] = weight[i][j];
        }
    }

    /**
     * 显示图的邻接矩阵
     *
     * @param graph
     */
    public void showGraph(MGraph graph) {
        for (int[] itemp : graph.weight)
            System.out.println(Arrays.toString(itemp));
    }

    /**
     * 编写prim算法，得到最小生成树
     *
     * @param graph 图
     * @param v     表示从图的第几个顶点开始生成 'A' -> 0 , 'B' -> 1 ...
     */
    public void prim(MGraph graph, int v) {
        //标记节点(顶点)是否被访问过
        int[] visited = new int[graph.vertx];
        //visited[] 默认元素的值都是0，表示没有访问过,Java中默认为0所以不能特意进行次操作
        //for(int i = 0;i < graph.vertx;i ++)
        //visited[i] = 0;
        //把当前节点标记为已访问
        visited[v] = 1;
        //h1,h2记录两个顶点的下标
        int h1 = -1;
        int h2 = -1;
        //将miniweight初始成一个大数，后面在遍历过程中，会被替换
        int miniWeight = 10000;
        //因为有graph.vertx顶点，普利姆算法结束后，有graph.vertx - 1 边
        for (int k = 1; k < graph.vertx; k++) {
            //遍历当前顶点可连通的所有的顶点
            //确定每一次生成的子图，和哪个节点的距离最近
            for (int i = 0; i < graph.vertx; i++)//i节点表示被访问过的节点
                for (int j = 0; j < graph.vertx; j++)//j节点表示没有被访问过的节点
                    if (visited[i] == 1 && visited[j] == 0 && graph.weight[i][j] < miniWeight) {
                        //替换miniweight(寻找已经访问过的节点和为访问过的节点间的权值最小的边)
                        miniWeight = graph.weight[i][j];
                        h1 = i;
                        h2 = j;
                    }
            //找到一条边是最小
            System.out.println("边 <" + graph.data[h1] + "," + graph.data[h2] + "> 权值: " + miniWeight);
            //将当前这个节点标记为已访问过
            visited[h2] = 1;
            //将 miniWeight 重新设置为 最大值
            miniWeight = 10000;
        }
    }
}

@SuppressWarnings("all")
class MGraph {
    int vertx;//表示图的节点个数
    char[] data;//存放节点数据
    int[][] weight;//存放边，就是邻接矩阵

    public MGraph(int vertx) {
        this.vertx = vertx;
        data = new char[vertx];
        weight = new int[vertx][vertx];
    }
}
```

结果：

```
[10000, 5, 7, 10000, 10000, 10000, 2]
[5, 10000, 10000, 9, 10000, 10000, 3]
[7, 10000, 10000, 10000, 8, 10000, 10000]
[10000, 9, 10000, 10000, 10000, 4, 10000]
[10000, 10000, 8, 10000, 10000, 5, 4]
[10000, 10000, 10000, 4, 5, 10000, 6]
[2, 3, 10000, 10000, 4, 6, 10000]
边 <A,G> 权值: 2
边 <G,B> 权值: 3
边 <G,E> 权值: 4
边 <E,F> 权值: 5
边 <F,D> 权值: 4
边 <A,C> 权值: 7
```

### 克鲁斯卡尔算法

**公交站问题**：

![image-20230716112527142](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307161125011.png)

1 某城市新增7个站点(A,B,C,D,E,F,G)，现在需要修路把7个站点连通

2 各个站点的距离用边线表示(权)，比如A-B距离12公里

3 问：如何修路保证各个站点都能连通，并且总的修建公路总里程最短？

#### 克鲁斯卡尔介绍

1.  克鲁斯卡尔(Kruskal)算法，是用来求加权连通图的最小生成树的算法。
2.  **基本思想**：
    -  按照权值从小到大的顺序选择n-1条边，并保证n-1条边不构成回路
3.  **具体做法**：
    -  首先构造一个只含n个顶点的森林，然后依权值从小到大连通网中选择边加入到森林中，并使森林中不产生回路，直至森森变成一颗树为止。

在含有n个顶点的连通图中选择n-1条边，构成一颗极小连通子图，并使该连通子图中n-1条边上权值之和达到最小，则称其为连通网的最小生成树。

![image-20230716133935680](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307161339514.png)

例如，对于如上图G4所示的联通网可以有多颗权值总和不相同的生成树。

#### 克鲁斯卡尔算法图解

以上图G4为例，来对克鲁斯卡尔进行演示(假设，用数组R保存最小生成树结果)

![image-20230716134223900](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307161342469.png)

第1步：将边<E,F>加入R中

-  边<E,F>的权值最小，因此将它加入到最小生成树结果R中。

第2步：将边<C,D>加入R中

-  上一步操作后，边<C,D>的权值最小，因此将它加入到最小生成树结果R中。

第3步：将边<D,E>加入R中

-  上一步操作后，边<D,E>的权值最小，因此将它加入到最小生成树结果R中。

第4步：将边<B,F>加入R中

-  上一步操作后，边<C,E>的权值最小，但<C,E>会和已有的边构成回路；因此，跳过<C,E>。同理，跳过边<C,F>。将边<B,F>加入到最小生成树结果R中。

第5步：将边<E,G>加入R中

-  上一步操作后，边<E,G>的权值最小，因此将它加入到最小生成树结果R中

第6步：将边<A,B>加入R中

-  上一步操作后，边<F,G>的权值最小，但<F,G>会和已有的边构成回路；因此，跳过边<F,G>。同理，跳过边<F,G>。同理，跳过边<B,C>。将边<A,B>加入到最小生成树结果R中。

此时，最小生成树构造完成！它包括的边依次是：<E,F> <C,D> <D,E> <B,F> <E,G> <A,B>。

#### 克鲁斯卡尔算法分析

根据前面介绍的克鲁斯卡尔算法的基本思想和做法，我们能够了解到，克鲁斯卡尔算法重点需要解决的以下两个问题：

**问题一**：对图的所有边按照权值大小进行排序

**问题二**：将边添加到最小生成树中时，怎么样判断是否形成了回路。

问题一很好解决，采用排序算法进行排序即可。

问题二，处理方式是：记录顶点在"最小生成树"中的终点，顶点的终点是"在最小生成树中与它连通的最大顶点"。然后每次需要将一条边添加到最小生成树时，判断该边的两个顶点的重点是否重合，重合的 话则会构成回路。

#### 如何判断是否构成回路——举例说明(如图)

![image-20230716151306831](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307161513317.png)

在将<E,F> <C,D> <D,E> 加入到最小生成树R中之后，这几条边的顶点就都有了终点：

1.  C的终点是F
2.  D的终点是F
3.  E的终点是F
4.  F的终点是F

关于终点的说明：

1.  这就是将所有顶点按照从小到大的顺序排列好之后；某个顶点的终点就是"与它连通的最大顶点"。
2.  因此，接下来，虽然<C,E>是权值最小的边。但是C和E的终点都是F，即它们的终点相同，因此，将<C,E>加入最小生成树的话，会形成回路。这就是判断回路的方式。也就是说，**我们加入的<font style="color:blue">边</font>的<font style="color:blue">两个顶点</font>不能<font style="color:red">都指向同一个终点</font>，否则将构成回路**。

代码实现：

```java
@SuppressWarnings("all")
public class Kruskal {
    private int edgeNum;//边的个数
    private char[] vertexs;//顶点数目
    private int[][] matrix;//邻接矩阵
    private static int INF = Integer.MAX_VALUE;//使用INF表示两个顶点不能连通

    //构造器
    public Kruskal(char[] vertexs, int[][] matrix) {
        //初始化定点数和边的个数
        int vlen = vertexs.length;
        //初始化顶点
        this.vertexs = new char[vlen];
        //拷贝的方式，使得构造函数中的vertexs和成员字段vertexs互不影响
        for (int i = 0; i < vertexs.length; i++)
            this.vertexs[i] = vertexs[i];
        //初始化边
        this.matrix = new int[vlen][vlen];
        for (int i = 0; i < vertexs.length; i++)
            for (int j = 0; j < vertexs.length; j++)
                this.matrix[i][j] = matrix[i][j];
        //统计边的条数
        for (int i = 0; i < vlen; i++)
            for (int j = i + 1; j < vlen; j++)
                if (this.matrix[i][j] != INF)
                    edgeNum++;
    }

    public void kruskal() {
        int index = 0;//表示最后结果数组的索引
        int[] ends = new int[edgeNum];//用于保存"已有最小生成树"中的每个顶点在最小生成树中的终点
        //创建结果数组，保存最后的最小生成树
        EData[] rets = new EData[edgeNum];
        //获取图中 所有的边的集合，一共有12边
        EData[] edges = getEdges();
        System.out.println("图的边的集合= " + Arrays.toString(edges) + " 共 " + edges.length);//12
        //按照边的权值大小进行排序(从小到大)
        sortEdges(edges);
        //遍历edges数组，将边添加到最小生成树中时，判断是准备加入的边是否形成了回路，如果没有，就加入rets，否则不能加入
        for (int i = 0; i < edgeNum; i++) {
            //获取到第i条边的第一个顶点
            int p1 = getPosition(edges[i].start);
            //获取到第i条边的第二个顶点
            int p2 = getPosition(edges[i].end);
            //获取p1这个顶点在已有最小生成树中的终点
            int m = getEnd(ends, p1);
            //获取p2这个顶点在已有最小生成树中的终点
            int n = getEnd(ends, p2);
            //是否构成回路
            if (m != n){
                //设置m在 "已有最小生成树" 中的终点
                ends[m] = n;
                //有一条边加入到rets数组中
                rets[index ++] = edges[i];
            }
        }
        System.out.println("----------------最小生成树为----------------");
        for(int i = 0;i < index;i ++)
            System.out.println(rets[i]);
    }

    //打印邻接矩阵
    public void print() {
        System.out.println("邻接矩阵为\n");
        for (int i = 0; i < vertexs.length; i++) {
            for (int j = 0; j < vertexs.length; j++)
                System.out.printf("%12d", matrix[i][j]);
            System.out.println();
        }
    }

    //对边进行排序处理，冒泡排序处理
    private void sortEdges(EData[] edges) {
        for (int i = 0; i < edges.length; i++)
            for (int j = 0; j < edges.length - 1 - i; j++)
                if (edges[j].weight > edges[j + 1].weight) {
                    EData temp = edges[j];
                    edges[j] = edges[j + 1];
                    edges[j + 1] = temp;
                }
    }

    /**
     * @param ch 顶点的值,比如'A','B'
     * @return 返回ch顶点对应的下标, 如果找不到返回-1
     */
    private int getPosition(char ch) {
        for (int i = 0; i < vertexs.length; i++)
            if (vertexs[i] == ch)
                return i;
        return -1;
    }

    /**
     * comment: 获取图中边,放到EData[]数组中，后面我们需要遍历该数组
     * 是通过matrix邻接矩阵来获取
     * EData形式 [['A','B',12],['B','F',7],...]
     *
     * @return
     */
    private EData[] getEdges() {
        int index = 0;
        EData[] edges = new EData[edgeNum];
        for (int i = 0; i < vertexs.length; i++)
            for (int j = i + 1; j < vertexs.length; j++)
                if (matrix[i][j] != INF)
                    edges[index++] = new EData(vertexs[i], vertexs[j], matrix[i][j]);
        return edges;
    }

    /**
     * comment: 获取下标为i的顶点的终点，用于后面判断两个顶点的终点是否相同
     *
     * @param ends 数组就是记录了各个顶点对应的终点是哪个，ends数组是在遍历过程中，逐步形成的(不是一次形成的)
     * @param i    表示传入的顶点对应的下标
     * @return 返回的就是 下标为i的这个顶点对应的终点的下标
     */
    private int getEnd(int[] ends, int i) {
        while (ends[i] != 0)
            i = ends[i];
        return i;
    }

    public static void main(String[] args) {
        char[] vertexs = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        //克鲁斯卡尔算法的邻接矩阵
        int matrix[][] = {
                /*A*//*B*//*C*//*D*//*E*//*F*//*G*/
                /*A*/ {0, 12, INF, INF, INF, 16, 14},
                /*B*/ {12, 0, 10, INF, INF, 7, INF},
                /*C*/ {INF, 10, 0, 3, 5, 6, INF},
                /*D*/ {INF, INF, 3, 0, 4, INF, INF},
                /*E*/ {INF, INF, 5, 4, 0, 2, 8},
                /*F*/ {16, 7, 6, INF, 2, 0, 9},
                /*G*/ {14, INF, INF, INF, 8, 9, 0}};
        //大家可以在去测试其它的邻接矩阵，结果都可以得到最小生成树.
        //创建对象
        Kruskal krusKal = new Kruskal(vertexs, matrix);
        krusKal.print();
        krusKal.kruskal();
    }
}

@SuppressWarnings("all")
//创建一个类EData,它的对象实力就表示一条边
class EData {
    char start;//边的一个点
    char end;//边的另外一个点
    int weight;//边的权值

    public EData(char start, char end, int weight) {
        this.start = start;
        this.end = end;
        this.weight = weight;
    }

    @Override
    public String toString() {
        return "EData{" +
                 "<" + start +
                " , " + end + ">" +
                " , weight=" + weight +
                '}';
    }
}
```

结果：

```
邻接矩阵为

           0          12  2147483647  2147483647  2147483647          16          14
          12           0          10  2147483647  2147483647           7  2147483647
  2147483647          10           0           3           5           6  2147483647
  2147483647  2147483647           3           0           4  2147483647  2147483647
  2147483647  2147483647           5           4           0           2           8
          16           7           6  2147483647           2           0           9
          14  2147483647  2147483647  2147483647           8           9           0
图的边的集合= [EData{<A , B> , weight=12}, EData{<A , F> , weight=16}, EData{<A , G> , weight=14}, EData{<B , C> , weight=10}, EData{<B , F> , weight=7}, EData{<C , D> , weight=3}, EData{<C , E> , weight=5}, EData{<C , F> , weight=6}, EData{<D , E> , weight=4}, EData{<E , F> , weight=2}, EData{<E , G> , weight=8}, EData{<F , G> , weight=9}] 共 12
----------------最小生成树为----------------
EData{<E , F> , weight=2}
EData{<C , D> , weight=3}
EData{<D , E> , weight=4}
EData{<B , F> , weight=7}
EData{<E , G> , weight=8}
EData{<A , B> , weight=12}
```

### 迪杰斯特拉算法

应用场景问题：

![image-20230716205732320](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307162057769.png)

1.  战争时期，胜利乡有7个村庄(A,B,C,D,E,F,G)，现在有六个邮差，从G点出发，需要分别把邮件分别送到A,B,C,D,E,F六个村庄
2.  各个村庄的距离用边线表示(权)，比如 A-B距离5公里
3.  问：如何计算出G村庄到 其它各个村庄的最短距离？
4.  如果从其它点出发到各个点的最短距离又是多少？

#### 迪杰斯特拉(Dijkstra)算法介绍

迪杰斯特拉(Dijkstra)算法是**典型的最短路径算法**，用于计算一个节点到其它节点的最短路径。它的主要特点是以起始点为中心向外层层扩展(==广度优先搜索思想==)，直到扩展到终点为止。

#### 迪杰斯特拉(Dijkstra)算法过程

设置出发顶点为v，顶点集合V{v1,v2,vi,...}，v到V中各个顶点的距离构成距离集合Dis，Dis{d1,d2,di,...}，Dis集合记录着v到图中各个顶点的距离(到自身可以看作0，v到vi距离对应为di)

1 从Dis中选择值最小的di并移除Dis集合，同时移出V集合中对应的顶点vi，此时的v到vi即位最短路径

2 更新Dis集合，更新规则为：比较v到V集合中顶点的距离值，与v通过vi到V集合中顶点的距离值，保留值较小的一个(同时也应该更新顶点的前驱节点为vi，表明是通过vi到达的)

3 重复执行两步骤，直到最短路径顶点为目标顶点即可结束。

​	