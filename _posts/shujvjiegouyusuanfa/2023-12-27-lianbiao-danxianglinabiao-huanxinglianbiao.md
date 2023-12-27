---
layout: post
title:  链表
categories: [java,数据结构与算法]
description: 链表的详细介绍。
keywords: 数据结构
---

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
