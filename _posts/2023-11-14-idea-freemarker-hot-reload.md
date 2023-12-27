---
layout: post
title: ASCII
categories: [Java]
description: ascii编码表的介绍
keywords: Java, FreeMarker, IDEA, SpringBoot
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

ascii介绍

# ASCII编码表

![image-20221107113245742](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/image-20221107113245742.png)

如果看不清图片可以进入网站查看 [ascii码 | ascii码对照表](https://ascii.org.cn/#:~:text=Ascii码表 ASCII ((American Standard Code,for Information Interchange)%3A 美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统，主要用于显示现代英语和其他西欧语言。 它是最通用的信息交换标准，并等同于国际标准ISO%2FIEC 646。) 

```java
char ch = 'A';//这是一个字符型变量,里面是大写字母A
System.out.println(ch+1);//66,也就是大写字母A被当做65进行处理
System.out.println(ch);//如果只是输出一个字符而不进行一些计算那么输出为大写A
//计算机的底层会用一个数字(二进制)来代表字符A,就是65
//一旦char类型进行了数学运算,那么字符就会按照一定的规则翻译成为一个数字
```

但是ASCII编码表中表示字符的数字太多了怎么办呢?

我们不必全记住当然也是不可能全部记住所以我们只需要主机几个常用的就行 比如:

0 -> 48 记住了48为0那么推算到9就是 : 57

A -> 65

a -> 97

只需要记住这三个就行,这三个往后可以进行推算出对应的数字都是那些

>  数字和字符的对照关系表(编码表) :
>
>  ASCII码表 : American Standard Code for Information Interchange , 美国信息交换标准代码
>
>  Unicode码表 : 万国码 , 也是数字和符号的对照关系 , 开头0~127部分和ASCII完全一样 , 但是从128开始包含有更多字符

```java
//将char类型的大写A加上十进制的0转换为十进制的数字打印输出
char ch = 'A';
System.out.println(ch+0);//65
//将char类型的大写A转换为int类型
/*
        转换形式: char --> int 隐式转换
         */
int num = ch;
System.out.println(num);
//将中文'与'加上十进制的0转换为十进制的数字打印输出
char ch3 = '与';
System.out.println(ch3+0);//19982
//将char类型的1通过加十进制的0将其转换为十进制的数字打输出
char ch1 = '1';
System.out.println(ch1+0);//输出为49