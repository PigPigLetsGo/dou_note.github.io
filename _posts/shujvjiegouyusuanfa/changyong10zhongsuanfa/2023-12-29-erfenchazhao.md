---
layout: post
title: 二分查找(非递归)
categories: [java,程序员常用算法]
description: 程序员常用算法-二分查找（非递归）
keywords: 算法
---

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
             // 判断中间的值是否小于目标值
            if (arr[mid] < value) {
                // 如果中间值小于目标值则将指针向后移动
                left = mid + 1; // 应该向后移动
            } 
            // 判断中间值是否大于目标值
            else if (arr[mid] > value) {
                // 如果中间值大于了目标值则将指针向前移动
                right = mid - 1; // 应该向前移动
            } 
            else {
                // 找到目标值
                return mid;
            }
            //如果没有找到则返回-1
            return -1;
    }
}
```
