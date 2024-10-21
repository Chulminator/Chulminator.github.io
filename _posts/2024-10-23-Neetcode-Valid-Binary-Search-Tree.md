---
title: NeetCode - Valid Binary Search Tree
author: chulmin
date: 2024-10-23 00:00:00 -0500
categories: [Daily Smashing Through Random Problems, Neetcode, Valid Binary Search Tree]
tags: [coding test, c++, neetcode]
math: true
---

## Code
``` c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return isValidBST(root, INT_MIN, INT_MAX);
    }
private:
    bool isValidBST(TreeNode* root, int minVal, int maxVal) {
        if (root == nullptr) {
            return true; 
        }
        if (root->val <= minVal || root->val >= maxVal) {
            return false;
        }
        if( !isValidBST(root->left, minVal, root->val) ){
            return false;
        }
        if( !isValidBST(root->right, root->val, maxVal) ){
            return false;
        }
        return true;
    }
};
```

## Reference
- [Valid Binary Search Tree](https://neetcode.io/problems/valid-binary-search-tree)