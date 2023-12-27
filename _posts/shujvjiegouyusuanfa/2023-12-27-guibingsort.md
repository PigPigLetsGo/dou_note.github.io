---
layout: post
title:  归并排序
categories: [java,数据结构与算法,算法,排序]
description: 归并排序的详细介绍。
keywords: 算法
---

### 归并排序

**归并排序介绍**:

归并排序（MERGE-SORT）是利用归并的思想实现的排序方法，该算法采用经典的分治（divide-and-conquer）策略（分治法将问题分(divide)成一些小的问题然后递归求解，而治(conquer)的阶段则将分的阶段得到的各答案"修补"在一起，即分而治之)。

**归并排序思想示意图1-基本思想**:

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902386.png)

**说明**:

可以看到这种结构很像一棵完全二叉树，本文的归并排序我们采用递归去实现（也可采用迭代的方式去实现）。分阶段可以理解为就是递归拆分子序列的过程。

**归并排序思想示意图2-合并相邻有序子序列**:

再来看看治阶段，我们需要将两个已经有序的子序列合并成一个有序序列，比如上图中的最后一次合并，要将[4,5,7,8]和[1,2,3,6]两个已经有序的子序列，合并为最终序列[1,2,3,4,5,6,7,8]，来看下实现步骤

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902315.png)

代码实现：

```java
@SuppressWarnings("all")
public class GuibingSort {
    public static void main(String[] args) {
        int[] arr= {8,4,5,7,1,3,6,2,0};
        //归并排序需要一个额外空间
        int[] temp = new int[arr.length];
        mergeSort(arr,0,arr.length - 1,temp);
        System.out.println(Arrays.toString(arr));
    }

    /**
     * 分+合方法
     * @param arr
     * @param left
     * @param right
     * @param temp
     */
    public static void mergeSort(int[] arr,int left,int right,int[] temp){
        if(left < right){
            //中间索引
            int mid = (left + right) / 2;
            //向左递归进行分解
            mergeSort(arr,left,mid,temp);
            //向右递归进行分解
            mergeSort(arr,mid + 1,right,temp);
            //合并
            merge(arr,left,mid,right,temp);
        }
    }

    /**
     * 合并方法
     * @param arr 排序的原始数据
     * @param left 左边有序序列的初始索引
     * @param mid 中间索引
     * @param right 右边索引
     * @param temp 做中转的数组
     */
    public static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left;//初始化i，左边有序序列的初始索引
        int j = mid + 1;//初始化j，右边有序序列的初始索引
        int t = 0;//指向temp数组的当前索引

        /**
         * (一)
         * 先把左右两边(有序)的数据按照规则填充到temp数组
         * 直到左右两边的有序序列，有一边处理完毕为止
         */
        while (i <= mid && j <= right) {
            /**
             * 如果左边有序序列的当前元素，小于等于右边有序的当前元素
             * 即将左边的当前元素，拷贝到temp数组
             * 然后t ++ i ++
             */
            if (arr[i] <= arr[j]) {
                temp[t] = arr[i];
                t += 1;
                i += 1;
            }else{
                temp[t] = arr[j];
                j += 1;
                t += 1;
            }
        }

        /**
         * (二)
         * 把有剩余数据的一边的数据依次全部填充到temp
         */
        //左边的有序序列还有剩余的元素，就全部填充到temp
        while(i <= mid){
            temp[t] = arr[i];
            t += 1;
            i += 1;
        }
        //右边的有序序列还有剩余的元素，就全部填充到temp
        while(j <= right){
            temp[t] = arr[j];
            t += 1;
            j += 1;
        }

        /**
         * (三)
         * 将temp数组的元素拷贝到arr
         * 注意：并不是每次都拷贝所有的数据
         */
        t = 0;
        int tempLeft = left;
        /**
         * 第一次合并tempLeft = 0, right = 1
         * 第二次合并tempLeft = 2 right = 3
         * 第三次合并tempLeft = 0 right = 3
         * 最后一次tempLeft = 0 right = 7
         */
        while(tempLeft <= right){
            arr[tempLeft] = temp[t];
            t += 1;
            tempLeft += 1;
        }
    }
}
```