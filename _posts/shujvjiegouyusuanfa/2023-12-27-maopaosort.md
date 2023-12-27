---
layout: post
title:  冒泡排序
categories: [java,数据结构与算法,算法,排序]
description: 冒泡排序的详细介绍。
keywords: 冒泡排序
---

### 冒泡排序

基本介绍

冒泡排序（Bubble Sorting）的基本思想是：通过对待

排序序列从前向后（从下标较小的元素开始）,依次比较

相邻元素的值，若发现逆序则交换，使值较大

的元素逐渐从前移向后部，就象水底下的气泡一样逐渐

向上冒。

因为排序的过程中，各元素不断接近自己的位置，如果一趟比较下

来没有进行过交换，就说明序列有序，因此要在排序过程中设置

一个标志flag判断元素是否进行过交换。从而减少不必要的比较。(这里说的优化，可以在冒泡排序写好后，在进行)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900791.png)

演示冒泡过程的例子(图解)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900022.png)

冒泡排序应用实例

我们举一个具体的案例来说明冒泡法。我们将五个无序的数：3, 9, -1, 10, -2  使用冒泡排序法将其排成一个从小到大的有序数列。

图解：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161900920.png)

代码实现：

先将排序的结构拆分了写，步骤如下：

每次进行排序的时候都需要将长度 - 1 - n 因为排序后范围总是要缩小的否则就会一直来回的排序。

```java
public class BubbleSort01 {
    public static void main(String[] args) {
        int[] arr = {-1,-2,3,1,20,10,9,5};
        int temp = 0;
        for(int i = 0;i < arr.length - 1;i ++){
            if(arr[i] > arr[i+1]){
                temp = arr[i];
                arr[i] = arr[i+1];
                arr[i+1] = temp;
            }
        }
        System.out.println("第一趟排序结果："+ Arrays.toString(arr));
        for(int i = 0;i < arr.length - 1 - 1;i ++){
            if(arr[i] > arr[i+1]){
                temp = arr[i];
                arr[i] = arr[i+1];
                arr[i+1] = temp;
            }
        }
        System.out.println("第二趟排序结果："+Arrays.toString(arr));
        for(int i = 0;i < arr.length - 1 - 2;i ++){
            if(arr[i] > arr[i+1]){
                temp = arr[i];
                arr[i] = arr[i+1];
                arr[i+1] = temp;
            }
        }
        System.out.println("第三趟排序结果："+Arrays.toString(arr));
    }
}
```

简化代码：将排序的过程结合为嵌套循环来完成冒泡排序：

代码如下：

```java
@SuppressWarnings("all")
public class BubbleSort {
    public static void main(String[] args) {
        int temp = 0;
        int[] arr = {-1,-2,3,1,10,9,5};
        //标识变量
        boolean flag = false;
        //冒泡排序 的时间复杂度为 O(n^2)
        for(int i = 0;i < arr.length - 1;i ++){
            for(int j = 0;j < arr.length - i - 1;j ++){
                if(arr[j] > arr[j+1]){
                    //如果进行过交换则将标识变量赋值为true
                    flag = true;
                    temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
            System.out.println("第"+i+"趟，的排序结果为: "+Arrays.toString(arr));
            //判断标识变量进行对应的操作
            if(!flag){
                //在一趟排序中，一次交换都没有发生过
                break;
            }else{
                //如果发生过交换则进入else中，将flag重置进行下次判断
                flag = false;
            }
        }
    }
}
```
