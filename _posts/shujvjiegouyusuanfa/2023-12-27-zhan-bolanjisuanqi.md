---
layout: post
title:  栈
categories: [java,数据结构与算法,栈]
description: 栈的详细介绍与波兰计算器。
keywords: 数据结构
---

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
