---
layout: post
title: 插值查找算法
categories: [java,数据结构与算法,算法,查找]
description: 插值查找算法的介绍
keywords: 算法
---

### 插值查找算法

**插值查找原理介绍**:

1. 插值查找算法类似于二分查找，不同的是插值查找每次从自适应mid处开始查找。
2. 将折半查找中的求mid 索引的公式 , low 表示左边索引left, high表示右边索引right.  key 就是前面我们讲的 findVal
![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902056.png)
3. int mid = low + (high - low) `*` (key - arr[low]) / (arr[high] -arr[low]) ;/*插值索引*/  
对应前面的代码公式：  
int mid = left + (right – left) * (findVal – arr[left]) / (arr[right] – arr[left])  


4. 举例说明插值查找算法 1-100 的数组

代码实现：

```java
@SuppressWarnings("all")
public class InsertValueSearch {
    public static void main(String[] args) {
        int[] arr = new int[100];
        for(int i = 1;i <= 100;i ++){
            arr[(i - 1)] += i;
        }
        System.out.println(insertValuesearch(arr,0,arr.length - 1,100));
    }

    /**
     * @param arr 数组
     * @param left 左边索引
     * @param right 右边索引
     * @param value 查找值
     * @return 如果找到，返回对应下标，没有返回 -1
     */
    public static int insertValuesearch(int[] arr,int left,int right,int value){
        /**
         * 注意：value < arr[0] 和 value > arr[arr.length - 1] 必须需要
         * 否则我们得到的mid可能越界
         * arr.length - 1 < right 判断传入的right值是否大于了传入数组的长度
         */
        if(left > right || value < arr[0] || value > arr[arr.length - 1] || arr.length - 1 < right){
            return -1;
        }

        //求mid的值 称为自适应写法
        int mid = left + (right - left) * (value - arr[left]) / (arr[right] - arr[left]);

        int midVal = arr[mid];

        if (value > midVal) {
            //向右边递归
            return insertValuesearch(arr,mid + 1,right,value);
        }else if(value < midVal) {
            //向左边递归
            return insertValuesearch(arr,left,mid - 1,value);
        }else{
            return mid;
        }
    }
}
```

<font style="color:red">插入查找注意事项</font>：

1.  对于数据量较大，**关键字分布比较均匀**的查找表来说，采用**插值查找**,**速度较快**.
2.  关键字分布不均匀的情况下，该方法不一定比折半查找要好
