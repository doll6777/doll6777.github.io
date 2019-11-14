---
layout: post
title: (Tree) Diameter of Binary Tree
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/diameter-of-binary-tree/>

두 노드 사이의 길이가 가장 긴 길이를 찾는 것이다.  
일반적으로 그래프를 루트부터 탐색하는 방법과는 조금 다르다.  

모든 노드를 순회하며, 왼쪽 서브트리의 최대뎁스와 오른쪽 서브트리의 최대뎁스를 합친것의 최대뎁스를 구하면 된다.  

<pre class="prettyprint">
class Solution {

    int maxDepth = Integer.MIN_VALUE;

    private int getDepth(TreeNode node, int depth) {
        if (node == null) {
            return depth;
        }
        return Math.max(getDepth(node.left, depth + 1), getDepth(node.right, depth + 1));
    }

    private void getMaxDepth(TreeNode node) {
        if (node == null) {
            return;
        }

        int left = getDepth(node.left, 0);
        int right = getDepth(node.right, 0);
        int sum = left + right;

        if (maxDepth &lt; sum) {
            maxDepth = sum;
        }

        getMaxDepth(node.left);
        getMaxDepth(node.right);
    }

    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) return 0;
        if (root.left == null &amp;&amp; root.right == null) return 0;
        getMaxDepth(root);
        return maxDepth;
    }
}
</pre>

아래처럼 개선 가능하다.

<pre class="prettyprint">
class Solution {
    private int max = 1;
    public int diameterOfBinaryTree(TreeNode root) {
        helper(root);
        return max - 1;
    }
    
    private int helper(TreeNode root) {
        if (root == null) return 0;
        
        int left = helper(root.left);
        int right = helper(root.right);
        
        max= Math.max(max, 1 + left + right);
        return 1 + Math.max(left, right);
    }
}
</pre>