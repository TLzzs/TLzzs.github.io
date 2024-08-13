---
title: "Leetcode Day 12 | 226. Invert Binary Tree、101. Symmetric Tree"
date: 2024-08-11 00:00:00
categories: [Leetcode, BinaryTree]
tags: [BinaryTree]
---
## 226. Invert Binary Tree
**LeetCode Problem:** [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/?envType=problem-list-v2&envId=mzt8n1h6)

### Approach and Thought Process
There are 2 approach **Recursive** and **Iteratively**

### Recursive
```java
class Solution {
  public TreeNode invertTree(TreeNode root) {
    if (root == null || (root.left == null && root.right == null)) return root;
    invertTrees(root);
    return root;
  }

  void invertTrees(TreeNode root) {
    TreeNode tmp = root.left;
    root.left = root.right;
    root.right = tmp;
    invertTree(root.left);
    invertTree(root.right);
  }
}
```


## 101. Symmetric Tree
**LeetCode Problem:** [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/?envType=problem-list-v2&envId=mzt8n1h6)

### Approach and Thought Process
There are 2 approach **Recursive** and **Iteratively**

### Recursive
```java
class Solution {
  public boolean isSymmetric(TreeNode root) {
    if (root == null) return true;

    return isSymetric(root.left, root.right);
  }

  boolean isSymetric(TreeNode left, TreeNode right) {
    if (left == null && right == null) return true;
    if (left == null || right == null) return false;

    if (left.val != right.val) return false;

    return isSymetric(left.left, right.right) && isSymetric(left.right, right.left);
  }
}
```

