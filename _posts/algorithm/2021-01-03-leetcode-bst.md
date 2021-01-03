---
layout: post
title: (BST) Construct Binary Search Tree from Preorder Traversal
tags: [algorithm, programming, leetcode]
category : algorithm
---

preorder로 주어진 트리 배열이 있다. 주어진 배열을 가지고  
BST(Binary Search Tree)를 만들어라.  

이진 탐색트리란, 모든 루트보다 왼쪽에 있는 원소들은 루트보다 값이 작고,
오른쪽에 있는 원소들은 루트보다 값이 크다.  

이 점을 사용하고 주어진 배열이 어차피 preorder이기 때문에 루트부터 depth가 커지며 아래로 탐색하는 모양이다.  

따라서 하나씩 순회하며 맨 첫번째 값을 루트로 두고, 그다음 값이 루트보다 작은지 큰지를 이용하여 있어야 할 위치에 원소를 위치시킨다.  


<pre class="prettyprint">
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    public TreeNode makeBST(int elem, TreeNode node) {
        if(node == null) {
            TreeNode newNode = new TreeNode(elem);
            return newNode;
        }
        if(node.val == elem) {
            return node;
        }
        
        TreeNode left, right;
        left = right = null;
        
        if(node.val &#x3E; elem) {
            left = makeBST(elem, node.left);
        } else {
            right = makeBST(elem, node.right);
        }        
        if(left == null) {
            node.right = right;
        } else {
            node.left = left;
        }
        return node;
    }
    
    public TreeNode bstFromPreorder(int[] preorder) {
        TreeNode root = new TreeNode(preorder[0]);
        
        for(int i=0; i&#x3C; preorder.length; i++) {
            makeBST(preorder[i], root);
        }
        
        return root;
    }
}
</pre>