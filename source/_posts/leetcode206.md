---
title: leetcode206反转链表
date: 2020-02-02 20:54:52
tags:
    - leetcode
    - 链表
categories:
    - 算法
---
# 题目
反转一个单链表。

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

---

# 解法1：使用递归
## 思路
递归，需要确定每一次需要做的事情，并找到结束的条件

每一次递归需要确定的是2个节点，最后返回的节点 p，以及当前需要进行反转的节点
我们要做的一轮递归只是 将当前节点加入到反转链表中，仅此而已
假设

$$
n_1\rightarrow n_2\rightarrow ...\rightarrow n_{k-1}\rightarrow n_k\rightarrow n_{k+1}\rightarrow...n_m
$$
已经变成了
$$
n_1\rightarrow n_2\rightarrow...\rightarrow n_{k-1}\rightarrow n_k\rightarrow n_{k+1}\leftarrow...n_m
$$
我们希望的是$$n_{k+1}$$的下一个节点指向$$n_k$$

## 实现代码
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        // 节点 p 其实就是反转链表的头节点 
        ListNode p = reverseList(head.next);
        // 我们将反转链表的尾结点（head.next）的 next 指向当前即将反转的节点
        head.next.next = head;
        // 然后让当前节点变成反转链表的尾结点
        head.next = null;
        return p;
    }
}
```

# 解法2：使用迭代
## 思路
假设

$$
n_1\rightarrow n_2\rightarrow ...\rightarrow n_{k-1}\rightarrow n_k\rightarrow n_{k+1}\rightarrow...n_m
$$
的前k-1个已完成反转，现在反转节点$$ n_k $$
$$
n_1\leftarrow n_2\leftarrow...\leftarrow n_{k-1} \quad n_k\rightarrow n_{k+1}\rightarrow...n_m
$$
我们希望的是$$n_k$$的下一个节点指向$$n_{k-1}$$

## 实现代码
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(null == head || head.next == null){
            return head;
        }
        ListNode newStart = new ListNode(-1);
        ListNode preNode = null;
        while(head != null ){
            newStart = head.next;
            head.next = preNode;
            preNode = head;
            head = newStart;
        }
        return preNode;
    }
}
```