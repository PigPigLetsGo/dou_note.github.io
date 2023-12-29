---
layout: post
title: 二分查找[详解]
categories: [java,数据结构与算法,算法,查找]
description: 二分查找？什么是左闭，什么是右闭，一文搞懂！
keywords: 算法
---

# 二分查找

[TOC]

先了解一下简单查找: 很简单就是一个一个的查找效率非常低, 打个比方都玩过的猜数字游戏,小明心里想着一个1到100之间其中一个数组让小李猜这个数字是多少,如果小李一个一个的从1开始猜到100那么显然是很慢很笨拙的,我们再来说说二分查找,回到主题就是将问题拆分为半,让小李同学不再一个一个的去试

>  在二分查找中,目标元素的查找区间的定义十分重要,不同的区间的定义写法不一样

因为查找的区间是不断迭代的,所以确定查找的范围十分重要,主要就是左右区间的开和闭的问题,对应的迭代方式也不一样有以下两种方式:

-  左闭右闭[left,right]
-  左闭右开[left,right]

### 例子🌰

<u>这是一个使用二分查找的例题</u>

题目如下:

>  给定一个人n个元素有序的(升序)整形数组nums和一个目标值target,写一个函数搜索nums中的target,如果目标值存在返回下标,不存在返回-1

[可参考](https://leetcode.cn/problems/binary-search/)	Leetcode中的二分查找题

示例一:

>  输入:nums = [-1,0,3,5,9,12],target = 9
>
>  输出: 4
>
>  解释: 9出现在nums中并且下标为4

示例二:

>  输入:nums = [-1,0,3,5,9,12],target = 2
>
>  输出: -1
>
>  解释: 2不存在nums中因此返回-1

---
==提示==:

-  你可以假设nums中的所有元素是不重复的
-  n将在[1-10000]之间
-  nums的每个元素都将在[-9999,9999]之间

二分法的思想很简单,因为整个数组是有序的,数组默认是递增的

-  首先选择数组中间的数字和需要查找的目标值比较

-  如果相等最好,就可以直接返回答案了

-  如果不相等

   -  `int mid = left + (right + left) / 2;`取中间位置为起始位置

   -  如果==中间的数字大于目标值==,则**中间数字向右**的**所有数字都大于目标值**,**全部排除**

      `target < nums[mid]`

      `left = mid + 1;`

   -  如果==中间的数字小于目标值==,则**中间数字向左**的**所有数字都小于目标值**,**全部排除**

      `target > nums[mid]`

      `right = mid - 1;`

>  tips:不用去纠结数组的长度是奇数或偶数的时候,怎么取长度的一半,一下说明,可以跳过

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424160141149.png)

因为29大于中间的数字大于11,所以左边的所有数字全部排除

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424160241753.png)

如果数组长度为奇数

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424160301212.png)

不要一直纠结中间的数字两边的数字数量不一样这个问题,因为:

-  两边数量不一样是一定会出现的情况
-  但是这种情况并不影响我们对中间数字和目标值数字大小关系的判断
   -  只要中间数字大于目标数字,就排除右边
   -  只要中间数字小于目标数字,就排除左边

所以数组长度是偶数还是奇数这个真的不重要,不影响怎么排除的问题,无非是多排除一个数字或者少排除一个数字

-  真正影响的是中间那个数字到底该不该加入下一次的查找中,也就是==边界问题==

### 正向写法

```java
int search(int nums[], int size, int target) //nums是数组，size是数组的大小，target是需要查找的值
{
    int left = 0;
    int right = size - 1;	// 定义了target在左闭右闭的区间内，[left, right]
    while (left <= right) {	//当left == right时，区间[left, right]仍然有效
        int middle = left + ((right - left) / 2);//等同于 (left + right) / 2，防止溢出
        if (nums[middle] > target) {
            right = middle - 1;	//target在左区间，所以[left, middle - 1]
        } else if (nums[middle] < target) {
            left = middle + 1;	//target在右区间，所以[middle + 1, right]
        } else {	//既不在左边，也不在右边，那就是找到答案了
            return middle;
        }
    }
    //没有找到目标值
    return -1;
}
```



![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424160758343.png)

-  首先,对left的值和right的值进行初始化,然后计算middle的值
   -  `left = 0,right = size - 1;`
   -   `middle = (left + (right - left) / 2);`

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424160823492.png)

-  比较nums[middle]的值和target的值大小关系
   -  `if(nums[middle] > target)`,代表middle向右所有的数字大于target
   -  `if(nums[middle] < target)`,代表middle向左所有的数字小于target
   -  既不大于也不小于就是找到了相等的值

-  `nums[middle] = 13 < target = 33,left = middle + 1;`

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/2021042416084724.png)

-  循环条件为`while(left <= right)`
-  此时,`left = 6 <= tight ==11;`,则继续进行循环
-  当前`middle = left + ((right - left) / 2);`计算出middle的值

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424160925843.png)

-  计算出middle的值后,比较nums[middle]和target的值,发现:
   -  nums[middle] == 33 == target == 33,找到目标值,结束并返回结果

###   反向写法(也就是循环长度不够从未达不到要找到目标值的要求)

对应第一种正向的写法,我们把循环条件修改看看会发生什么事

-  原查找区间[left,right]
-  原循环条件是`while(left <= right)`

修改后题目对应的条件:

-  查找区间不变,仍然是[left,right]
-  查找数字为27(target = 27)
-  ==循环条件修改为`while(left < right)`==

```java
int search(int nums[], int size, int target) 
{
    int left = 0;
    int right = size - 1;	
    while (left < right) {	//left <= right 修改为 left < right,其他不变
        int middle = left + ((right - left) / 2);
        if (nums[middle] > target) {
            right = middle - 1;
        } else if (nums[middle] < target) {
            left = middle + 1;
        } else {	
            return middle;
        }
    }
    //没有找到目标值
    return -1;
}
```

图片模拟过程:

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161148136.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161214479.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161238873.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161339460.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161355107.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161422235.png)

-  因为nums[middle] = 22 < target = 27,此时left = middle + 1,此时left = tight,而循环条件为while(left < right),所以还未找到27的情况下算法就跳出了循环,返回-1

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161440637.png)

### 第二种写法(左闭右开)

#### 正向写法

第二种写法:每次查找的区间在[left,right],(左闭右开区间),根据区间的定义,条件控制应该如下:

-  循环条件使用`while(left < right)`
-  `if(nums[middle] > target), right = minddle`因为当前的nums[middle]是大于target的,不符合条件,不能取到middle,并且区间的定义是[left,right],刚好区间上的定义就去不到right,所以right赋值为middle

```java
int search(int nums[], int size, int target)
{
	int left = 0;
	int right = size; //定义target在左闭右开的区间里，即[left, right)
	while (left < right) {	//因为left = right的时候，在[left, right)区间上无意义
		int middle = left + ((right - left) / 2);
		if (nums[middle] > target) {
			right = middle; //target 在左区间，在[left, middle)中 
		} else if (nums[middle] < target) {
			left = middle + 1;
		} else {
			return middle;
		}
	} 
    // 没找到就返回-1
	return -1;
}
```

图片演示过程

-  需要查找的值为3

第一步是初始化left和right的值,然后计算middle的值

-  left = 0,right = size
-  循环条件 `while(left < right)`



![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161720534.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/2021042416174072.png)

-  比较nums[middle]和target的大小,因为nums[middle] = 22 > target = 3
-  所以right = middle

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161805761.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161805761-1665451818972-35.png)

-  比较nums[middle]和target的大小, 因为nums[middle] = 9 > target = 3
-  所以right = middle

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161836963.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161848640.png)

-  比较nums[middle]和target的大小关系,因为nums[middle] = 0 < target = 3
-  所以left = middle + 1

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424161919407.png)

-  比较nums[middle]和target的关系,nums[middle] = 3 == target = 3
-  成功找到target

#### 反向写法(错误演示)

对应第二种正确的写法,照样把循环条件修改,看会发生什么事

正确的写法中条件为:

-  查找原区间[left,right]
-  循环条件为`while(left < right)`

修改后题目对应的条件:

-  查找区间不变,仍然是[left,right]
-  循环条件修改为:`while(left <= right)`
-  查找的数字为26(==数组中不存在这个数字==)

```java
int search(int nums[], int size, int target)
{
	int left = 0;
	int right = size; 
	while (left <= right) {	//条件left < right 修改为 left <= right
		int middle = left + ((right - left) / 2);
		if (nums[middle] > target) {
			right = middle; 
		} else if (nums[middle] < target) {
			left = middle + 1;
		} else {
			return middle;
		}
	} 
    // 没找到就返回-1
	return -1;
}
```

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162039656.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162054302.png)

-  判断nums[middle]和target的大小关系,nums[middle] = 22 < target = 26
-  left = middle + 1(其实这里nums[left]已经等于27,26不可能找到,接下去就看算法是否能够知道数组中不存在26并且返回-1了)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162108630.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162119840.png)

-  判断nums[middle]和target的大小关系:nums[middle] = 57 > target = 26
-  right = middle

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162137619.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162154499.png)

-  比较nums[middle]和target的大小关系:nums[middle] = 33 > target = 26
-  right = middle

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162207609.png)

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162223901.png)

-  比较nums[middle]和target大小关系,因为nums[middle] = 27 > target = 26
-  所以right = middle,自此left和right相遇,但是循环条件被我们修改成`while(left <= right)`会接着做循环

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162235639.png)

-  接下来就是死循环
-  因为middle = left + ((right - left) / 2),当left = right的时候,middle的值不会继续改变
-  middle不继续改变,由于right = middle,right也不会改变,所以三个数字自此开始不会继续改变
-  循环条件`while(left <= right)`却仍然满足,不会跳出循环
-  死循环....

## 总结

二分最重要的两个点,就是==循环条件==和==后续的区间赋值问题==

![img](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/20210424162256792.png)

因为两者是互相联系,互相影响的,所以就需要两者统一,如果两者不统一,就会出现问题,所以循环条件和赋值问题必须统一,也就是循环不变量
