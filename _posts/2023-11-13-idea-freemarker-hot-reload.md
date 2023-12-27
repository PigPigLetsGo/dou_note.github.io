---
layout: post
title: Java｜SpringBoot 项目开发时，让 FreeMarker 文件编辑后自动更新
categories: [Java]
description: FreeMarker 文件编辑后，如果每次都需要重启才能看到效果，那效率会非常低下。通过一些配置可以让它们自动更新。
keywords: Java, FreeMarker, IDEA, SpringBoot
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

正在维护的一个 SpringBoot 项目是前后端一体的，页面使用 FreeMarker 编写。在开发过程中，ftl 文件编辑后，每次都需要重启应用才能看到效果，效率非常低下。这里记录通过哪些配置后，可以让它们免重启自动更新。

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
