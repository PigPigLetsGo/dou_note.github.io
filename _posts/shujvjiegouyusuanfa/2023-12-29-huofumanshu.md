---
layout: post
title: 赫夫曼树
categories: [java,数据结构与算法,算法,数据结构]
description: 赫夫曼树的介绍和实现解压缩代码
keywords: 算法
---

### 赫夫曼树

**基本介绍** 

1.  给定n个权值作为n个 **叶子结点** ，构造一颗二叉树，若该树的**带权路径长度(wpl)达到最小**，称这样的二叉树为**最优二叉树**，也称为**赫夫曼树(HuffmanTree)**，还有的书翻译为**霍夫曼树**。
2.  赫夫曼树是**带权路径长度最短的树**，**权值较大的节点离根较近**。

**赫夫曼树几个重要概念和举例说明** 

1.  **路径和长度**：在一颗树中，从一个节点往下可以达到的孩子或孙子节点之间的通路，称为路径。通路中分支的数目称为路径长度。若规定根节点的层数为1，则从根节点到第L层节点的路径长度为L-1。
2.  **节点的权及带权路径长度**：若将树中节点赋给一个有着某种含义的数值，则这个数值称为该节点的权。**节点的带权路径长度为**：从根节点到该节点之间的路径长度与该节点的权的乘积。

![image-20230705194848316](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230705194848316.png)

3.  树的带权路径长度：树的带权路径长度规定为所有 **叶子结点** 的带权路径长度之和，记为WPL(weighted path length)，权值越大的节点离根节点越近的二叉树才是最优二叉树。
4.  WPL最小的就是**赫夫曼树** 下图中-中间的就是赫夫曼树

![image-20230705195146344](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230705195146344.png)

**赫夫曼树创建思路图解** 

给你一个数列 {13,7,8,3,29,6,1} ，要求转成一颗赫夫曼树

思路分析示意图：

![image-20230705195844840](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230705195844840.png)

**构成赫夫曼树的步骤**：

1.  从小到大进行排序，将每一个数据，每个数据都是一个节点，每个节点可以看成是一颗最简单的二叉树
2.  取出根节点权值最小的两颗二叉树
3.  组成一颗新的二叉树，该新的二叉树的根节点的权值是前两棵二叉树根节点权值的和
4.  再将这棵新的二叉树，以根节点的权值大小再次排序，不断重复 1-2-3-4 的步骤，直到数列中，所有的数据都被处理，就得到一颗赫夫曼树

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/519:01
 * @comment
 */
public class HuffmanTree {
    public static void main(String[] args) {
        int[] arr = {13,7,8,3,29,6,1};
        Node huffmanTree = createHuffmanTree(arr);
        preOrder(huffmanTree);
    }

    public static void preOrder(Node root){
        if(root != null){
            root.preOrder();
        }else{
            System.out.println("赫夫曼二叉树数据为空");
        }
    }

    /**
     * 创建赫夫曼树方法
     * @param arr
     */
    public static Node createHuffmanTree(int[] arr){
        /**
         * 1.遍历arr数组
         * 2.将arr的每个元素构成一个Node
         * 3.将Node放入到ArrayList中
         */
        List<Node> list = new ArrayList<>();
        for(int value:arr){
            list.add(new Node(value));
        }

        while(list.size() > 1){
            //排序从小打大
            Collections.sort(list);

            //取出节点权值最小的两棵二叉树
            //(1)取出权值最小的节点(二叉树)
            Node leftNode = list.get(0);

            //(2)取出第二小的节点(二叉树)
            Node rightNode = list.get(1);

            //(3)构建成一棵新的二叉树
            Node parent = new Node(leftNode.value + rightNode.value);
            parent.left = leftNode;
            parent.right = rightNode;

            //(4)从ArrayList删除处理过的二叉树
            list.remove(leftNode);
            list.remove(rightNode);
            //(5)将parent加入到list
            list.add(parent);
        }

        //返回赫夫曼树的root节点
        return list.get(0);
    }
}
//创建节点类
class Node implements Comparable<Node>{
    /**
     * value: 节点权值
     * left: 指向左子节点
     * right: 指向右子节点
     */
    int value;
    Node left;
    Node right;

    public Node (int value) {
        this.value = value;
    }

    //前序遍历
    public void preOrder(){
        System.out.println(this);
        if(this.left != null){
            this.left.preOrder();
        }
        if(this.right != null){
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }

    @Override
    public int compareTo(Node o) {
        return this.value - o.value;
    }
}
```

#### 赫夫曼编码

**基本介绍**：

1.  赫夫曼编码也翻译为 **哈夫曼编码(HuffmanCoding)** ，又称霍夫曼编码，是一种编码方式，属于一种程序算法
2.  赫夫曼编码是赫夫曼树在电讯通信中的经典的应用之一。
3.  赫夫曼编码广泛地用于数据文件压缩，其压缩率通常在20% ~ 90%之间
4.  赫夫曼编码是可变字长编码(VLC)的一种，Huffman与1952年提出一种编码方法，称之为最佳编码

原理剖析：

-  通信领域中信息的处理方式1- 定长编码

   -  i like like like java do you like a java    // 共40个字符(包括空格) 

   -  //对应Ascii码

      >  105 32 108 105 107 101 32 108 105 107 101 32 108 105 107 101 32 106 97 118 97 32 100 111 32 121 111 117 32 108 105 107 101 32 97 32 106 97 118 97 

   -  //对应的二进制

      >  01101001 00100000 01101100 01101001 01101011 01100101 00100000 01101100 01101001 01101011 01100101 00100000 01101100 01101001 01101011 01100101 00100000 01101010 01100001 01110110 01100001 00100000 01100100 01101111 00100000 01111001 01101111 01110101 00100000 01101100 01101001 01101011 01100101 00100000 01100001 00100000 01101010 01100001 01110110 01100001 

   -  按照二进制来传递信息，总的长度是 359  (包括空格)

-  通信领域中信息的处理方式2-变长编码

   -  i like like like java do you like a java    // 共40个字符(包括空格)

   -  d:1 y:1 u:1 j:2 v:2 o:2 l:4 k:4 e:4 i:5 a:5  :9 // 各个字符对应的个数

      0= , 1=a, 10=i, 11=e, 100=k, 101=l, 110=o, 111=v, 1000=j, 1001=u, 1010=y, 1011=d

   -  ==说明==：按照各个字符出现的次数进行编码，原则是出现次数越多的，则编码越小，比如 空格出现了9 次， 编码为0 ,其它依次类推.

   -  按照上面给各个字符规定的编码，则我们在传输 "i like like like java do you like a java" 数据时，编码就是

      `10010110100... `

   -  •字符的编码都不能是其他字符编码的前缀，符合此要求的编码叫做前缀编码， 即不能匹配到重复的编码(这个在**赫夫曼编码**中，我们还要进行举例说明, 不捉急)

-  通信领域中信息的处理方式3-赫夫曼编码

   -  d:1 y:1 u:1 j:2 v:2 o:2 l:4 k:4 e:4 i:5 a:5  :9 // 各个字符对应的个数

      按照上面字符出现的次数构建一颗赫夫曼树, 次数作为权值.(图后)

![image-20230706151354255](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230706151354255.png)

//根据赫夫曼树，给各个字符

//规定编码 ， 向左的路径为0

//向右的路径为1 ， 编码如下:

o: 1000  u: 10010 d: 100110 y: 100111 i: 101

a : 110   k: 1110  e: 1111    j: 0000    v: 0001

l: 001     : 01

按照上面的赫夫曼编码，我们的"i like like like java do you like a java"  字符串对应的编码为 (注意这里我们使用的无损压缩)

1010100110111101111010011011110111101001101111011110100001100001110011001111000011001111000100100100110111101111011100100001100001110

**长度为** ： 133 

==说明==:

1.  原来长度是 359 , 压缩了 (359-133) / 359 = 62.9%
2.  此编码满足前缀编码, 即字符的编码都不能是其他字符编码的前缀。不会造成匹配的多义性

==注意==:, 这个赫夫曼树根据排序方法不同，也可能不太一样，**这样对应的赫夫曼编码也不完全一样**，但是wpl 是一样的，都是最小的, 比如: 如果我们让每次生成的新的二叉树总是排在权值相同的二叉树的最后一个，则生成的二叉树为:

![image-20230706151550342](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230706151550342.png)

#### 最佳实践-数据压缩(创建赫夫曼树)

将给出的一段文本，比如 "i like like like java do you like a java" ， 根据前面的讲的赫夫曼编码原理，对其进行数据压缩处理 ，形式如 "1010100110111101111010011011110111101001101111011110100001100001110011001111000011001111000100100100110111101111011100100001100001110"

步骤1：根据赫夫曼编码压缩数据的原理，需要创建 "i like like like java do you like a java" 对应的赫夫曼树.

思路：前面已经分析过了，而且我们已然讲过了构建赫夫曼树的具体实现。

代码实现：

```java
package com.atguigu.huffmancode;

import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;


public class HuffmanCode {

	public static void main(String[] args) {
		String content="i like like like java do you like a java";
		//转成byte数组
		byte[] bytes = content.getBytes();
		//进行赫夫曼编码压缩
		huffmanZip(bytes);
	}
	
	
	private static byte[] huffmanZip(byte[] bytes) {
		//先统计每一个byte出现的次数，并放入一个集合中
		List<Node> nodes = getNodes(bytes);
		//创建一颗赫夫曼树
		Node root = createHuffmanTree(nodes);
		//测试一下 赫夫曼树 是否创建成功!
		//输出root 的 值
		System.out.println(root + " " + root.left + " " + root.right);
		return null;
	}
	
	/**
	 * 
	 * @param bytes byte数组，就是内容字符串对应的byte数组
	 * @return List<Node> 形式如 {Node{'i', 3}, Node{'a', 5}...}
	 */
	private static List<Node> getNodes(byte[] bytes) {
		List<Node> nodes = new ArrayList<>();
		//存储每一个byte出现了多少次。
		Map<Byte, Integer> counts = new HashMap<>();
		//统计每一个byte出现的次数
		for(byte b:bytes) {
			Integer count = counts.get(b);
			if(count==null) {
				counts.put(b, 1);
			}else {
				counts.put(b, count+1);
			}
		}
		//把每一个键值对转为一个node对象,并加入到 nodes集合
		for(Map.Entry<Byte, Integer> entry:counts.entrySet()) {
			nodes.add(new Node(entry.getKey(), entry.getValue()));
		}
		return nodes;
	}
	
	/**
	 * 创建赫夫曼树
	 * @param nodes 传入的是一个node集合
	 * @return 返回赫夫曼树的根节点
	 */
	private static Node createHuffmanTree(List<Node> nodes) {
		
		// 循环处理，
		while (nodes.size() > 1) { // 保证可以get(0) 和 get(1)
			// 排序
			// 说明
			// 1. 需要nodes 集合存放的对象实现 Comparable接口
			Collections.sort(nodes);
			// 取出来权值最小的两个二叉树
			Node left = nodes.get(0);
			// 取出最权值次小的二叉树, 作为新的二叉树的右子树
			Node right = nodes.get(1);
			// 创建一颗新的二叉树 ， 新的节点 data 没有，权值为两颗子树权值和
			Node parent = new Node(null, left.weight + right.weight);
			// 把之前取出来的两颗二叉树设置为新创建的二叉树的子树
			parent.left = left;
			parent.right = right;
			// 把取出来的两个二叉树移除
			nodes.remove(left);
			nodes.remove(right);
			// 放入原来的二叉树集合中
			nodes.add(parent);
		}
		// 返回的节点就是赫夫曼树的根节点.
		return nodes.get(0);
	}


}

//实现 Comparable<Node> 接口是让Node 对象可以进行排序
class Node implements Comparable<Node> {
	Byte data;  // 数据本身 , char 本质就是 Byte
	int weight; // 可以理解成权值, 即字符出现的次数
	Node left;
	Node right;

	public Node(Byte data, int weight) {
		this.data = data;
		this.weight = weight;
	}

	
	@Override
	public int compareTo(Node node) {
		//这样写是从小到大排序
		return this.weight - node.weight;
	}

	
	
	@Override
	public String toString() {
		return "Node [data=" + data + ", weight=" + weight + "]";
	}


	// 前序遍历
	public void preOrder() {
		System.out.println(this);// 先输出父节点
		if (this.left != null) {
			this.left.preOrder();
		}
		if (this.right != null) {
			this.right.preOrder();
		}
	}
}
```

#### 最佳实践-数据压缩(生成赫夫曼编码和赫夫曼编码后的数据)

我们已经生成了 赫夫曼树, 下面我们继续完成任务

1. 生成赫夫曼树对应的赫夫曼编码 , 如下表:  =01 a=100 d=11000 u=11001 e=1110 v=11011 i=101 y=11010 j=0010 k=1111 l=000 o=0011  
2. 使用赫夫曼编码来生成赫夫曼编码数据 ,即按照上面的赫夫曼编码，将"i like like like java do you like a java" 字符串生成对应的编码数据, 形式如下.  
1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100

**思路**：前面已经分析过了，而且我们讲过了生成赫夫曼编码的具体实现。

代码实现：

```java
package com.atguigu.huffmancode;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Set;


public class HuffmanCode {

	public static void main(String[] args) {
		String content="i like like like java do you like a java";
		//转成byte数组
		byte[] bytes = content.getBytes();
		//进行赫夫曼编码压缩
		byte[] huffmanBytes = huffmanZip(bytes);
		
	}
	
	/**
	 * 将 content 进程赫夫曼编码，返回的就是 按 赫夫曼编码后的 byte数组
	 * 比如: "i like like like java do you like a java" => 
	 * 101010011011110111101001101111011110100..  但是 是按照 byte数组来存放的,每8位 放入到一个 byte中!
	 * 比如：huffmanBytes[0] = -41(10101001)  huffmanBytes[1] = -61(10111101) ...
	 * @param bytes
	 * @return
	 */
	private static byte[] huffmanZip(byte[] bytes) {
		//先统计每一个byte出现的次数，并放入一个集合中
		List<Node> nodes = getNodes(bytes);
		//创建一颗赫夫曼树
		Node root = createHuffmanTree(nodes);
		//测试一下 赫夫曼树 是否创建成功!
		//输出root 的 值
		//System.out.println(root + " " + root.left + " " + root.right);
		//创建一个赫夫曼编码表
		Map<Byte, String> huffmanCodes = getCodes(root);
		//测试一把, 看看 	赫夫曼编码表 是否正确
		//32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011
		//System.out.println(huffmanCodes);
		//遍历一把
		Set<Byte> keySet = huffmanCodes.keySet();
		for(Byte key: keySet) {
			System.out.print((char)key.intValue() + "=" + huffmanCodes.get(key) + " ");
		}
		//对 bytes 进行哈夫曼编码，返回的是 一个byte数组
		//"i like like like java do you like a java" 
		//对应的 是 "1010100010111111110.."
		//huffmanBytes[0] = 10101000 = -88
		//huffmanBytes[1] = 10111111 = -65
		byte[] huffmanBytes = zip(bytes,huffmanCodes);
		//测试一个 
		System.out.println("\n" + Arrays.toString(huffmanBytes));
		return huffmanBytes;
		
	}
	
	
	//某个叶子节点存储路径
	static StringBuilder stringBuilder = new StringBuilder();
	//存储赫夫曼编码
	static Map<Byte, String> huffmanCodes = new HashMap<>();
	
	/**
	 * 传入赫夫曼树的根节点，返回对应的赫夫曼编码 Map<Byte, String>
	 * 即形式如(和生成的赫夫曼树有关系)： 	
	 * 			o: 1000   	u: 10010  	d: 100110  		y: 100111  		i: 101
				a: 110     k: 1110    	e: 1111       	j: 0000       	v: 0001
				l: 001 		 : 01

	 * @param tree
	 * @return
	 */
	private static Map<Byte, String> getCodes(Node root) {
		
		if(root==null) {
			return null;
		}
		//处理root的左子树
		getCodes(root.left,"0",stringBuilder);
		//处理root的右子树
		getCodes(root.right,"1",stringBuilder);
		return huffmanCodes;
		
	}
	
	
	/**
	 * 该方法会node 节点的所有叶子节点的赫夫曼编码,并存放在huffmanCodes集合中
	 * 其形式如：{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011}
	 * @param node
	 * @param code
	 * @param stringBuilder
	 */
	private static void getCodes(Node node, String code, StringBuilder stringBuilder) {
		StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
		stringBuilder2.append(code);
		if(node != null) {
			if (node.data == null) {
				getCodes(node.left, "0", stringBuilder2);
				getCodes(node.right, "1", stringBuilder2);
			} else {
				huffmanCodes.put(node.data, stringBuilder2.toString());
			}
		}
	}
	
	/**
	 * 
	 * @param bytes 传入的"i like .." 对应的字节数组，和 哈夫曼编码表
	 * @param huffCodes 返回哈夫曼编码后的byte[]
	 * @return
	 */
	private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {
		StringBuilder stringBuilder = new StringBuilder();
		//把需要压缩的byte数组处理成一个二进制的字符串
		//stringBuilder 就是 形式如 "10101001101111011110..."
		for(byte b:bytes) {
			stringBuilder.append(huffmanCodes.get(b)); 
		}
		System.out.println("stringBuilder(就是哈夫曼编码后的串)=" + stringBuilder);
		//定义就是需要多少个byte来存储，这样就可以定义byte[] 的大小了
		//也可以一句话 len = (stringBuilder.length()+7) / 8
		int len; 
		if (stringBuilder.length() % 8 == 0) {
			len = stringBuilder.length() / 8;
		} else {
			len = stringBuilder.length() / 8 + 1;
		}
		//用于存储压缩后的byte
		byte[] by = new byte[len];
		//记录新byte的位置
		int index = 0;
		for(int i=0;i<stringBuilder.length();i+=8) {
			String strByte;
			if(i+8>stringBuilder.length()) {
				strByte = stringBuilder.substring(i);
			}else {
				strByte = stringBuilder.substring(i, i+8);
			}
			byte byt = (byte)Integer.parseInt(strByte, 2);
			by[index]=byt;
			index++;
		}
		return by;
	}

	/**
	 * 
	 * @param bytes byte数组，就是内容字符串对应的byte数组
	 * @return List<Node> 形式如 {Node{'i', 3}, Node{'a', 5}...}
	 */
	private static List<Node> getNodes(byte[] bytes) {
		List<Node> nodes = new ArrayList<>();
		//存储每一个byte出现了多少次。
		Map<Byte, Integer> counts = new HashMap<>();
		//统计每一个byte出现的次数
		for(byte b:bytes) {
			Integer count = counts.get(b);
			if(count==null) {
				counts.put(b, 1);
			}else {
				counts.put(b, count+1);
			}
		}
		//把每一个键值对转为一个node对象,并加入到 nodes集合
		for(Map.Entry<Byte, Integer> entry:counts.entrySet()) {
			nodes.add(new Node(entry.getKey(), entry.getValue()));
		}
		return nodes;
	}
	
	/**
	 * 创建赫夫曼树
	 * @param nodes 传入的是一个node集合
	 * @return 返回赫夫曼树的根节点
	 */
	private static Node createHuffmanTree(List<Node> nodes) {
		
		// 循环处理，
		while (nodes.size() > 1) { // 保证可以get(0) 和 get(1)
			// 排序
			// 说明
			// 1. 需要nodes 集合存放的对象实现 Comparable接口
			Collections.sort(nodes);
			// 取出来权值最小的两个二叉树
			Node left = nodes.get(0);
			// 取出最权值次小的二叉树, 作为新的二叉树的右子树
			Node right = nodes.get(1);
			// 创建一颗新的二叉树 ， 新的节点 data 没有，权值为两颗子树权值和
			Node parent = new Node(null, left.weight + right.weight);
			// 把之前取出来的两颗二叉树设置为新创建的二叉树的子树
			parent.left = left;
			parent.right = right;
			// 把取出来的两个二叉树移除
			nodes.remove(left);
			nodes.remove(right);
			// 放入原来的二叉树集合中
			nodes.add(parent);
		}
		// 返回的节点就是赫夫曼树的根节点.
		return nodes.get(0);
	}


}

//实现 Comparable<Node> 接口是让Node 对象可以进行排序
class Node implements Comparable<Node> {
	Byte data;  // 数据本身 , char 本质就是 Byte
	int weight; // 可以理解成权值, 即字符出现的次数
	Node left;
	Node right;

	public Node(Byte data, int weight) {
		this.data = data;
		this.weight = weight;
	}

	
	@Override
	public int compareTo(Node node) {
		//这样写是从小到大排序
		return this.weight - node.weight;
	}

	
	
	@Override
	public String toString() {
		return "Node [data=" + data + ", weight=" + weight + "]";
	}


	// 前序遍历
	public void preOrder() {
		System.out.println(this);// 先输出父节点
		if (this.left != null) {
			this.left.preOrder();
		}
		if (this.right != null) {
			this.right.preOrder();
		}
	}
}
```

#### 最佳实践-数据解压(使用赫夫曼编码解码)

使用赫夫曼编码来解码数据，具体要求是

1. 前面我们得到了赫夫曼编码和对应的编码byte[] , 即:[-88, -65, -56, -65, -56, -65, -55, 77  
, -57, 6, -24, -14, -117, -4, -60, -90, 28]  
2. 现在要求使用赫夫曼编码， 进行解码，又  
重新得到原来的字符串"i like like likejava do you like a java"

**思路**：解码过程，就是编码的一个逆向操作。

代码实现：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/521:26
 * @comment
 */
@SuppressWarnings("all")
public class HuffmanTreeCode {
    public static void main(String[] args) {
        String content = "i like like like java do you like a java";
        byte[] contentBytes = content.getBytes();
        System.out.println(contentBytes.length);//40
        byte[] huffmanCodeBytes = huffmanZip(contentBytes);
        System.out.println("压缩后的结果是：" + Arrays.toString(huffmanCodeBytes));

        byte[] sourceBytes = decode(huffmanCodes, huffmanCodeBytes);

        System.out.println("原来的字符串" + new String(sourceBytes));

        //分步过程
        /*
        List<Node01> list = getNodes(contentBytes);
        System.out.println(list);

        System.out.println("----------------赫夫曼树----------------");
        Node01 huffmanTree = createHuffmanTree(list);
        System.out.println("前序遍历");
        preOrder(huffmanTree);

        //测试是否生成了对应的赫夫曼编码
        Map<Byte, String> codes = getCodes(huffmanTree);
        System.out.println("生成的赫夫曼编码表："+codes);
        byte[] huffmanCodeBytes = zip(contentBytes,huffmanCodes);
        System.out.println("huffmanCodeBytes: "+Arrays.toString(huffmanCodeBytes));
         */
    }

    /**
     * 编写一个方法，完成对压缩数据的解码
     *
     * @param huffmanCodes 赫夫曼编码表 map
     * @param huffmanBytes 赫夫曼编码得到的字节数组
     * @return 就是原来的字符串对应的数组
     */
    private static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < huffmanBytes.length; i++) {
            byte b = huffmanBytes[i];
            boolean flag = (i == huffmanBytes.length - 1);
            stringBuilder.append(byteToBitString(!flag, b));
        }
//        System.out.println("赫夫曼字节数组对应的二进制字符串："+stringBuilder.toString());
        //把字符串按照指定的赫夫曼编码进行解码
        //把赫夫曼编码表进行调换，因为反向查询 a -> 100 100 -> a
        Map<String, Byte> map = new HashMap<>();
        for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
            map.put(entry.getValue(), entry.getKey());
        }

        //创建集合，存放byte
        List<Byte> list = new ArrayList<>();
        for (int i = 0; i < stringBuilder.length(); ) {
            int count = 1;//小的计数器
            boolean flag = true;
            Byte b = null;
            while (flag) {
                //先取出一个'1','0'
                String key = stringBuilder.substring(i, i + count);//i不动，让count移动 匹配字符
                b = map.get(key);
                if (b == null) {//b == null说明没有匹配到
                    count++;
                } else {//说明 匹配到了
                    flag = false;
                }
            }
            list.add(b);
            i += count;//i 直接移动到count位置
        }
        //当for循环结束后list中就存放了所有的字符 i like like like java do you like a java
        //把list中 的数据放入byte数组并返回
        byte[] b = new byte[list.size()];
        for (int i = 0; i < b.length; i++) {
            b[i] = list.get(i);
        }
        return b;
    }

    /**
     * 将一个byte转成一个二进制的字符串
     *
     * @param flag 标志是否需要补高位如果是true，需要，false，不需要
     * @param b    传入的byte
     * @return b 对应的二进制的字符串(注意是按补码返回)
     */
    private static String byteToBitString(boolean flag, byte b) {
        //使用变量保存b,将b转换成int
        int temp = b;
        //判断是正数则需要补高位
        if (flag) {
            temp |= 256;
            //说明：按位与 256 1 0000 0000 | 0000 0001 => 1 0000 0001
        }
        //返回的是temp对应的二进制的补码
        String str = Integer.toBinaryString(temp);
        if (flag) {
            return str.substring(str.length() - 8);
        } else {
            return str;
        }
    }

    //使用一个方法，将前面的方法封装起来，便于我们的调用

    /**
     * @param bytes 原始的字符串对应的字节数组
     * @return 是经过赫夫曼编码处理后的字节数组（压缩后的数组）
     */
    private static byte[] huffmanZip(byte[] bytes) {
        List<Node01> nodes = getNodes(bytes);
        //根据nodes创建的赫夫曼树
        Node01 huffmanTreeRoot = createHuffmanTree(nodes);
        //对应的赫夫曼编码(根据 赫夫曼树)
        Map<Byte, String> huffmanCodes = getCodes(huffmanTreeRoot);
        //根据生成的赫夫曼编码，压缩得到压缩后的赫夫曼编码字节数组
        byte[] huffmanCodeBytes = zip(bytes, huffmanCodes);
        return huffmanCodeBytes;
    }

    //编写一个方法，将字符串对应的byte[] 数组，通过生成的赫夫曼编码表，返回一个赫夫曼编码 压缩后的byte[]

    /**
     * @param bytes        这时原始的字符串对应的 byte[]
     * @param huffmanCodes 生成的赫夫曼编码map
     * @return 返回赫夫曼编码处理后的 byte[]
     * 举例： String content = "i like like like java do you like a java"; =》 byte[] contentBytes = content.getBytes();
     * 返回的是 字符串 "1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100"
     * => 对应的 byte[] huffmanCodeBytes  ，即 8位对应一个 byte,放入到 huffmanCodeBytes
     * huffmanCodeBytes[0] =  10101000(补码) => byte  [推导  10101000=> 10101000 - 1 => 10100111(反码)=> 11011000= -88 ]
     * huffmanCodeBytes[1] = -88
     */
    private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {

        //1.利用 huffmanCodes 将  bytes 转成  赫夫曼编码对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        //遍历bytes 数组
        for (byte b : bytes) {
            stringBuilder.append(huffmanCodes.get(b));
        }

        //System.out.println("测试 stringBuilder~~~=" + stringBuilder.toString());

        //将 "1010100010111111110..." 转成 byte[]

        //统计返回  byte[] huffmanCodeBytes 长度
        //一句话 int len = (stringBuilder.length() + 7) / 8;
        int len;
        if (stringBuilder.length() % 8 == 0) {
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建 存储压缩后的 byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;//记录是第几个byte
        for (int i = 0; i < stringBuilder.length(); i += 8) { //因为是每8位对应一个byte,所以步长 +8
            String strByte;
            if (i + 8 > stringBuilder.length()) {//不够8位
                strByte = stringBuilder.substring(i);
            } else {
                strByte = stringBuilder.substring(i, i + 8);
            }
            //将strByte 转成一个byte,放入到 huffmanCodeBytes
            huffmanCodeBytes[index] = (byte) Integer.parseInt(strByte, 2);
            index++;
        }
        return huffmanCodeBytes;
    }

    //生成赫夫曼树对应的赫夫曼编码
    //思路：
    //1.将赫夫曼编码表存放在Map<Byte,String> 形式
    //{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011} 等等[形式]
    static Map<Byte, String> huffmanCodes = new HashMap<>();
    //2.在生成赫夫曼编码表时，需要去拼接路径，定义一个StringBuilder，存储某个叶子节点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    private static Map<Byte, String> getCodes(Node01 root) {
        if (root == null) {
            return null;
        }
        //处理root的左子树
        getCodes(root.left, "0", stringBuilder);
        //处理root的右子树
        getCodes(root.right, "1", stringBuilder);
        return huffmanCodes;
    }

    /**
     * 功能：将传入的Node节点的所有叶子节点的赫夫曼编码存放到，并放入到huffmanCodes集合
     *
     * @param node          传入节点
     * @param code          路径：左子节点是0，右子节点是1
     * @param stringBuilder 用于拼接路径
     */
    private static void getCodes(Node01 node, String code, StringBuilder stringBuilder) {
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code，加入到stringBuilder2中
        stringBuilder2.append(code);
        if (node != null) {
            //判断当前node,是叶子节点还是非叶子节点
            if (node.data == null) {
                //非叶子节点
                //递归处理
                //向左递归
                getCodes(node.left, "0", stringBuilder2);
                //向右递归
                getCodes(node.right, "1", stringBuilder2);
            } else {
                //说明是叶子节点
                //表示找到了某个叶子节点的最后
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }

    //前序遍历
    private static void preOrder(Node01 root) {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("赫夫曼树为空");
        }
    }

    /**
     * @param bytes bytes 接受字节数组
     * @return 返回的就是List形式
     */
    private static List<Node01> getNodes(byte[] bytes) {
        //1.创建一个ArrayList
        List<Node01> list = new ArrayList<>();
        //遍历list，统计每一个byte出现的次数 -> map[key,value]
        Map<Byte, Integer> map = new HashMap<>();
        for (byte b : bytes) {
            Integer count = map.get(b);
            //Map还没有这个字符数据，第一次则执行如下
            if (count == null) {
                map.put(b, 1);
            } else {//已经存在了执行如下
                map.put(b, count + 1);
            }
        }

        //把每一个键值对转成一个Node对象，并加入list集合
        //遍历map
        for (Map.Entry<Byte, Integer> entry : map.entrySet()) {
            list.add(new Node01(entry.getKey(), entry.getValue()));
        }

        return list;
    }

    //通过List创建赫夫曼树
    private static Node01 createHuffmanTree(List<Node01> list) {

        while (list.size() > 1) {
            //排序，从小到大
            Collections.sort(list);
            //取出第一棵最小的二叉树
            Node01 leftNode = list.get(0);
            //取出第二棵最小的二叉树
            Node01 rightNode = list.get(1);
            //创建一颗新的二叉树，它的根节点 没有data，只有权值
            Node01 parent = new Node01(null, leftNode.weight + rightNode.weight);
            parent.left = leftNode;
            parent.right = rightNode;
            //将处理过的两颗二叉树从list中移除
            list.remove(leftNode);
            list.remove(rightNode);
            //将新的二叉树加入到list集合中
            list.add(parent);
        }
        return list.get(0);
    }
}

@SuppressWarnings("all")
class Node01 implements Comparable<Node01> {
    Byte data;
    int weight;
    Node01 left;
    Node01 right;

    public Node01(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node01 o) {
        return this.weight - o.weight;
    }

    public void preOrder() {
        System.out.println(this);
        if (this.left != null) {
            this.left.preOrder();
        }
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node[data = " + data + " , weight = " + weight + "]";
    }
}
```

#### 最佳实践-文件压缩

我们学习了通过赫夫曼编码对一个字符串进行编码和解码, 下面我们来完成对文件的压缩和解压， 具体要求：给你一个图片文件，要求对其进行无损压缩, 看看压缩效果如何。

**思路**：读取文件-> 得到赫夫曼编码表 -> 完成压缩

代码实现：

```java
/**
     * 编写方法，将一个文件进行压缩
     * @param srcFile 你传入的希望压缩的文件的全路径
     * @param dstFile 我们压缩后将压缩文件放到哪个目录
     */
private static void zipFile(String srcFile,String dstFile){
   //创建输出流
   OutputStream os = null;
   ObjectOutputStream oos = null;
   //创建文件的输入流
   FileInputStream is = null;
   try{
      //创建文件的输入流
      is = new FileInputStream(srcFile);
      //创建一个和源文件大小一样的byte[]
      byte[] b = new byte[is.available()];
      //读取文件
      is.read(b);
      //获取到文件对应的赫夫曼编码表
      //直接对原文件进行压缩
      byte[] huffmanBytes = huffmanZip(b);
      //创建文件的输出流,存放压缩文件
      os = new FileOutputStream(dstFile);
      //创建一个和文件输出流关联的ObjectOutputStream
      oos = new ObjectOutputStream(os);
      //把赫夫曼编码后的字节数组写入压缩文件
      oos.writeObject(huffmanBytes);
      //以对象流形式写入赫夫曼编码，为了解压(恢复源文件)时使用
      //注意：一定要把赫夫曼编码，写入压缩文件
      oos.writeObject(huffmanCodes);
   }catch(IOException e){
      System.out.println(e.getMessage());
   }finally{
      //关闭流，释放资源
      try{
         if(is != null){
            is.close();
            os.close();
            oos.close();
         }
      }catch(IOException e){
         System.out.println(e.getMessage());
      }
   }
}
```

完整代码：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/521:26
 * @comment
 */
@SuppressWarnings("all")
public class HuffmanTreeCode {
    public static void main(String[] args) {
        //测试压缩文件
        String srcFile = "E://src.bmp";
        String dstFile = "E://dst.zip";
        zipFile(srcFile,dstFile);
        System.out.println("压缩文件OK");
    }

    /**
     * 编写方法，将一个文件进行压缩
     * @param srcFile 你传入的希望压缩的文件的全路径
     * @param dstFile 我们压缩后将压缩文件放到哪个目录
     */
    private static void zipFile(String srcFile,String dstFile){
        //创建输出流
        OutputStream os = null;
        ObjectOutputStream oos = null;
        //创建文件的输入流
        FileInputStream is = null;
        try{
            //创建文件的输入流
            is = new FileInputStream(srcFile);
            //创建一个和源文件大小一样的byte[]
            byte[] b = new byte[is.available()];
            //读取文件
            is.read(b);
            //获取到文件对应的赫夫曼编码表
            //直接对原文件进行压缩
            byte[] huffmanBytes = huffmanZip(b);
            //创建文件的输出流,存放压缩文件
            os = new FileOutputStream(dstFile);
            //创建一个和文件输出流关联的ObjectOutputStream
            oos = new ObjectOutputStream(os);
            //把赫夫曼编码后的字节数组写入压缩文件
            oos.writeObject(huffmanBytes);
            //以对象流形式写入赫夫曼编码，为了解压(恢复源文件)时使用
            //注意：一定要把赫夫曼编码，写入压缩文件
            oos.writeObject(huffmanCodes);
        }catch(IOException e){
            System.out.println(e.getMessage());
        }finally{
            //关闭流，释放资源
            try{
                if(is != null){
                    is.close();
                    os.close();
                    oos.close();
                }
            }catch(IOException e){
                System.out.println(e.getMessage());
            }
        }
    }

    /**
     * 编写一个方法，完成对压缩数据的解码
     *
     * @param huffmanCodes 赫夫曼编码表 map
     * @param huffmanBytes 赫夫曼编码得到的字节数组
     * @return 就是原来的字符串对应的数组
     */
    private static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < huffmanBytes.length; i++) {
            byte b = huffmanBytes[i];
            boolean flag = (i == huffmanBytes.length - 1);
            stringBuilder.append(byteToBitString(!flag, b));
        }
//        System.out.println("赫夫曼字节数组对应的二进制字符串："+stringBuilder.toString());
        //把字符串按照指定的赫夫曼编码进行解码
        //把赫夫曼编码表进行调换，因为反向查询 a -> 100 100 -> a
        Map<String, Byte> map = new HashMap<>();
        for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
            map.put(entry.getValue(), entry.getKey());
        }

        //创建集合，存放byte
        List<Byte> list = new ArrayList<>();
        for (int i = 0; i < stringBuilder.length(); ) {
            int count = 1;//小的计数器
            boolean flag = true;
            Byte b = null;
            while (flag) {
                //先取出一个'1','0'
                String key = stringBuilder.substring(i, i + count);//i不动，让count移动 匹配字符
                b = map.get(key);
                if (b == null) {//b == null说明没有匹配到
                    count++;
                } else {//说明 匹配到了
                    flag = false;
                }
            }
            list.add(b);
            i += count;//i 直接移动到count位置
        }
        //当for循环结束后list中就存放了所有的字符 i like like like java do you like a java
        //把list中 的数据放入byte数组并返回
        byte[] b = new byte[list.size()];
        for (int i = 0; i < b.length; i++) {
            b[i] = list.get(i);
        }
        return b;
    }

    /**
     * 将一个byte转成一个二进制的字符串
     *
     * @param flag 标志是否需要补高位如果是true，需要，false，不需要
     * @param b    传入的byte
     * @return b 对应的二进制的字符串(注意是按补码返回)
     */
    private static String byteToBitString(boolean flag, byte b) {
        //使用变量保存b,将b转换成int
        int temp = b;
        //判断是正数则需要补高位
        if (flag) {
            temp |= 256;
            //说明：按位与 256 1 0000 0000 | 0000 0001 => 1 0000 0001
        }
        //返回的是temp对应的二进制的补码
        String str = Integer.toBinaryString(temp);
        if (flag) {
            return str.substring(str.length() - 8);
        } else {
            return str;
        }
    }

    //使用一个方法，将前面的方法封装起来，便于我们的调用

    /**
     * @param bytes 原始的字符串对应的字节数组
     * @return 是经过赫夫曼编码处理后的字节数组（压缩后的数组）
     */
    private static byte[] huffmanZip(byte[] bytes) {
        List<Node01> nodes = getNodes(bytes);
        //根据nodes创建的赫夫曼树
        Node01 huffmanTreeRoot = createHuffmanTree(nodes);
        //对应的赫夫曼编码(根据 赫夫曼树)
        Map<Byte, String> huffmanCodes = getCodes(huffmanTreeRoot);
        //根据生成的赫夫曼编码，压缩得到压缩后的赫夫曼编码字节数组
        byte[] huffmanCodeBytes = zip(bytes, huffmanCodes);
        return huffmanCodeBytes;
    }

    //编写一个方法，将字符串对应的byte[] 数组，通过生成的赫夫曼编码表，返回一个赫夫曼编码 压缩后的byte[]

    /**
     * @param bytes        这时原始的字符串对应的 byte[]
     * @param huffmanCodes 生成的赫夫曼编码map
     * @return 返回赫夫曼编码处理后的 byte[]
     * 举例： String content = "i like like like java do you like a java"; =》 byte[] contentBytes = content.getBytes();
     * 返回的是 字符串 "1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100"
     * => 对应的 byte[] huffmanCodeBytes  ，即 8位对应一个 byte,放入到 huffmanCodeBytes
     * huffmanCodeBytes[0] =  10101000(补码) => byte  [推导  10101000=> 10101000 - 1 => 10100111(反码)=> 11011000= -88 ]
     * huffmanCodeBytes[1] = -88
     */
    private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {

        //1.利用 huffmanCodes 将  bytes 转成  赫夫曼编码对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        //遍历bytes 数组
        for (byte b : bytes) {
            stringBuilder.append(huffmanCodes.get(b));
        }

        //System.out.println("测试 stringBuilder~~~=" + stringBuilder.toString());

        //将 "1010100010111111110..." 转成 byte[]

        //统计返回  byte[] huffmanCodeBytes 长度
        //一句话 int len = (stringBuilder.length() + 7) / 8;
        int len;
        if (stringBuilder.length() % 8 == 0) {
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建 存储压缩后的 byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;//记录是第几个byte
        for (int i = 0; i < stringBuilder.length(); i += 8) { //因为是每8位对应一个byte,所以步长 +8
            String strByte;
            if (i + 8 > stringBuilder.length()) {//不够8位
                strByte = stringBuilder.substring(i);
            } else {
                strByte = stringBuilder.substring(i, i + 8);
            }
            //将strByte 转成一个byte,放入到 huffmanCodeBytes
            huffmanCodeBytes[index] = (byte) Integer.parseInt(strByte, 2);
            index++;
        }
        return huffmanCodeBytes;
    }

    //生成赫夫曼树对应的赫夫曼编码
    //思路：
    //1.将赫夫曼编码表存放在Map<Byte,String> 形式
    //{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011} 等等[形式]
    static Map<Byte, String> huffmanCodes = new HashMap<>();
    //2.在生成赫夫曼编码表时，需要去拼接路径，定义一个StringBuilder，存储某个叶子节点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    private static Map<Byte, String> getCodes(Node01 root) {
        if (root == null) {
            return null;
        }
        //处理root的左子树
        getCodes(root.left, "0", stringBuilder);
        //处理root的右子树
        getCodes(root.right, "1", stringBuilder);
        return huffmanCodes;
    }

    /**
     * 功能：将传入的Node节点的所有叶子节点的赫夫曼编码存放到，并放入到huffmanCodes集合
     *
     * @param node          传入节点
     * @param code          路径：左子节点是0，右子节点是1
     * @param stringBuilder 用于拼接路径
     */
    private static void getCodes(Node01 node, String code, StringBuilder stringBuilder) {
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code，加入到stringBuilder2中
        stringBuilder2.append(code);
        if (node != null) {
            //判断当前node,是叶子节点还是非叶子节点
            if (node.data == null) {
                //非叶子节点
                //递归处理
                //向左递归
                getCodes(node.left, "0", stringBuilder2);
                //向右递归
                getCodes(node.right, "1", stringBuilder2);
            } else {
                //说明是叶子节点
                //表示找到了某个叶子节点的最后
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }

    //前序遍历
    private static void preOrder(Node01 root) {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("赫夫曼树为空");
        }
    }

    /**
     * @param bytes bytes 接受字节数组
     * @return 返回的就是List形式
     */
    private static List<Node01> getNodes(byte[] bytes) {
        //1.创建一个ArrayList
        List<Node01> list = new ArrayList<>();
        //遍历list，统计每一个byte出现的次数 -> map[key,value]
        Map<Byte, Integer> map = new HashMap<>();
        for (byte b : bytes) {
            Integer count = map.get(b);
            //Map还没有这个字符数据，第一次则执行如下
            if (count == null) {
                map.put(b, 1);
            } else {//已经存在了执行如下
                map.put(b, count + 1);
            }
        }

        //把每一个键值对转成一个Node对象，并加入list集合
        //遍历map
        for (Map.Entry<Byte, Integer> entry : map.entrySet()) {
            list.add(new Node01(entry.getKey(), entry.getValue()));
        }

        return list;
    }

    //通过List创建赫夫曼树
    private static Node01 createHuffmanTree(List<Node01> list) {

        while (list.size() > 1) {
            //排序，从小到大
            Collections.sort(list);
            //取出第一棵最小的二叉树
            Node01 leftNode = list.get(0);
            //取出第二棵最小的二叉树
            Node01 rightNode = list.get(1);
            //创建一颗新的二叉树，它的根节点 没有data，只有权值
            Node01 parent = new Node01(null, leftNode.weight + rightNode.weight);
            parent.left = leftNode;
            parent.right = rightNode;
            //将处理过的两颗二叉树从list中移除
            list.remove(leftNode);
            list.remove(rightNode);
            //将新的二叉树加入到list集合中
            list.add(parent);
        }
        return list.get(0);
    }
}

@SuppressWarnings("all")
class Node01 implements Comparable<Node01> {
    Byte data;
    int weight;
    Node01 left;
    Node01 right;

    public Node01(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node01 o) {
        return this.weight - o.weight;
    }

    public void preOrder() {
        System.out.println(this);
        if (this.left != null) {
            this.left.preOrder();
        }
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node[data = " + data + " , weight = " + weight + "]";
    }
}
```

#### 最佳实践——文件解压(文件恢复)

具体要求：将前面压缩的文件，重新恢复成原来的文件。

思路：读取压缩文件(数据和赫夫曼编码表) -> 完成解压(文件恢复)

```java
/**
     * 完成对压缩文件的解压
     * @param zipFile 准备解压的文件
     * @param dstFile 将文件解压到哪个路径
     */
private static void unZipFile(String zipFile,String dstFile){
   InputStream is = null;
   ObjectInputStream ois = null;
   OutputStream os = null;
   try{
      is = new FileInputStream(zipFile);
      ois = new ObjectInputStream(is);
      //读取byte数组 huffmanBytes
      byte[] huffmanBytes = (byte[]) ois.readObject();
      //读取赫夫曼编码表
      Map<Byte,String> huffmanCodes = (Map<Byte,String>) ois.readObject();
      //解码
      byte[] bytes = decode(huffmanCodes,huffmanBytes);
      //将bytes,数组写入到目标文件
      os = new FileOutputStream(dstFile);
      //写出数据到文件中
      os.write(bytes);
   }catch(Exception e){
      System.out.println(e.getMessage());
   }finally{
      //关闭流，释放资源
      try{
         if(os != null){
            os.close();
         }
         if(ois != null){
            ois.close();
         }
         if(is != null){
            is.close();
         }
      }catch(Exception e){
         System.out.println(e.getMessage());
      }
   }
}
```

完整代码：

```java
/**
 * @author 窦凯欣
 * @version 1.0
 * @2023/7/521:26
 * @comment
 */
@SuppressWarnings("all")
public class HuffmanTreeCode {
    public static void main(String[] args) {
        //测试压缩文件
        /*
        String srcFile = "E://src.bmp";
        String dstFile = "E://dst.zip";
        zipFile(srcFile,dstFile);
        System.out.println("压缩文件OK");
         */

        //测试解压文件
        String zipFile = "E://dst.zip";
        String dstFile = "E://srcTwo.bmp";
        unZipFile(zipFile,dstFile);
        System.out.println("解压完毕OK");
    }


    /**
     * 完成对压缩文件的解压
     * @param zipFile 准备解压的文件
     * @param dstFile 将文件解压到哪个路径
     */
    private static void unZipFile(String zipFile,String dstFile){
        InputStream is = null;
        ObjectInputStream ois = null;
        OutputStream os = null;
        try{
            is = new FileInputStream(zipFile);
            ois = new ObjectInputStream(is);
            //读取byte数组 huffmanBytes
            byte[] huffmanBytes = (byte[]) ois.readObject();
            //读取赫夫曼编码表
            Map<Byte,String> huffmanCodes = (Map<Byte,String>) ois.readObject();
            //解码
            byte[] bytes = decode(huffmanCodes,huffmanBytes);
            //将bytes,数组写入到目标文件
            os = new FileOutputStream(dstFile);
            //写出数据到文件中
            os.write(bytes);
        }catch(Exception e){
            System.out.println(e.getMessage());
        }finally{
            //关闭流，释放资源
            try{
                if(os != null){
                    os.close();
                }
                if(ois != null){
                    ois.close();
                }
                if(is != null){
                    is.close();
                }
            }catch(Exception e){
                System.out.println(e.getMessage());
            }
        }
    }

    /**
     * 编写方法，将一个文件进行压缩
     * @param srcFile 你传入的希望压缩的文件的全路径
     * @param dstFile 我们压缩后将压缩文件放到哪个目录
     */
    private static void zipFile(String srcFile,String dstFile){
        //创建输出流
        OutputStream os = null;
        ObjectOutputStream oos = null;
        //创建文件的输入流
        FileInputStream is = null;
        try{
            //创建文件的输入流
            is = new FileInputStream(srcFile);
            //创建一个和源文件大小一样的byte[]
            byte[] b = new byte[is.available()];
            //读取文件
            is.read(b);
            //获取到文件对应的赫夫曼编码表
            //直接对原文件进行压缩
            byte[] huffmanBytes = huffmanZip(b);
            //创建文件的输出流,存放压缩文件
            os = new FileOutputStream(dstFile);
            //创建一个和文件输出流关联的ObjectOutputStream
            oos = new ObjectOutputStream(os);
            //把赫夫曼编码后的字节数组写入压缩文件
            oos.writeObject(huffmanBytes);
            //以对象流形式写入赫夫曼编码，为了解压(恢复源文件)时使用
            //注意：一定要把赫夫曼编码，写入压缩文件
            oos.writeObject(huffmanCodes);
        }catch(IOException e){
            System.out.println(e.getMessage());
        }finally{
            //关闭流，释放资源
            try{
                if(is != null){
                    is.close();
                    os.close();
                    oos.close();
                }
            }catch(IOException e){
                System.out.println(e.getMessage());
            }
        }
    }

    /**
     * 编写一个方法，完成对压缩数据的解码
     *
     * @param huffmanCodes 赫夫曼编码表 map
     * @param huffmanBytes 赫夫曼编码得到的字节数组
     * @return 就是原来的字符串对应的数组
     */
    private static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < huffmanBytes.length; i++) {
            byte b = huffmanBytes[i];
            boolean flag = (i == huffmanBytes.length - 1);
            stringBuilder.append(byteToBitString(!flag, b));
        }
//        System.out.println("赫夫曼字节数组对应的二进制字符串："+stringBuilder.toString());
        //把字符串按照指定的赫夫曼编码进行解码
        //把赫夫曼编码表进行调换，因为反向查询 a -> 100 100 -> a
        Map<String, Byte> map = new HashMap<>();
        for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
            map.put(entry.getValue(), entry.getKey());
        }

        //创建集合，存放byte
        List<Byte> list = new ArrayList<>();
        for (int i = 0; i < stringBuilder.length(); ) {
            int count = 1;//小的计数器
            boolean flag = true;
            Byte b = null;
            while (flag) {
                //先取出一个'1','0'
                String key = stringBuilder.substring(i, i + count);//i不动，让count移动 匹配字符
                b = map.get(key);
                if (b == null) {//b == null说明没有匹配到
                    count++;
                } else {//说明 匹配到了
                    flag = false;
                }
            }
            list.add(b);
            i += count;//i 直接移动到count位置
        }
        //当for循环结束后list中就存放了所有的字符 i like like like java do you like a java
        //把list中 的数据放入byte数组并返回
        byte[] b = new byte[list.size()];
        for (int i = 0; i < b.length; i++) {
            b[i] = list.get(i);
        }
        return b;
    }

    /**
     * 将一个byte转成一个二进制的字符串
     *
     * @param flag 标志是否需要补高位如果是true，需要，false，不需要
     * @param b    传入的byte
     * @return b 对应的二进制的字符串(注意是按补码返回)
     */
    private static String byteToBitString(boolean flag, byte b) {
        //使用变量保存b,将b转换成int
        int temp = b;
        //判断是正数则需要补高位
        if (flag) {
            temp |= 256;
            //说明：按位与 256 1 0000 0000 | 0000 0001 => 1 0000 0001
        }
        //返回的是temp对应的二进制的补码
        String str = Integer.toBinaryString(temp);
        if (flag) {
            return str.substring(str.length() - 8);
        } else {
            return str;
        }
    }

    //使用一个方法，将前面的方法封装起来，便于我们的调用

    /**
     * @param bytes 原始的字符串对应的字节数组
     * @return 是经过赫夫曼编码处理后的字节数组（压缩后的数组）
     */
    private static byte[] huffmanZip(byte[] bytes) {
        List<Node01> nodes = getNodes(bytes);
        //根据nodes创建的赫夫曼树
        Node01 huffmanTreeRoot = createHuffmanTree(nodes);
        //对应的赫夫曼编码(根据 赫夫曼树)
        Map<Byte, String> huffmanCodes = getCodes(huffmanTreeRoot);
        //根据生成的赫夫曼编码，压缩得到压缩后的赫夫曼编码字节数组
        byte[] huffmanCodeBytes = zip(bytes, huffmanCodes);
        return huffmanCodeBytes;
    }

    //编写一个方法，将字符串对应的byte[] 数组，通过生成的赫夫曼编码表，返回一个赫夫曼编码 压缩后的byte[]

    /**
     * @param bytes        这时原始的字符串对应的 byte[]
     * @param huffmanCodes 生成的赫夫曼编码map
     * @return 返回赫夫曼编码处理后的 byte[]
     * 举例： String content = "i like like like java do you like a java"; =》 byte[] contentBytes = content.getBytes();
     * 返回的是 字符串 "1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100"
     * => 对应的 byte[] huffmanCodeBytes  ，即 8位对应一个 byte,放入到 huffmanCodeBytes
     * huffmanCodeBytes[0] =  10101000(补码) => byte  [推导  10101000=> 10101000 - 1 => 10100111(反码)=> 11011000= -88 ]
     * huffmanCodeBytes[1] = -88
     */
    private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {

        //1.利用 huffmanCodes 将  bytes 转成  赫夫曼编码对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        //遍历bytes 数组
        for (byte b : bytes) {
            stringBuilder.append(huffmanCodes.get(b));
        }

        //System.out.println("测试 stringBuilder~~~=" + stringBuilder.toString());

        //将 "1010100010111111110..." 转成 byte[]

        //统计返回  byte[] huffmanCodeBytes 长度
        //一句话 int len = (stringBuilder.length() + 7) / 8;
        int len;
        if (stringBuilder.length() % 8 == 0) {
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建 存储压缩后的 byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;//记录是第几个byte
        for (int i = 0; i < stringBuilder.length(); i += 8) { //因为是每8位对应一个byte,所以步长 +8
            String strByte;
            if (i + 8 > stringBuilder.length()) {//不够8位
                strByte = stringBuilder.substring(i);
            } else {
                strByte = stringBuilder.substring(i, i + 8);
            }
            //将strByte 转成一个byte,放入到 huffmanCodeBytes
            huffmanCodeBytes[index] = (byte) Integer.parseInt(strByte, 2);
            index++;
        }
        return huffmanCodeBytes;
    }

    //生成赫夫曼树对应的赫夫曼编码
    //思路：
    //1.将赫夫曼编码表存放在Map<Byte,String> 形式
    //{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011} 等等[形式]
    static Map<Byte, String> huffmanCodes = new HashMap<>();
    //2.在生成赫夫曼编码表时，需要去拼接路径，定义一个StringBuilder，存储某个叶子节点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    private static Map<Byte, String> getCodes(Node01 root) {
        if (root == null) {
            return null;
        }
        //处理root的左子树
        getCodes(root.left, "0", stringBuilder);
        //处理root的右子树
        getCodes(root.right, "1", stringBuilder);
        return huffmanCodes;
    }

    /**
     * 功能：将传入的Node节点的所有叶子节点的赫夫曼编码存放到，并放入到huffmanCodes集合
     *
     * @param node          传入节点
     * @param code          路径：左子节点是0，右子节点是1
     * @param stringBuilder 用于拼接路径
     */
    private static void getCodes(Node01 node, String code, StringBuilder stringBuilder) {
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code，加入到stringBuilder2中
        stringBuilder2.append(code);
        if (node != null) {
            //判断当前node,是叶子节点还是非叶子节点
            if (node.data == null) {
                //非叶子节点
                //递归处理
                //向左递归
                getCodes(node.left, "0", stringBuilder2);
                //向右递归
                getCodes(node.right, "1", stringBuilder2);
            } else {
                //说明是叶子节点
                //表示找到了某个叶子节点的最后
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }

    //前序遍历
    private static void preOrder(Node01 root) {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("赫夫曼树为空");
        }
    }

    /**
     * @param bytes bytes 接受字节数组
     * @return 返回的就是List形式
     */
    private static List<Node01> getNodes(byte[] bytes) {
        //1.创建一个ArrayList
        List<Node01> list = new ArrayList<>();
        //遍历list，统计每一个byte出现的次数 -> map[key,value]
        Map<Byte, Integer> map = new HashMap<>();
        for (byte b : bytes) {
            Integer count = map.get(b);
            //Map还没有这个字符数据，第一次则执行如下
            if (count == null) {
                map.put(b, 1);
            } else {//已经存在了执行如下
                map.put(b, count + 1);
            }
        }

        //把每一个键值对转成一个Node对象，并加入list集合
        //遍历map
        for (Map.Entry<Byte, Integer> entry : map.entrySet()) {
            list.add(new Node01(entry.getKey(), entry.getValue()));
        }

        return list;
    }

    //通过List创建赫夫曼树
    private static Node01 createHuffmanTree(List<Node01> list) {

        while (list.size() > 1) {
            //排序，从小到大
            Collections.sort(list);
            //取出第一棵最小的二叉树
            Node01 leftNode = list.get(0);
            //取出第二棵最小的二叉树
            Node01 rightNode = list.get(1);
            //创建一颗新的二叉树，它的根节点 没有data，只有权值
            Node01 parent = new Node01(null, leftNode.weight + rightNode.weight);
            parent.left = leftNode;
            parent.right = rightNode;
            //将处理过的两颗二叉树从list中移除
            list.remove(leftNode);
            list.remove(rightNode);
            //将新的二叉树加入到list集合中
            list.add(parent);
        }
        return list.get(0);
    }
}

@SuppressWarnings("all")
class Node01 implements Comparable<Node01> {
    Byte data;
    int weight;
    Node01 left;
    Node01 right;

    public Node01(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node01 o) {
        return this.weight - o.weight;
    }

    public void preOrder() {
        System.out.println(this);
        if (this.left != null) {
            this.left.preOrder();
        }
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    @Override
    public String toString() {
        return "Node[data = " + data + " , weight = " + weight + "]";
    }
}
```

#### 赫夫曼编码压缩文件注意事项

1.  如果文件本身就是经过压缩处理的，那么使用赫夫曼编码再压缩效率不会有明显变化，比如视频，ppt，等等文件
2.  赫夫曼编码是按字节来处理的，因此可以处理所有的文件(二进制，文本文件)
3.  如果一个文件中的内容，重复的数据不多，压缩效果也不会很明显
