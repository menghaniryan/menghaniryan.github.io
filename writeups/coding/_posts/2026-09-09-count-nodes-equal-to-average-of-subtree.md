---
layout: post
title:  "Count Nodes Equal to Average of Subtree"
---

## Problem Statement

This problem comes from [LeetCode](https://leetcode.com/problems/count-nodes-equal-to-average-of-subtree).

## The Approach

This is a really straightforward problem. We define a recursive function that evaluates the sum and count of all subnodes of a given node. Then, once we grab the sum and count, we can perform evaluation on our current node, by calculating the average as `sum_left_subnodes + sum_right_subnodes + self_val / count_left_subnodes + count_right_subnodes + 1 == self_val`. If it's equal, increment some global answer variable.

## Time and Space Complexity

1. We are traversing each node exactly once, so time complexity is O(N) where N is the number of nodes.
2. We are performing at max N recursive calls, each recursive call requires stack space to propagate back up the chain, therefore space complexity is also O(N).

## The code

The code can be cleaned up a bit -- I think we actually don't need to handle each left only, right only, and both subtree nodes indepedently if we define correct default values for our `node_metadata` struct and account for null values. But I was too lazy to do this.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:

    struct node_metadata {
        int count_nodes = 0;
        int sum = 0;
    };

    int averageOfSubtree(TreeNode* root) {
        int ans = 0;
        getAverage(root, ans);
        return ans;
    }

    node_metadata getAverage(TreeNode* node, int& global_count) {
        if (node->left == nullptr && node->right == nullptr) {
            // Leaf case, the leaf always contributes to the average, so increment global_count and return the metadata.
            global_count++;
            return {.count_nodes = 1, .sum=node->val};
        }

        // Left
        if (node->left != nullptr && node->right == nullptr) {
            node_metadata left = getAverage(node->left, global_count);
            if ((left.sum + node->val) / (left.count_nodes + 1) == node->val) {
                global_count++;
            }
            return {.count_nodes = left.count_nodes + 1, .sum = left.sum + node->val};
        }

        // Right
        if (node->left == nullptr && node->right != nullptr) {
            node_metadata right = getAverage(node->right, global_count);
            if ((right.sum + node->val) / (right.count_nodes + 1) == node->val) {
                global_count++;
            }
            return {.count_nodes = right.count_nodes + 1, .sum = right.sum + node->val};
        }

        // Both
        node_metadata left = getAverage(node->left, global_count);
        node_metadata right = getAverage(node->right, global_count);
        if ((left.sum + right.sum + node->val) / (left.count_nodes + right.count_nodes + 1) == node->val) {
            global_count++;
        }
        return {.count_nodes = left.count_nodes + right.count_nodes + 1, .sum = left.sum + right.sum + node->val};
    }
};
```
