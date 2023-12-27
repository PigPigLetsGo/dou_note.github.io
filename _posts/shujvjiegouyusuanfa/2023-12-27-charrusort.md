---
layout: post
title:  插入排序
categories: [java,数据结构与算法]
description: 插入排序的详细介绍。
keywords: 算法
---

### 插入排序

**插入排序法介绍**：

插入式排序属于内部排序法，是对于预排序的元素以插入的方式找寻元素的适当位置，以达到排序的目的。

**插入排序法思想**:

插入排序（Insertion Sorting）的**基本思想**是：把n个待排序的元素看成为一个有序表和一个无序表，开始时有序表中只包含一个元素，无序表中包含有n-1个元素，排序过程中每次从无序表中取出第一个元素，把它的排序码依次与有序表元素的排序码进行比较，将它插入到有序表中的适当位置，使之成为新的有序表。

**插入排序思路图**：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161901111.png)

代码实现：

```java
public static void main(String[]args){
    int[] ar = {5,3,4,6,2,1};
    //需要使用j循环值来进行位置互换
    int j;
    for(int i = 1;i < ar.length;i ++){
        //取一个值进行比较
        int tmp = ar[i];
        //j = i - 1 循环0等于或到j的数值,j也就是i-1的值,判断是否满足交换条件执行则j-- 
        for(j = i-1;j >= 0 && ar[j] > tmp;j --){
            //将大的元素赋值到索引前
            ar[j+1] = ar[j];
        }
        //将元素小的赋值到索引后,因为 j-- j循环变量在满足条件执行方法体时不会及时执行 j--
        ar[j+1] = tmp;
    }
    System.out.println(Arrays.toString(ar));
}
```

-  优化 - 折半插入排序

```java
public static void main(String[]args){
    int[] arr = {5,3,4,6,2,1};
    int n = arr.length;
    int j,low,mid,high,temp;
    for(int i = 1;i < n;i ++){
        //循环的起始数据
        low = 0;
        //循环的循环数值
        high = i - 1;
        //取第i个元素赋值到tmp中进行比较
        temp = arr[i];
        /*找到合适的插入位置high+1，如果中间位置元素
         *比要插入元素大，则查找区域向低半区移动，否
         *则向高半区移动
         */
        //从前面的元素开始判断到temp的元素如果不满足则直接进行下一次循环
        while(low <= high){
        /*每次执行将low与长度相加的值除2得出mid指针位置*/
            mid = (low+high)/2;
            //判断mid的索引位置的值 是否 大于temp的值
            if(arr[mid] > temp){
                //满足交换条件,指针位置-1,缩短判断范围
                high = mid - 1;
            }else{
                //不满足交换条件 指针位置+1,缩短判断范围
                low = mid + 1;
            }
        }
        /*high+1后的元素后移*/
        //经过上面判断i-1为temp后一位的位置,从high循环到 i
        for(j = i - 1;j >= high + 1;j --) {
            arr[j + 1] = arr[j];
        }
        /*将元素插入到指定位置*/
        arr[j + 1] = temp;
    }
    System.out.println(Arrays.toString(arr));
}
```

-  优化 - 路插入排序

```java
int j, first, last, mid;
int len = arr.length;
/*临时数组*/
int[] tempArr =new int[len];
tempArr[0] = arr[0];
/*first和last分别指临时数组tempArr中排好序的元素的第一个和最后一个位置*/
first = last = 0;

for(int i = 1; i<len; i++){
   /*j 是调整系数*/
   if(first > last){
      j = len;        
   }else{
      j = 0;
   }
   /*tempArr中间元素的位置*/
   mid = ((first+last+j)/2)%len; 
   /*arr[i]应该插入在tempArr的前半部分*/
   if(arr[i] < tempArr[mid]){      
      /*j指向tempArr数组中的第一个元素*/
      j = first;
      /*first 前移，取余是为了实现循环数组效果*/
      first = (first-1+len)%len;  
      /*待插元素大于 j 所指元素*/
      while(arr[i] > tempArr[j]){    
         /*j 所指元素前移，取余是为了实现循环数组效果*/
         tempArr[(j-1+len)%len] = tempArr[j];  
         /*j 指向下一个元素*/
         j = j+1;
      }
      /*移动结束，待插元素插在tempArr[j]前*/
      tempArr[(j-1+len)%len] = arr[i];    
      /*arr[i]应该插入在tempArr的后半部分*/
   }else{
      /*j指向tempArr数组中的最后一个元素*/
      j = last;
      /*last后移， 指向插入后的最后一个元素*/
      last++;
      /*待插元素小于 j 所指元素*/
      while(arr[i] < tempArr[j]){  
         /*j 所指元素后移*/
         tempArr[(j+1)%len] = tempArr[j]; 
         /*j 指向上一个元素*/
         j = (j-1+len)%len;         
      }
      /*移动结束，待插元素插在tempArr[j]后*/
      tempArr[(j+1)%len] = arr[i]; 
   }
}

/*把在tempArr中排好序的元素依次赋给arr*/
for(int i = 0; i < len; i++){                    
   arr[i] = tempArr[(first+i)%len];
}
```
