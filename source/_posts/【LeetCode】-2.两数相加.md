---
title: 【LeetCode】-2.两数相加
date: 2022-08-10 13:31:40
tags:
 Leetcode Hot 100, 链表
categories:
 算法

---

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。



**示例 1：**

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



**题解：**

由于输入的两个链表都是 **逆序** 存储数字的位数的，因此两个链表中同一位置的数字可以直接相加。

我们同时遍历两个链表，逐位计算它们的和，并与当前位置的进位值相加。具体而言，如果当前两个链表处相应位置的数字为  `n1` , `n2` ，进位值为 `carry`，则它们的和为 `n1`+`n2`+`carry`；其中，答案链表处相应位置的数字为 (`n1`+`n2`+`carry`) **mod** `10`，而新的进位值为 ⌊ (`n1`+`n2`+`carry`) / `10`⌋。

如果两个链表的长度不同，则可以认为长度短的链表的后面有若干个 0 。

此外，如果链表遍历结束后，有 `carry`>0，还需要在答案链表的后面附加一个节点，节点的值为 `carry`。

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
        int  x = 0, y = 0, digit = 0;
        ListNode pre = new ListNode();
        ListNode node = new ListNode();
        pre.next = node;
        while(l1!=null||l2!=null){
            if(l1!=null){
                x = l1.val;
                l1 = l1.next;
            }
            else x = 0;
            if(l2!=null){
                y = l2.val;
                l2 = l2.next;
            }
            else y = 0;
            ListNode temp = new ListNode();
            node.next = temp;
            node = node.next;
            node.val = (x + y + digit)%10;
            digit = (x + y + digit)/10;
        }
        if(digit!=0){
            ListNode temp = new ListNode();
            temp.val = digit;
            node.next = temp;
        }
        return pre.next.next;
    }
}
```

