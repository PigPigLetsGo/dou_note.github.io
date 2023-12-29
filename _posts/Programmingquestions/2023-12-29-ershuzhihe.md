---
layout: post
title: 两数之和
categories: [java,算法,编程题]
description: 两数之和的题解(梦开始的地方)
keywords: 编程题
---

# 两数之和

==难度==：<font style="color:green">简单</font> 

给定一个整数数组`nums`和一个整数目标值`target`，请你在该数组中找出 **和为目标值** `target`的那 **两个** 整数，并返回它们的下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

>  示例1：
>
>  ```text
>  输入：nums = [2,7,11,15], target = 9
>  输出：[0,1]
>  解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
>  ```
>
>  示例2：
>
>  ```text
>  输入：nums = [3,2,4], target = 6
>  输出：[1,2]
>  ```
>
>  示例3：
>
>  ```text
>  输入：nums = [3,3], target = 6
>  输出：[0,1]
>  ```
>
>  <font style="color:green">提示</font>：
>
>  -  2 <= nums.length <= 104
>  -  109 <= nums[i] <= 109
>  -  109 <= target <= 109
>     只会存在一个有效答案

---

**方法**：哈希表

思路及算法

1.  定义Map集合，key：存储数组下标的值，value：存储循环次数也就是数组当前下标
2.  遍历数组，判断map集合中存储的key是否有`target`^最大值^ - 当前数组下标元素的值，如果有则返回新数组{(`target` - nums[i]),i}，最大值减当前数组下标值与当前循环次数^数组下标^ 相当于最大值 - 当前下标的值就是最终答案的值
3.  如果找不到则返回null

```java
public int[] twoSum(int[] nums, int target) {
   Map<Integer,Integer> map = new HashMap<>();
   for(int i = 0;i < nums.length;i ++){
      if(map.containsKey(target - nums[i])){
         return new int[]{map.get(target - nums[i]),i};
      }
      map.put(nums[i],i);
   }
   return null;
}
```

**方法**：暴力穷举

思路及算法

1.  使用双重for循环内循环if判断前一个值i与后一个值j相加的结果是否等于`target`^最大值^
2.  如果两数相加等于`target`则执行返回新建数组中i与j的值^数组下标值^
3.  如果始终没有满足的条件则返回新建数组的0下标也就是没有值

```java
public int[] twoSum(int[] nums, int target) {
   for(int i = 0;i < nums.length;i ++){
      for(int j = i + 1;j < nums.length;j ++){
         if(nums[i] + nums[j] == target){
            return new int[]{i,j};
         }
      }
   }
   return new int[0];
}
```

