---
layout: post
title:  整数转罗马数字
categories: [java,算法,编程题]
description: 整数转罗马数字介绍。
keywords: 算法
---



# 整数转罗马数字

**题目**：

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

-  `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
-  `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
-  `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给你一个整数，将其转为罗马数字。

**示例**：

**示例 1:**

```
输入: num = 3
输出: "III"
```

**示例 2:**

```
输入: num = 4
输出: "IV"
```

**示例 3:**

```
输入: num = 9
输出: "IX"
```

**示例 4:**

```
输入: num = 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
```

**示例 5:**

```
输入: num = 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

**提示：**

-  `1 <= num <= 3999`

代码：

```java
public class Demo03
{
    // 根据规矩将数组中的数值与符号进行映射关系
    int values[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    String symbols[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    /**
     * 规则如下：
     * L + L + XL = 50 + 50 + 40 = 140
     * C + X + X + X + X = 100 + 10 + 10 + 10 + 10 = 140
     * C + XL = 100 + 40 = 140
     * XC + L = 90 + 50 = 140
     * XL + XL + XL + X + X = 40 + 40 + 40 + 10 + 10 = 140
     * C + X + X + V + V + V + V = 100 + 10 + 10 + 5 + 5 + 5 + 5 = 140
     */
    public static void main(String[] args)
    {
        System.out.println(new Demo03().intToRoman(3));
    }

    public String intToRoman(int num) {
        // 创建字符缓冲区 用于存储最终结果
        StringBuffer roman = new StringBuffer();
        for (int i = 0; i < values.length; i++)
        {
            // 循环values数组的元素
            int value = values[i];
            // 循环symbols数组的元素
            String symbol = symbols[i];
            // 判断传入的值是否大于等于当前数组循环的元素值
            while(num >= value)
            {
                // 减去当前循环的数组的值
                num -= value;
                // 将当前数组的值对应的符号追加到字符缓冲区中
                roman.append(symbol);
            }
            // 判断num是否被减为0了，如果为0就跳出循环
            if(num == 0)
            {
                break;
            }
        }
        // 返回最终结果
        return roman.toString();
    }
}
```

