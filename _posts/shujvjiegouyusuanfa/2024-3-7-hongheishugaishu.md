---
layout: post
title:  红黑树-概述
categories: [java,数据结构与算法,算法,红黑树]
description: 红黑树-概述介绍。
keywords: 算法
---

# 红黑树-概述

红黑树也是一种自平衡的二叉搜索树，较之 AVL，插入和删除时旋转次数更少

**红黑树和AVL树的区别主要在于它们判断平衡的依据不一样**：

之前学习的AVL树就是看左右子树的高度差，是不是超过了1，超过了表示是不平衡的

红黑树判断一颗树是不是平衡

**红黑树与AVL树相比的优势**：

插入和删除时旋转次数更少

## 红黑树特性

1.  所有节点都有两种颜色：红 与 黑
2.  所有null 视为 黑色
3.  红色节点不能相邻
4.  根节点是黑色
5.  从根到任意一个叶子节点，路径中的黑色节点数一样 (黑色完美平衡)

![image-20240204210426638](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240204210426638.png)

第五个规则说明的是，无论到2,4,7,9,11 哪个叶子节点其它们所经过的黑色节点数目都是一样的，比如从 根节点到 2 算上根节点 再加上 3  就是经过两个 黑色节点，从根节点到 4 也同样，从根节点到11 也同样，到9也同样。这时就算是 "黑色完美平衡"