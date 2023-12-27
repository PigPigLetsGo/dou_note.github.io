---
layout: post
title:  队列
categories: [java]
description: 队列的详细介绍。
keywords: 数据结构
---

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