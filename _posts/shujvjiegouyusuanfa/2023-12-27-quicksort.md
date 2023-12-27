---
layout: post
title:  快速排序
categories: [java,数据结构与算法,排序]
description: 快速排序的详细介绍。
keywords: 算法
---

### 快速排序

**快速排序法介绍**:

快速排序（Quicksort）是对冒泡排序的一种改进。基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列

**快速排序法示意图**:

以每个递归分支的最后一个元素为基准进行排序

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902712.png)

以每个递归分支的最中间的元素为基准进行排序

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902376.png)

代码实现:

```java
@SuppressWarnings("all")
public class QuickSort {
    public static void main(String[] args) {
        int[] arr = {-1,-2,3,1,20,10,9,5};
        quickSort(arr,0,arr.length - 1);
        System.out.println(Arrays.toString(arr));
    }

    public static void quickSort(int[] arr,int left,int right){
        int low = left;//左下标
        int max = right;//右下标
        //中间下标
        int pivot = arr[(left + right) / 2];

        //临时交换值变量
        int temp = 0;
        //while循环的目的是让比pivot值小放到左边比pivot值大的放到右边
        while(low < max){
            //在pivot的左边一直找，找到大于pivot值，才退出
            while(arr[low] < pivot){
                low += 1;
            }

            //在pivot的右边一直找，找到小于pivot值，才退出
            while(arr[max] > pivot){
                max -= 1;
            }

            //如果low >= max说明pivot 的左右两边的值，已经按照左边全部是
            //小于等于pivot的值，右边全部是大于等于pivot的值
            if(low >= max){
                break;
            }

            //交换
            temp = arr[low];
            arr[low] = arr[max];
            arr[max] = temp;

            //如果交换完后，发现这个arr[low] == pivot值 相等 则 max -- ，前移
            if(arr[low] == pivot){
                max -= 1;
            }

            //如果交换完后，发现这个arr[max] == pivot值 相等 则 low ++ ，后移
            if(arr[max] == pivot){
                low += 1;
            }

            //如果low == max,必须low ++ , max -- ,否则会出现堆栈溢出
            if(low == max){
                low += 1;
                max -= 1;
            }

            //向左递归
            if(left < max){
                quickSort(arr,left,max);
            }

            //向右递归
            if(right > low){
                quickSort(arr,low,right);
            }
        }
    }
}
```