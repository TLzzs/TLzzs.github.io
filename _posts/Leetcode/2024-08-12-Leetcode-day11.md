---
title: "Leetcode Day 11 | Binary Tree Preorder/Inorder/Postorder Traversal"
date: 2024-08-11 00:00:00
categories: [Leetcode, BinaryTree]
tags: [BinaryTree]
---
## 144. Binary Tree Preorder Traversal
**LeetCode Problem:** [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/?envType=problem-list-v2&envId=mzt8n1h6)

## Approach and Thought Process
There are 2 approach **Recursive** and **Iteratively**

### Recursive
```java
class Solution {
    List<Integer> ans = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        preOrder(root);
        return ans;
    }

    public void preOrder(TreeNode node) {
        if (node == null) return;
        ans.add(node.val);
        preOrder(node.left);
        preOrder(node.right);
    }
}
```

### Iteratively
![](https://code-thinking.cdn.bcebos.com/gifs/%E4%BA%8C%E5%8F%89%E6%A0%91%E5%89%8D%E5%BA%8F%E9%81%8D%E5%8E%86%EF%BC%88%E8%BF%AD%E4%BB%A3%E6%B3%95%EF%BC%89.gif)
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            TreeNode node = stack.pop();
            result.add(node.val);
            if (node.right != null){
                stack.push(node.right);
            }
            if (node.left != null){
                stack.push(node.left);
            }
        }
        return result;
    }
}
```


## 94. Binary Tree Inorder Traversal
**LeetCode Problem:** [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/?envType=problem-list-v2&envId=mzt8n1h6)

## Approach and Thought Process
There are 2 approach **Recursive** and **Iteratively**

### Recursive

```java
class Solution {
  List<Integer> ans = new ArrayList<>();
  public List<Integer> inorderTraversal(TreeNode root) {
    inOrder(root);
    return ans;
  }

  public void inOrder(TreeNode node) {
    if (node == null) return;
    inOrder(node.left);
    ans.add(node.val);
    inOrder(node.right);
  }
}
```
### Iteratively
![](https://code-thinking.cdn.bcebos.com/gifs/%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%AD%E5%BA%8F%E9%81%8D%E5%8E%86%EF%BC%88%E8%BF%AD%E4%BB%A3%E6%B3%95%EF%BC%89.gif)

```java
class Solution {
  public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null){
      return result;
    }
    Stack<TreeNode> stack = new Stack<>();
    TreeNode cur = root;
    while (cur != null || !stack.isEmpty()){
      if (cur != null){
        stack.push(cur);
        cur = cur.left;
      }else{
        cur = stack.pop();
        result.add(cur.val);
        cur = cur.right;
      }
    }
    return result;
  }
}
```


## 145. Binary Tree Postorder Traversal
**LeetCode Problem:** [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/?envType=problem-list-v2&envId=mzt8n1h6)

## Approach and Thought Process
There are 2 approach **Recursive** and **Iteratively**

### Recursive

```java
class Solution {
  List<Integer> ans = new ArrayList<>();
  public List<Integer> postorderTraversal(TreeNode root) {
    postOrder(root);
    return ans;
  }

  public void postOrder(TreeNode node) {
    if (node == null) return;
    postOrder(node.left);
    postOrder(node.right);
    ans.add(node.val);
  }
}
```
### Iteratively
PreOrder: mid -> left -> right
PostOrder: left -> right -> mid

when we do traverse for preorder : mid -> right -> left, then reverse.

```java
class Solution {
  public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null){
      return result;
    }
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    while (!stack.isEmpty()){
      TreeNode node = stack.pop();
      result.add(node.val);
      if (node.left != null){
        stack.push(node.left);
      }
      if (node.right != null){
        stack.push(node.right);
      }
    }
    Collections.reverse(result);
    return result;
  }
}
```
