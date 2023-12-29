---
layout: post
title: 普利姆算法
categories: [java,程序员常用算法]
descripiton: 程序员常用算法-普利姆算法
keywords: 算法
---

### 普利姆算法

应用场景和问题：

![image-20230716093030683](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307160930413.png)

1 有胜利乡有7个村庄(A,B,C,D,E,F,G)，现在需要修路把7个村庄连通。

2 各个村庄的距离用边线表示(权)，比如A - B 距离 5 公里。

3 问：如何修路保证各个村庄都能连通，并且总的修建公路总里程最短？

**思路**：

​	将10条边，链接即可，但是总的里程数不是最小。

**正确的思路**：

​	就是尽可能的选择少的路线，并且每条路线最小，保证总里程数最少。

#### 最小生成树

修路问题本质就是最小生成树问题，先介绍一下何为最小生成树(Minimum Cost Spanning Tree)，简称MST。

1 给定一个带权的无向连通图，如何选取一棵生成树，使树上所有边上权的总和为最小，这叫最小生成树

2 N个顶点，一定有N-1条边

3 包含全部顶点

4 N-1条边都在图中

5 举例说明(如图)

![image-20230716093418628](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307160934701.png)

6 求最小生成树的算法主要是==普利姆算法==和==克鲁斯卡尔算法== 

#### 普利姆算法介绍

1 普利姆(Prim)算法求最小生成树，也就是在包含n个顶点的连通图中，找出只有(n-1)条边包含所有n个顶点的连通子图，也就是所谓的==极小连通子图== 

2 普利姆的算法如下：

1.  设G=(V,E)是连通网。T=(U,D)是最小生成树。V,U是顶点集合。E,D是边的集合。
2.  若从顶点u开始构造最小生成树，则从集合V中取出顶点u放入集合U中，标记顶点v的visited[u] = 1
3.  若集合U中顶点ui与集合V-U中的顶点vj之间存在边，则寻找这些边中权值最小的边，但不能构成回路，将顶点vj加入集合U中，将边(ui,vj)加入集合D中，标记visited[vj] = 1
4.  重复步骤②，直到U与V相等，即所有顶点都被标记为访问过，此时D中有n-1条边
5.  ==提示==：单独看步骤很难理解，我们通过代码来讲解，比较好理解。

#### 普利姆算法图解

上面的文字描述很难理解，下面是图解分析

![image-20230716102343503](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307161023485.png)

代码实现

```java
{}
@SuppressWarnings("all")
public class PrimAlgorithm {
    public static void main(String[] args) {
        //测试图是否创建成功
        char[] data = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int vertx = data.length;
        //邻接矩阵的关系使用二维数组表示，10000这个大数，表示两个点不连通
        int[][] weight = {{10000, 5, 7, 10000, 10000, 10000, 2},
                {5, 10000, 10000, 9, 10000, 10000, 3},
                {7, 10000, 10000, 10000, 8, 10000, 10000},
                {10000, 9, 10000, 10000, 10000, 4, 10000},
                {10000, 10000, 8, 10000, 10000, 5, 4},
                {10000, 10000, 10000, 4, 5, 10000, 6},
                {2, 3, 10000, 10000, 4, 6, 10000}};
        //创建MGraph对象
        MGraph mGraph = new MGraph(vertx);
        //创建一个MiniTree对象
        MiniTree minTree = new MiniTree();
        minTree.createGraph(mGraph, vertx, data, weight);
        //输出
        minTree.showGraph(mGraph);
        //测试普利姆算法
        minTree.prim(mGraph,0);
    }
}

//创建最小生成树
@SuppressWarnings("all")
class MiniTree {

    /**
     * 创建图的邻接矩阵
     *
     * @param graph  图对象
     * @param vertx  图对应的顶点个数
     * @param data   图的各个顶点的值
     * @param weight 图的邻接矩阵
     */
    public void createGraph(MGraph graph, int vertx, char[] data, int[][] weight) {
        int i, j;
        for (i = 0; i < vertx; i++) {//顶点
            graph.data[i] = data[i];
            for (j = 0; j < vertx; j++)
                graph.weight[i][j] = weight[i][j];
        }
    }

    /**
     * 显示图的邻接矩阵
     *
     * @param graph
     */
    public void showGraph(MGraph graph) {
        for (int[] itemp : graph.weight)
            System.out.println(Arrays.toString(itemp));
    }

    /**
     * 编写prim算法，得到最小生成树
     *
     * @param graph 图
     * @param v     表示从图的第几个顶点开始生成 'A' -> 0 , 'B' -> 1 ...
     */
    public void prim(MGraph graph, int v) {
        //标记节点(顶点)是否被访问过
        int[] visited = new int[graph.vertx];
        //visited[] 默认元素的值都是0，表示没有访问过,Java中默认为0所以不能特意进行次操作
        //for(int i = 0;i < graph.vertx;i ++)
        //visited[i] = 0;
        //把当前节点标记为已访问
        visited[v] = 1;
        //h1,h2记录两个顶点的下标
        int h1 = -1;
        int h2 = -1;
        //将miniweight初始成一个大数，后面在遍历过程中，会被替换
        int miniWeight = 10000;
        //因为有graph.vertx顶点，普利姆算法结束后，有graph.vertx - 1 边
        for (int k = 1; k < graph.vertx; k++) {
            //遍历当前顶点可连通的所有的顶点
            //确定每一次生成的子图，和哪个节点的距离最近
            for (int i = 0; i < graph.vertx; i++)//i节点表示被访问过的节点
                for (int j = 0; j < graph.vertx; j++)//j节点表示没有被访问过的节点
                    if (visited[i] == 1 && visited[j] == 0 && graph.weight[i][j] < miniWeight) {
                        //替换miniweight(寻找已经访问过的节点和为访问过的节点间的权值最小的边)
                        miniWeight = graph.weight[i][j];
                        h1 = i;
                        h2 = j;
                    }
            //找到一条边是最小
            System.out.println("边 <" + graph.data[h1] + "," + graph.data[h2] + "> 权值: " + miniWeight);
            //将当前这个节点标记为已访问过
            visited[h2] = 1;
            //将 miniWeight 重新设置为 最大值
            miniWeight = 10000;
        }
    }
}

@SuppressWarnings("all")
class MGraph {
    int vertx;//表示图的节点个数
    char[] data;//存放节点数据
    int[][] weight;//存放边，就是邻接矩阵

    public MGraph(int vertx) {
        this.vertx = vertx;
        data = new char[vertx];
        weight = new int[vertx][vertx];
    }
}

```

结果：

```java
[10000, 5, 7, 10000, 10000, 10000, 2]
[5, 10000, 10000, 9, 10000, 10000, 3]
[7, 10000, 10000, 10000, 8, 10000, 10000]
[10000, 9, 10000, 10000, 10000, 4, 10000]
[10000, 10000, 8, 10000, 10000, 5, 4]
[10000, 10000, 10000, 4, 5, 10000, 6]
[2, 3, 10000, 10000, 4, 6, 10000]
边 <A,G> 权值: 2
边 <G,B> 权值: 3
边 <G,E> 权值: 4
边 <E,F> 权值: 5
边 <F,D> 权值: 4
边 <A,C> 权值: 7
```
