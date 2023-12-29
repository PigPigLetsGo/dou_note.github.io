---
layout: post
title: 线性查找
categories: [java,数据结构与算法,算法,查找]
description: 线性查找的介绍
keywords: 算法
---

### 线性查找算法

<font style="color:red">要求数组是一个有序的数组</font>.

有一个数列： {1,8, 10, 89, 1000, 1234} ，判断数列中是否包含此名称【顺序查找】  要求: 如果找到了，就提示找到，并给出下标值。  

思路：如果查找到全部符合条件的值。[思路分析.]

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902359.png)

```java
public static void main(String[] args)
    {
        int arr[] = {1, 2, 3, 4, 5, 6};
        int i = lineSearch(arr, 5);
        System.out.println(i);
    }
    private static int lineSearch(int arr[], int value)
    {
        for(int i = 0; i < arr.length; i++)
        {
            if(arr[i] == value)
            {
                return i;
            }
        }
        return -1;
    }
```

打印结果：

```
4
```
