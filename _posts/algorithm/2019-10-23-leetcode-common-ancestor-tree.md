---
layout: post
title: (Tree) Lowest Common Ancestor of a Binary Tree
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

이진 트리에서 두 노드가 주어졌을 때, 가장 루트와 멀리 떨어져 있는 공통의 조상을 찾는 문제이다.  

이 문제는 두 노드의 거리를 계산할 때 유용하게 쓰인다.  

### Solution 1 : 트리 탐색하면서 찾기, O(n)

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

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return null;
        }
        if (root == p || root == q) {
            return root;
        }

        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if ((left != null && right != null)) {
            System.out.println("result: " + root.val);
            return root;
        }
        return (left != null) ? left : right;
    }
}
</pre>

### Solution 2 : 루트부터 해당 노드까지의 경로를 기억하고 경로 나열하여 공통조상 찾기 O(n)

<pre class="prettyprint">
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        // Stack for tree traversal
        Deque<TreeNode> stack = new ArrayDeque<>();

        // HashMap for parent pointers
        Map<TreeNode, TreeNode> parent = new HashMap<>();

        parent.put(root, null);
        stack.push(root);

        // Iterate until we find both the nodes p and q
        while (!parent.containsKey(p) || !parent.containsKey(q)) {

            TreeNode node = stack.pop();

            // While traversing the tree, keep saving the parent pointers.
            if (node.left != null) {
                parent.put(node.left, node);
                stack.push(node.left);
            }
            if (node.right != null) {
                parent.put(node.right, node);
                stack.push(node.right);
            }
        }

        // Ancestors set() for node p.
        Set<TreeNode> ancestors = new HashSet<>();

        // Process all ancestors for node p using parent pointers.
        while (p != null) {
            ancestors.add(p);
            p = parent.get(p);
        }

        // The first ancestor of q which appears in
        // p's ancestor set() is their lowest common ancestor.
        while (!ancestors.contains(q))
            q = parent.get(q);
        return q;
    }
}
</pre>
