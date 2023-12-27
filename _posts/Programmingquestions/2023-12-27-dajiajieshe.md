---
layout: post
title:  打家劫舍
categories: [java,算法,编程题]
description: 打家劫舍。
keywords: 编程题
---

## 打家劫舍

题目要求：

>  你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。
>
>  给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

题目链接：[打家劫舍](https://leetcode.cn/problems/house-robber/description/?envType=daily-question&envId=2023-09-16) 

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

**提示：** 

-  `1 <= nums.length <= 100`
-  `0 <= nums[i] <= 400`

答案：

```java
class Solution {
    public int rob(int[] nums) {
        int len = nums.length;
        if(nums.length == 0 || nums == null) {
            return 0;
        }
        if(nums.length == 1) {
            return nums[0];
        }
        int newNums[] = new int[len];
        newNums[0] = nums[0];
        newNums[1] = Math.max(newNums[0], nums[1]);
        for(int i = 2;i < len;i ++) {
            newNums[i] = Math.max(newNums[i - 1], newNums[i - 2] + nums[i]);
        }
        return newNums[len - 1];
    }
}
```



```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    const len = nums.length
    if(nums.length === 0 || nums === 'undefined') {
        return 0
    }
    if(nums.length === 1) {
        return nums[0]
    }
    let newVar = []
    newVar[0] = nums[0]
    newVar[1] = Math.max(newVar[0], nums[1])
    for(let i = 2;i < len;i ++) {
        newVar[i] = Math.max(newVar[i - 1], newVar[i - 2] + nums[i])
    }
    return newVar[newVar.length - 1]
};
```

<span alt='solid'>思路</span>：

第1间房屋：S~0~ = H~0~ = 1

第2间房屋：S~1~ = max(S~0~,H~1~) = 2

第3间房屋：S~2~ = max(S~1~,S~0~+H~2~) = 4

第4间房屋：s~3~ = max(S~2~,S~1~+H~3~) = 4

>  递推公式：S~n~ = max(S~n-1~,S~n-2~+H~n~)

偷窃前n-1间房屋的最高金额

或偷窃前n-2间房屋的最高金额加第n间房屋的金额

![1](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161947745.png)

![image-20230916195041709](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161950004.png)

<span alt='solid'>复杂度分析</span>：

-  时间复杂度：O(N)
-  空间复杂度：O(N)

可以进行优化：

使用两个变量，frist和last 来记录 前一个房屋和前两个房屋之间的最大利益。省去一个数组的空间复杂度

优化：空间复杂度：O(1)

```java
class Solution {
    public int rob(int[] nums) {
        int len = nums.length;
        if(nums.length == 0 || nums == null) {
            return 0;
        }
        if(nums.length == 1) {
            return nums[0];
        }
        int first = nums[0];
        int last = Math.max(first, nums[1]);
        for(int i = 2;i < len;i ++) {
            int temp = last;
            last = Math.max(last, first + nums[i]);
            first = temp;
        }
        return last;
    }
}
```

