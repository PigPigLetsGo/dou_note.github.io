---
layout: post
title: 斐波那契(黄金分割法)查找算法
categories: [java,数据结构与算法,算法,查找]
description: 斐波那契(黄金分割法)查找算法的介绍
keywords: 算法
---

### 斐波那契(黄金分割法)查找算法

**斐波那契(黄金分割法)查找基本介绍**:

1. 黄金分割点是指把一条[线段](https://baike.baidu.com/item/%E7%BA%BF%E6%AE%B5/8679802)分割为两部分，使其中一部分与全长之比等于另一部分与这部分之比。取其前三位数字的近似值是[0.618](https://baike.baidu.com/item/0.618)。由于按此比例设计的造型十分美丽，因此称为[黄金分割](https://baike.baidu.com/item/%E9%BB%84%E9%87%91%E5%88%86%E5%89%B2/115896)，也称为[中外比](https://baike.baidu.com/item/%E4%B8%AD%E5%A4%96%E6%AF%94/7590939)。这是一个神奇的数字，会带来意向不大的效果。  

2. 斐波那契数列  {1, 1, 2, 3, 5, 8, 13, 21, 34, 55 } 发现斐波那契数列的两个相邻数 的比例，无限接近 黄金分割值0.618

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902110.png)

斐波那契(黄金分割法)查找算法

**斐波那契(黄金分割法)原理**:

斐波那契查找原理与前两种相似，仅仅  改变了中间结点（mid）的位置，mid不  再是中间或插值得到，而是位于黄金分  割点附近，即mid=low+F(k-1)-1  （F代表斐波那契数列），如下图所示
对F(k-1)-1的理解：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309161902101.png)

1. 由斐波那契数列 F[k]=F[k-1]+F[k-2] 的性质，可以得到 （F[k]-1）=（F[k-1]-1）+（F[k-2]-1）+1 。该式说明：只要顺序表的长度为F[k]-1，则可以将该表分成长度为F[k-1]-1和F[k-2]-1的两段，即如上图所示。从而中间位置为mid=low+F(k-1)-1
2. 类似的，每一子段也可以用相同的方式分割
3. 但顺序表长度n不一定刚好等于F[k]-1，所以需要将原来的顺序表长度n增加至F[k]-1。这里的k值只要能使得F[k]-1恰好大于或等于n即可，由以下代码得到,顺序表长度增加后，新增的位置（从n+1到F[k]-1位置），都赋为n位置的值即可。

**斐波那契查找应用案例**：

请对一个有序数组进行斐波那契查找 {1,8, 10, 89, 1000, 1234} ，输入一个数看看该数组是否存在此数，并且求出下标，如果没有就提示"没有这个数"。

**代码实现**：

```java
@SuppressWarnings("all")
public class FibonnacciSearch {
    public static int maxSize = 20;
    public static void main(String[] args) {
        int[] arr = {1,8, 10, 89, 1000, 1234};
        int i = fibSearch(arr, 89);
        System.out.println("index : "+i);
        System.out.println(Arrays.toString(fib()));
    }
    /**
     * 因为后面我们mid = low + f[k - 1] - 1;需要使用到斐波那契数列，因此我们需要先获取到一个斐波那契数列
     * 非递归方法得到一个斐波那契数列
     * @return
     */
    public static int[] fib(){
        int[] f = new int[maxSize];
        f[0] = 1;
        f[1] = 1;
        for(int i = 2;i < maxSize;i ++){
            f[i] = f[i - 1] + f[i - 2];
        }
        return f;
    }

    /**
     * 编写斐波那契数列查找算法
     * 使用递归方式编写算法
     * @param a 数组
     * @param key 我们需要查找的关键值
     * @return 返回对应的下标，如果没有返回-1
     */
    public static int fibSearch(int[] a,int key){
        int low = 0;
        int high = a.length - 1;
        int k = 0;//表示斐波那契数列分割数值的下标
        int mid = 0;//存放mid的值
        int[] f = fib();
        //f[k] = 1,1,2,3,5,8 (8 - 1 = 7) ==> false k == 5
        while(high > f[k] - 1){
            k ++;
        }
        /**
         * 因为f[k]值可能大于a的长度，因此我们需要使用Arrays类，构造一个新数组，并指向a[]
         * 不足的部分会使用0填充
         * [1, 8, 10, 89, 1000, 1234, 0, 0]
         */
        int[] temp = Arrays.copyOf(a,f[k]);
        /**
         * 实际上需求使用a数组最后的数填充temp
         * 举例：
         * temp = {1,8, 10, 89, 1000, 1234} => {1,8, 10, 89, 1000, 1234,1234,1234}
         */
        for(int i = high + 1;i < temp.length;i ++){
            temp[i] = a[high];
        }
        //使用while来循环处理，找到我们的数key
        while(low <= high){
            mid = low + f[k - 1] - 1;
            if(key < temp	[mid]){
                high = mid - 1;
                /**
                 * 为什么是 k--
                 * 说明：
                 * 1.全部元素 = 前面的元素 + 后面元素
                 * 2.f[k] = f[k - 1] + f[k - 2];
                 * 因为前面有f[k - 1]个元素，所以可以继续拆分f[k - 1] = f[k - 2] + f[k - 3]
                 * 即在f[k - 1]的前面继续查找k --
                 * 即下次循环mid = f[k - 1 - 1] - 1
                 */
                k--;
            }else if(key > temp[mid]){//我们应该继续向数组的后面查找(右边)
                low = mid + 1;
                /**
                 * 为什么是 k -= 2;
                 * 说明：
                 * 1.全部元素 = 前面元素+后面元素
                 * 2.f[k] = f[k - 1]  + f[k - 2]
                 * 3.因为后面我们有f[k - 2]所以可以继续拆分f[k - 1] = f[k - 3] + f[k - 4]
                 * 4.即在f[k - 2]的前面进行查找k -= 2
                 * 5.即下次循环mid = f[k - 1 - 2] - 1
                 */
                k -= 2;
            }else{
                //需要确定，返回的是哪个下标
                if(mid <= high){
                    return mid;
                }else{
                    return high;
                }
            }
        }
        return -1;
    }
}
```
