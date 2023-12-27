---
layout: post
title:  选择排序
categories: [java,数据结构与算法,算法,排序]
description: 选择排序的详细介绍。
keywords: 数据结构
---

### 选择排序

**基本介绍** 

选择式排序也属于内部排序法，是从欲排序的数据中，按指定的规则选出某一元素，再依规定交换位置后达到排序的目的。

**选择排序思想**: 

选择排序（select sorting）也是一种简单的排序方法。它的**基本思想**是：第一次从arr[0]`~`arr[n-1]中选取最小值，与arr[0]交换，第二次从arr[1]`~`arr[n-1]中选取最小值，与arr[1]交换，第三次从arr[2]`~`arr[n-1]中选取最小值，与arr[2]交换，…，第i次从arr[i-1]~arr[n-1]中选取最小值，与arr[i-1]交换，…, 第n-1次从arr[n-2]~arr[n-1]中选取最小值，与arr[n-2]交换，总共通过n-1次，得到一个按排序码从小到大排列的有序序列。

**选择排序思路分析图***: 

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161901514.png)
101, 34, 119, 1

代码实现：

```java
public static void selectionSort(int[] arr) {
   /*判断数组为空或为一个元素的情况，即边界检查*/
   if (arr == null || arr.length < 2) {
      return;
   }

   /*每次要进行比较的两个数，的前面那个数的下标*/
   for (int i = 0; i < arr.length - 1; i++) { 
      //min变量保存该趟比较过程中，最小元素所对应的索引，
      //先假设前面的元素为最小元素
      int minIndex = i;
      /*每趟比较，将前面的元素与其后的元素逐个比较*/
      for (int j = i + 1; j < arr.length; j++) {
         //如果后面的元素小，将后面元素的索引极为最小值的索引
         if(arr[j] < arr[minIndex]) {
            minIndex = j;
         }
      }
      //然后交换此次查找到的最小值和原始的最小值
      swap(arr, i, minIndex);
   }
}

public static void swap(int[] arr, int i, int j) {
   int tmp = arr[i];
   arr[i] = arr[j];
   arr[j] = tmp;
}
```

-  选择排序的优化

```java
public class Demo {
    public static void main(String[] args) {
        int[] arr = {-1,-2,3,1,20,10,9,5};
        sort(arr);
    }
    public static void sort(int[] arr){
        int left = 0;
        int right = arr.length - 1;
        while(left < right){
            int max = right;
            int min = left;
            for(int i = left;i <= right;i ++){
                if(arr[max] < arr[i])
                    max = i;
                if(arr[min] > arr[i])
                    min = i;
            }
            //最大值放在最右端
            int temp = arr[max];
            arr[max] = arr[right];
            arr[right] = temp;

            if(min == right){
                min = max;
            }
            //最下值放在最左端
            temp = arr[min];
            arr[min] = arr[left];
            arr[left] = temp;
            left ++;
            right --;
        }
        System.out.println(Arrays.toString(arr));
    }
}
```
