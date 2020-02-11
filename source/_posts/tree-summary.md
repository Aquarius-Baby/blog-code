---
title: 二叉树题解
date: 2020-02-10 15:18:03
tags:
    - 二叉树
categories:
    - 算法
---

二叉树题解整理

判断深度

<!--more-->

```java
// leetcode 104 二叉树的最大深度
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        int maxLeft = maxDepth(root.left);
        int maxRight = maxDepth(root.right);
        return maxLeft > maxRight? maxLeft + 1 : maxRight +1;
    }
}
```

```java

```