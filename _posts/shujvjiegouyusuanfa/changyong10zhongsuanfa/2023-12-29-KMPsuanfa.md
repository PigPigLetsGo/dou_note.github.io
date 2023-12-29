---
layout: post
title: KMP 算法
categories: [java,程序员常用算法]
description: 程序员常用算法-KMP 算法
keywords: 算法
---

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
