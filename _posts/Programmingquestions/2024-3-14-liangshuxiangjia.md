---
layout: post
title:  两数相加
categories: [java,算法,编程题]
description: 两数相加介绍。
keywords: 编程题
---

# 两数相加

**题目要求**：

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例**：

![image-20240314092446599](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240314092446599.png)

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

**示例 2：**

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

**示例 3：**

```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

**提示：**

-  每个链表中的节点数在范围 `[1, 100]` 内
-  `0 <= Node.val <= 9`
-  题目数据保证列表表示的数字不含前导零

思路与算法

由于输入的两个链表都是逆序存储数字的位数的，因此两个链表中同一位置的数字可以直接相加。

我们同时遍历两个链表，逐位计算它们的和，并与当前位置的进位值相加。具体而言，如果当前两个链表处相应位置的数字为 n1,n2n1,n2n1,n2，进位值为 carry\textit{carry}carry，则它们的和为 $$n1+n2+carryn1+n2+\textit{carry}n1+n2+carry$$；其中，答案链表处相应位置的数字为$$ (n1+n2+carry) mod 10(n1+n2+\textit{carry}) \bmod 10(n1+n2+carry)mod10$$，而新的进位值为 $$⌊n1+n2+carry10⌋\lfloor\frac{n1+n2+\textit{carry}}{10}\rfloor⌊ 10n1+n2+carry⌋$$。

如果两个链表的长度不同，则可以认为长度短的链表的后面有若干个 000 。

此外，如果链表遍历结束后，有 $$carry>0\textit{carry} > 0carry>0$$，还需要在答案链表的后面附加一个节点，节点的值为 carry。

**代码**：

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // 声明头，尾指针
        ListNode head = null, tail = null;
        // 定义进位变量
        int carry = 0;
        while(l1 != null || l2 != null) {
            // 获取 l1 与 l2的 val 值
            int n1 = l1 != null ? l1.val : 0;
            int n2 = l2 != null ? l2.val : 0;
            // 相加
            int sum = n1 + n2 + carry;
            // 如果头指针为null就初始化头指针并赋值还有尾指针
            if(head == null) {
                head = tail = new ListNode(sum % 10);
            } else {
                // 将值复制到尾指针的下一个位置
                tail.next = new ListNode(sum % 10);
                // 尾指针向后移动
                tail = tail.next; 
            }
            // 如果是10 那么取1 这个1就是进位数
            carry = sum / 10;
            // l1 与 l2 两个指针同时向后移动
            if(l1 != null) {
                l1 = l1.next;
            }
            if(l2 != null) {
                l2 = l2.next;
            }
        }
        // 判断进位数是否大于0如果大于就在尾指针的下一个里面赋值
        if(carry > 0) {
            tail.next = new ListNode(carry);
        }
        // 返回链表
        return head;
    }
}
```

