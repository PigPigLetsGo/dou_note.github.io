---
layout: post
title:  打家劫舍
categories: [java,数据结构与算法,编程题]
description: 打家劫舍。
keywords: 编程题
---

贪心算法
- 单独交易日: 设今天价格p1,明天价格p2,则今天买入,明天卖出可赚取金额p2-p1(负值代表亏损)
- 连续上涨交易日: 设此上涨交易日股票价格分别为p1,p2,....pn,则第一天买最后一天卖收益最大,即pn-p1等价于每天都买卖,即pn-p1=(p2-p1)+(p3-p2)+....+(pn-pn-1)
- 连续下降交易日: 则不买卖收益最大,既不会亏钱
算法流程: 
- 遍历整个股票交易日价格列表price,策略是所有上涨交易日都买卖,(赚到所有利润).所有下降交易日都不买卖(永不亏钱)
1.设tmp为第i-1日买入与第i日卖出赚取的利润,即tmp=prices[i] - prices[i-1]
2.当该天利润为正tmp>0.则将利润加入总利润profit,当利润为0或为负,则直接跳过
3.遍历完成后,返回总利润profit
- 复杂度分析:
- 时间复杂度O(n):只需遍历一次price
- 空间复杂度O(1):变量使用常数额外空间

```java
 public int maxProfit(int[] prices) {
        int len = prices.length;
        int max = 0;
        for(int i = 1;i < len;i ++){
            max += Math.max(0,prices[i] - prices[i-1]);
        }
        return max;
    }
```