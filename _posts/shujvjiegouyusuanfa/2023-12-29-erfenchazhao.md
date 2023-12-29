---
layout: post
title: 二分查找
categories: [java,数据结构与算法,算法,查找]
description: 二分查找的介绍以及递归和非递归实现
keywords: 算法
---

### 二分查找算法

**二分查找**：

<font style="color:red">要求数组是一个有序的数组</font>.

请对一个有序数组进行二分查找 {1,8, 10, 89, 1000, 1234} ，输入一个数看看该数组是否存在此数，并且求出下标，如果没有就提示"没有这个数"。  

课后思考题： {1,8, 10, 89, 1000, 1000，1234} 当一个有序数组中，有多个相同的数值时，如何将所有的数值都查找到，比如这里的 1000.

代码实现：

```java
public class Binary_search {
    //二分查找
    public static void main(String[]args){
        int[] ar = {1,3,5,7,8,9};
        int x = run(ar,8);
        System.out.println(x);
    }
    public static int run(int[] nums,int trans){
        int mid = 0;
        int first = 0;
        int last = nums.length - 1;
        //判断 目标值是否小于索引0的值,目标值是否大于索引长度的值,长度
        if(trans < nums[first] || trans > nums[last] || first > last)
            return -1;
        //判断循环
        while(first <= last){
            //将长度除2的值赋值给mid,起始位置,访问数组中间的数据
            mid = (first + last) / 2;
            //判断数字是否小了
            if(nums[mid] > trans)
                //如果小了则让last - 1然后再次除2判断数字的大小
                last = mid - 1;
            //else只执行一个结果
            else
                //判断数字是否大了
            if(nums[mid] < trans)
                //如果数字大了则first + 1然后再次除2判断数字的大了,每次都是一半一半的进行试探的判断
                first = mid + 1;
            //else只执行一个结果
            else
                //如果两个判断不满足则执行返回值当前的索引位置就是目标值
            return mid;
        }
        //如果全程没有找到则返回 -1
        return -1;
    }
}
```

递归实现：

```java
@SuppressWarnings("all")
public class Chazhao01 {
    public static void main(String[] args) {
        int[] arr = {1,8, 10, 89, 1000, 1000,1234};
        int i = binarySearch(arr, 0, arr.length - 1, 89);
        System.out.println(i);
        List<Integer> i1 = binarySearch1(arr, 0, arr.length - 1, 1000);
        System.out.println("多个值的下标："+i1);
    }

    /**
     * @param arr 数组
     * @param left 左边的索引
     * @param right 右边的索引
     * @param value 要查找的值
     * @return 如果找到就返回下标，否则返回-1
     */
    public static int binarySearch(int[] arr,int left,int right,int value){
        if(left > right){
            return - 1;
        }

        int mid = (left + right) / 2;
        int midValue = arr[mid];
        if(value > midValue){//向右递归
            return binarySearch(arr,mid + 1,right,value);
        }else if(value < midValue){//向左递归
            return binarySearch(arr,left,mid - 1,value);
        }else {
            return mid;
        }
    }

    public static List<Integer> binarySearch1(int[] arr,int left,int right,int value){
        if(left > right){
            return new ArrayList<>();
        }

        int mid = (left + right) / 2;
        int midValue = arr[mid];
        if(value > midValue){//向右递归
            return binarySearch1(arr,mid + 1,right,value);
        }else if(value < midValue){//向左递归
            return binarySearch1(arr,left,mid - 1,value);
        }else{
            /**
             * 思路分析
             * 1.在找到mid索引值，不要马上返回
             * 2.向mid索引值的左边扫描，将所有满足value的元素下标加入集合中
             * 3.向mid索引值的右边扫描，将所有满足value的元素下标加入集合中
             * 4.将集合返回
             */
            List<Integer> list = new ArrayList<>();
            //向mid索引值的左边扫描，将所有满足value的元素的下标加入集合中
            int temp = mid - 1;
            while(true){
                if(temp < 0 || arr[temp] != value){//结束
                    break;
                }
                //否则，将temp放入到集合中
                list.add(temp);
                //temp左移
                temp -= 1;
            }
            list.add(mid);
            temp = mid + 1;
            while(true){
                if(temp > arr.length - 1 || arr[temp] != value){
                    break;
                }
                list.add(temp);
                temp += 1;
            }
            return list;
        }
    }
}
```
