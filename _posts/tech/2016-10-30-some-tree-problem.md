---
layout: post
title: 一些有关二叉树的递归算法（some recursion algorithm about binary tree）
category: 技术
tags: 算法
keywords: 二叉树
---
没啥说的，直接上代码吧

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <map>
#include <set>
using namespace std;

struct TreeNode {
    char data;
    TreeNode *left;
    TreeNode *right;
    TreeNode(char ch) {
        data = ch;
        left = NULL;
        right = NULL;
    }
};

// whether a is same as b in structure and values
bool isSame(TreeNode *a, TreeNode *b) {
    if (a == NULL && b == NULL) {
        return true;
    }
    if (a == NULL || b == NULL) {
        return false;
    }
    if (a->data != b->data) {
        return false;
    }
    return isSame(a->left, b->left) && isSame(a->right, b->right);
}

// Invert a tree
void invert(TreeNode *root) {
    if (root == NULL) {
        return;
    }
    if (root->left == NULL && root->right == NULL) {
        return;
    }
    TreeNode *tmp = root->left;
    root->left = root->right;
    root->right = tmp;
    invert(root->left);
    invert(root->right);
}

// Whether Tree a is a mirror of Tree b
bool isMirror(TreeNode *a, TreeNode *b) {
    if (a == NULL && b == NULL) {
        return true;
    }
    if (a == NULL || b == NULL) {
        return false;
    }
    if (a->data != b->data) {
        return false;
    }
    return isMirror(a->left, b->right) && isMirror(a->right, b->left);
}

// Whether a tree is symmetric
bool isSymmetric(TreeNode *root) {
    if (root == NULL) {
        return true;
    }
    return isMirror(root->left, root->right);
}

// Build a Tree By preOrder and InOrder
TreeNode* build(string preOrder, string inOrder) {
    if (preOrder.length() == 0) {
        return NULL;
    }
    
    char chroot = preOrder[0];
    int indexOfRootInOrder = (int) inOrder.find(chroot);
    string leftInOrder = inOrder.substr(0, indexOfRootInOrder);
    string rightInOrder = inOrder.substr(indexOfRootInOrder + 1);
    string leftPreOrder = preOrder.substr(1, leftInOrder.length());
    string rightPreOrder = preOrder.substr(leftInOrder.length() + 1);
    TreeNode *root = new TreeNode(chroot);
    root->left = build(leftPreOrder, leftInOrder);
    root->right = build(rightPreOrder, rightInOrder);
    return root;
}

int main() {
    string pre = "ABCDBCC";
    string in = "CBDACBC";
    TreeNode *root = build(pre, in);
    cout << isSymmetric(root) << endl;
    return 0;
}
```
