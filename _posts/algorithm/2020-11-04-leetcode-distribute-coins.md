---
layout: post
title: (Traverse) Distribute Coins
tags: [algorithm, programming, leetcode]
category : algorithm
---

https://leetcode.com/problems/distribute-coins-in-binary-tree/

이진 트리에 동전들이 임의로 들어가 있을때, 모든 노드에 동전이 1개씩만 들어가 있도록 옮겨야 한다. 이때 옮겨야 하는 횟수를 구하시오.  

DFS로 풀 수 있는 문제다. 노드 단위로 생각했을 때 노드의 left, right 하위 노드에서 옮겨진 동전개수를 구해서 더해주고, 자신의 노드에 들어갈 1을 빼고 나머지 동전을 위로 올려주며 탐색을 진행하면 된다.  

이때 옮겨야 하는 횟수의 합을 구해야 하는데 이는 하위의 left, right에서 올려줘야 할 동전들의 절대값의 합이라고 볼 수 있다.  


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
    int result = 0;
 
    public int traverse(TreeNode node) {
        if(node == null) return 0;
        int left = traverse(node.left);
        int right = traverse(node.right);
        result += Math.abs(left) + Math.abs(right);
        return node.val + left + right - 1;
    }
    
    public int distributeCoins(TreeNode root) {
        result = 0;
        traverse(root);
        return result;
    }
}
</pre>

