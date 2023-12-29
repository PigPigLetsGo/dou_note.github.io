---
layout: post
title: 贪心算法
categories: [java,程序员常用算法]
description: 程序员常用算法-贪心算法
keywords: 算法
---

### 贪心算法

#### 应用场景——集合覆盖问题

假设存在下面需要付费的广播台，以及广播台信号可以覆盖的地区。

**如何选择最少的广播台**，让所有的地区都可以接收到信号？

| 广播台 | 覆盖地区               |
| ------ | ---------------------- |
| K1     | "北京", "上海", "天津" |
| K2     | "广州", "北京", "深圳" |
| K3     | "成都", "上海", "杭州" |
| K4     | "上海", "天津"         |
| K5     | "杭州", "大连"         |

#### 贪心算法介绍

1 贪心算法(贪婪算法) 是指在对问题进行求解时，在每一步选择中都采取最好或者最优(即最有利)的选择，从而希望能够导致结果是最好或者最优的算法。

2 贪心算法所得到的结果==不一定是最优的结果==(有时候会是最优解)，但是都是相对近似(接近)最优解的结果。

#### 思路分析：

如何找出覆盖所有地区的广播台的集合呢，使用穷举法实现，列出每个可能的广播台的集合，这被称为幂集。假设总的有n个广播台，则广播台的组合总共有2^n^-1个，假设每秒可以计算10个子集，如图：

| 广播台数量n | 子集总数2ⁿ | 需要的时间 |
| ----------- | ---------- | ---------- |
| 5           | 32         | 3.2秒      |
| 10          | 1024       | 102.4秒    |
| 32          | 4294967296 | 13.6年     |
| 100         | 1.26*100³º | 4x10²³年   |

**由此可见**：在进行组合的场景下，使用组合效率是很低的。

#### 使用贪心算法，效率高：

目前并没有算法可以快速计算得到准备的值，使用贪心算法，则可以得到非常接近的解，并且效率高。选择策略上，因为需要覆盖全部地区的最小集合：

1.  遍历所有的广播电台，找到一个覆盖了最多<font style="color:red">未覆盖的地区</font>的电台(此电台可能包含一些已覆盖的地区，但没有关系)
2.  将这个电台加入到一个集合中(比如ArrayList)，想办法把该电台覆盖的地区在下次比较时去掉。
3.  重复第1步直到覆盖了全部的地区。

图解：

![image-20230715221702246](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307152217484.png)

代码实现：

```java
@SuppressWarnings("all")
public class GreedyAlgorithm {
    public static void main(String[] args) {
        //创建广播电台，放入到HashMap
        HashMap<String, HashSet<String>> broadcasts = new HashMap<String, HashSet<String>>();
        //将各个电台放入到broadcasts
        HashSet<String> hashSet1 = new HashSet<>();
        hashSet1.add("北京");
        hashSet1.add("上海");
        hashSet1.add("天津");
        HashSet<String> hashSet2 = new HashSet<>();
        hashSet2.add("广州");
        hashSet2.add("上海");
        hashSet2.add("深圳");
        HashSet<String> hashSet3 = new HashSet<>();
        hashSet3.add("成都");
        hashSet3.add("上海");
        hashSet3.add("杭州");
        HashSet<String> hashSet4 = new HashSet<>();
        hashSet4.add("上海");
        hashSet4.add("天津");
        HashSet<String> hashSet5 = new HashSet<>();
        hashSet5.add("杭州");
        hashSet5.add("大连");
        //将所有电台加入到Map集合中
        broadcasts.put("K1", hashSet1);
        broadcasts.put("K2", hashSet2);
        broadcasts.put("K3", hashSet3);
        broadcasts.put("K4", hashSet4);
        broadcasts.put("K5", hashSet5);

        HashSet<String> allAreas = new HashSet<>();
        Collections.addAll(allAreas, "北京", "上海", "天津", "广州", "深圳", "成都", "杭州", "大连");

        //创建一个ArrayList存放选择的电台集合
        ArrayList<String> selects = new ArrayList<>();

        //定义临时集合保存在遍历过程中的电台覆盖的地区和当前还没有覆盖的地区的交集
        HashSet<String> tempSet = new HashSet<>();

        //定义maxKey保存在一次遍历过程中，能够覆盖的最多位覆盖的地区对应的电台的Key
        String maxKey = null;
        //如果maxKey不为null则加入到selects中
        while (allAreas.size() != 0) {//如果allAreas不为0,则表示还没有覆盖到所有的地区
            maxKey = null;
            for (String key : broadcasts.keySet()) {
                tempSet.clear();
                HashSet<String> areas = broadcasts.get(key);
                tempSet.addAll(areas);
                //求出tempSet和allAreas集合的交集,交集会赋给tempSet
                tempSet.retainAll(allAreas);
                //如果当前集合包含的未覆盖地区的数量比maxKey指向的集合未覆盖的地区还要多
                //就需要重置maxKey
                if (tempSet.size() > 0 && (maxKey == null || tempSet.size() > broadcasts.get(maxKey).size()))
                    maxKey = key;
            }
            //maxKey != null，就应该将maxKey加入selects
            if (maxKey != null) {
                selects.add(maxKey);
                //将maxKey指向的广播电台覆盖的地区，从allAreas去掉
                allAreas.removeAll(broadcasts.get(maxKey));
            }
        }
        System.out.println("得到的选择结果是: " + selects);//[k1,k2,k3,k5]
    }
}
```

#### 贪心算法注意事项和细节

1 贪心算法所得到的结果不一定是最优的结果(有时候会是最优解)，但是都是相对近似(接近)最优解的结果。

2 比如上题的算法选出的是K1，K2，K3，K5，符合覆盖了全部的地区但是我们发现K2，K3，K4，K5，也可以覆盖全部地区，如果K2的使用成本低于K1那么上题的K1，K2，K3，K5，虽然满足条件，但并不是最优解。
