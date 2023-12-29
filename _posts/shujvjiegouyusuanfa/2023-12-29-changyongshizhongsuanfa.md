---
layout: post
title: 程序员常用的10中算法
categories: [java,数据结构与算法,算法]
description: 程序员常用的10中算法的介绍和实现
keywords: 算法
---

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
