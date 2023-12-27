---
layout: post
title:  稀疏数组
categories: Algorithm
description: 如何将二维数组转换为稀疏数组。
keywords: 算法
---

## 稀疏数组

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