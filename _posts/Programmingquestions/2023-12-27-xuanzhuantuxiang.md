---
layout: post
title:  旋转图像
categories: [java,算法,编程题]
description: 旋转图像。
keywords: 编程题
---

题目要求:
给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。
你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。
作者：力扣 (LeetCode)
链接：https://leetcode.cn/leetbook/read/top-interview-questions-easy/xnhhkv/

示例:

![1](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202312272044174.jpg)提示:
n == matrix.length == matrix[i].length
1 <= n <= 20
-1000 <= matrix[i][j] <= 1000
思路: 
1.定义for循环遍历长度/2减去一半,交换上层和 长度-i-1层即可如果是3*3则交换上层和下层
2.定义for循环进行对折交换
3.打印

```java
public static void rotate(int[][] matrix) {
        int len = matrix.length;
        //上下交换位置
        //将长度除2,因为只循环交换前后位置
        for(int i = 0;i < len / 2;i ++){
            //将数组的一维的前面的放在tmp中
            int[] tmp = matrix[i];
            //将数组的一维长度最后的位置放到最前面的位置
            matrix[i] = matrix[len - i - 1];
            //再将前面的放入到最后面
            matrix[len - i - 1] = tmp;
            //完成交换位置  也就是   789
            //                456
            //              123
        }
        //对折交换位置
        for(int i = 0;i < len;i ++){
            for(int j = i + 1;j < len;j ++){
                //将数组中的数据对折交换位置
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = tmp;
            }
        }
        //交换完成    741
        //        852
        //        963
        //打印格式
        for(int[] num : matrix){
            System.out.println("");
            for(int i : num){
                System.out.print(i);
                System.out.print(" ");
            }
        }
    } 
```