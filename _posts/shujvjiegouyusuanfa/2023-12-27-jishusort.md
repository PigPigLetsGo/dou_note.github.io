---
layout: post
title:  基数排序
categories: [java,数据结构与算法,排序]
description: 基数排序的详细介绍。
keywords: 算法
---

### 基数排序

**基数排序(桶排序)介绍**:

1. [基](https://baike.baidu.com/item/%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F/7875498)[数排序](https://baike.baidu.com/item/%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F/7875498)（radix sort）属于“分配式排序”（distribution sort），又称“桶子法”（bucket sort）或bin sort，顾名思义，它是通过键值的各个位的值，将要排序的[元素分配](https://baike.baidu.com/item/%E5%85%83%E7%B4%A0%E5%88%86%E9%85%8D/2107419)至某些“桶”中，达到排序的作用

2. 基数排序法是属于稳定性的排序，基数排序法的是效率高的稳定性排序法

3. 基数排序(Radix Sort)是[桶排序](http://www.cnblogs.com/skywang12345/p/3602737.html)的扩展

4. 基数排序是1887年赫尔曼·何乐礼发明的。它是这样实现的：将整数按位数切割成不同的数字，然后按每个位数分别比较。

**基数排序基本思想** 

1. 将所有待比较数值统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后, 数列就变成一个有序序列。

2. 这样说明，比较难理解，下面我们看一个图文解释，理解基数排序的步骤

**基数排序图文说明** 

将数组 {53, 3, 542, 748, 14, 214} 使用基数排序, 进行升序排序。

第1轮排序 [按照个位排序]:

说明： 事先准备10个数组(10个桶)， 0-9 分别对应 位数的 0-9

1. 将 各个数，按照个位大小 放入到 对应的 各个数组中
2. 然后从 0-9 个数组/桶，依次，按照加入元素的先后顺序取出

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902994.png)

示意图：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902405.png)

分解代码：

```java
@SuppressWarnings("all")
public class JichuSort {
    public static void main(String[] args) {
        int[] arr = {53,3,542,748,14,214};
        radixSort(arr);
    }

    //基数排序方法
    public static void radixSort(int[] arr){

        /**
         * 第一轮（针对每个元素的个位进行排序处理）
         * 定义一个二维数组，表示10个桶，每个桶就是一个一维数组
         * 说明：
         * 1.二维数组包含10个一维数组
         * 2.为了防止在放入数值的时候，数据溢出，则每个一维数组（桶），大小定义为arr.length
         * 3.名明确,基础排序是使用空间换时间的经典算法
         */
        int[][] bucket = new int[10][arr.length];

        /**
         * 为了记录每个桶中，实际存放了多少个数据，我们定义一个一维数组来记录各个桶的每次放入的数据个数
         * 可以这么理解:
         * 比如：bucketElementCounts[0],记录的就是bucket[0]桶的放入数据个数
         */
        int[] bucketElementCounts = new int[10];

        //第一轮（针对每个元素的个位进行排序处理）
        for(int j = 0;j < arr.length;j ++){
            //取出每个元素的个位的值
            int digitOfElement = arr[j] % 10;
            //放入到对应的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement] ++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来数组）
        int index = 0;
        //遍历每一桶，并将桶中的数据，放入到原数组
        for(int k = 0;k < bucketElementCounts.length;k ++){
            //如果桶中，有数据，我们才放入原数组
            if(bucketElementCounts[k] != 0){
                //循环该桶即第k个桶（即第k个一维数组），放入
                for(int l = 0;l < bucketElementCounts[k];l ++){
                    //取出元素放入到arr
                    arr[index ++] = bucket[k][l];
                }
            }
            bucketElementCounts[k] = 0;
        }
        System.out.println("第一轮排序结果："+Arrays.toString(arr));
        //======================================
        //第二轮(针对十位进行排序)
        for(int j = 0;j < arr.length;j ++){
            //取出每个元素的十位的值
            int digitOfElement = arr[j] / 10 % 10;
            //放入到对应的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement] ++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来数组）
        index = 0;
        //遍历每一桶，并将桶中的数据，放入到原数组
        for(int k = 0;k < bucketElementCounts.length;k ++){
            //如果桶中，有数据，我们才放入原数组
            if(bucketElementCounts[k] != 0){
                //循环该桶即第k个桶（即第k个一维数组），放入
                for(int l = 0;l < bucketElementCounts[k];l ++){
                    //取出元素放入到arr
                    arr[index ++] = bucket[k][l];
                }
            }
            bucketElementCounts[k] = 0;
        }
        System.out.println("第二轮排序结果："+Arrays.toString(arr));
        //==========================================
        //第三轮(针对百位进行排序)
        for(int j = 0;j < arr.length;j ++){
            //取出每个元素的百位的值
            int digitOfElement = arr[j] / 100 % 10;
            //放入到对应的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement] ++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来数组）
        index = 0;
        //遍历每一桶，并将桶中的数据，放入到原数组
        for(int k = 0;k < bucketElementCounts.length;k ++){
            //如果桶中，有数据，我们才放入原数组
            if(bucketElementCounts[k] != 0){
                //循环该桶即第k个桶（即第k个一维数组），放入
                for(int l = 0;l < bucketElementCounts[k];l ++){
                    //取出元素放入到arr
                    arr[index ++] = bucket[k][l];
                }
            }
        }
        System.out.println("第三轮排序结果："+Arrays.toString(arr));
    }
}
```

规律：每轮求数值的位数值一直在改变其它不变

基数排序实现：

```java
@SuppressWarnings("all")
public class JichuSort {
    public static void main(String[] args) {
        int[] arr = {53, 3, 542, 748, 14, 214};
        radixSort(arr);
    }

    //基数排序方法
    public static void radixSort(int[] arr) {
        //根据前面的推到过程，我们可以得到最终的基数排序代码
        //1.得到数组中最大的数的位数
        int max = arr[0];//假设第一个元素就是最大为数
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        //得到最大数是几位数
        int maxLength = (max + "").length();

        /**
         * 第一轮（针对每个元素的个位进行排序处理）
         * 定义一个二维数组，表示10个桶，每个桶就是一个一维数组
         * 说明：
         * 1.二维数组包含10个一维数组
         * 2.为了防止在放入数值的时候，数据溢出，则每个一维数组（桶），大小定义为arr.length
         * 3.名明确,基础排序是使用空间换时间的经典算法
         */
        int[][] bucket = new int[10][arr.length];

        /**
         * 为了记录每个桶中，实际存放了多少个数据，我们定义一个一维数组来记录各个桶的每次放入的数据个数
         * 可以这么理解:
         * 比如：bucketElementCounts[0],记录的就是bucket[0]桶的放入数据个数
         */
        int[] bucketElementCounts = new int[10];

        for (int i = 0, n = 1; i < maxLength; i++, n *= 10) {

            //第一轮（针对每个元素的个位进行排序处理）
            for (int j = 0; j < arr.length; j++) {
                //取出每个元素的个位的值
                int digitOfElement = arr[j] / n % 10;
                //放入到对应的桶中
                bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
                bucketElementCounts[digitOfElement]++;
            }
            //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来数组）
            int index = 0;
            //遍历每一桶，并将桶中的数据，放入到原数组
            for (int k = 0; k < bucketElementCounts.length; k++) {
                //如果桶中，有数据，我们才放入原数组
                if (bucketElementCounts[k] != 0) {
                    //循环该桶即第k个桶（即第k个一维数组），放入
                    for (int l = 0; l < bucketElementCounts[k]; l++) {
                        //取出元素放入到arr
                        arr[index++] = bucket[k][l];
                    }
                }
                bucketElementCounts[k] = 0;
            }
            System.out.println("第"+i+"轮排序结果：" + Arrays.toString(arr));
        }
    }
}
```