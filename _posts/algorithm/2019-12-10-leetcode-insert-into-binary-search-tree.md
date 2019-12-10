---
layout: post
title: (Tree) Insert into a Binary Search Tree
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/insert-into-a-binary-search-tree/>

값을 이진탐색트리에 삽입한다. 이진탐색트리를 탐색하는 것처럼 하다가 삽입해야할 위치까지 오면 값을 넣고 리턴한다. 이때 부모에 자식을 연결해주는 것까지 해야한다.

<pre class="prettyprint">
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

class Solution {

    private TreeNode insert(TreeNode node, int val) {
        if (node == null) {
            return new TreeNode(val);
        }

        if (node.val > val) {
            node.left = insert(node.left, val);
        }
        if (node.val < val) {
            node.right = insert(node.right, val);
        }

        return node;
    }

    public TreeNode insertIntoBST(TreeNode root, int val) {
        insert(root, val);
        return root;
    }
}
</pre>
