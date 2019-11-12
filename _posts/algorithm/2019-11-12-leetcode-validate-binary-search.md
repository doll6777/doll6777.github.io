---
layout: post
title: (Tree) Validate Binary Search Tree
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/validate-binary-search-tree>

주어진 트리가 이진 탐색 트리가 맞는지 검증하는 문제이다.  

이진 검색 트리란, 한 노드의 왼쪽 서브트리가 모두 부모보다 작은 노드로 이루어져 있고, 한 노드의 오른쪽 서브트리는 모두 부모보다 큰 노드로 이루어져 있는 트리를 말한다.  

처음엔 무조건 왼쪽자식, 오른쪽 자식만 검사했었는데 이렇게 하면 조상으로부터 lowerBound, upperBound의 제약을 받기 때문에 이 문제를 풀 수 없다.  

따라서 lowerBound와 upperBound를 바꾸어 나가며 탐색해야 한다.  

이 문제를 풀 때 처음엔 바운드의 초기값을 Integer.MAX Integer.MIN 으로 하였더니 테스트케이스에 이 값이 들어있을 경우에 패스할 수 없었다.  

따라서 Long.MAX Long.MIN으로 대체하였다.

<pre class="prettyprint">
class Solution {

    private boolean isValidBST(TreeNode node, long upperBound, long lowerBound) {
        if (node == null) {
            return true;
        }

        if (upperBound <= node.val) {
            return false;
        } else if (lowerBound >= node.val) {
            return false;
        }

        return isValidBST(node.left, node.val, lowerBound) && isValidBST(node.right, upperBound, node.val);
    }

    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        if (root.left == null && root.right == null) {
            return true;
        }
        return isValidBST(root, Long.MAX_VALUE, Long.MIN_VALUE);
    }
}

</pre>

MIN, MAX 값 없이 푼 사람의 풀이

<pre class="prettyprint">
class Solution {
    public boolean isValidBST(TreeNode root) {
        
        ArrayList<TreeNode> list = new ArrayList<>();
        ArrayList<TreeNode> check = new ArrayList<>();
        boolean valid = true;
        
        check = inorder(root, list);
        
        for(int i = 0; i < check.size() - 1; i++)
        {
            if(check.get(i + 1).val <= check.get(i).val)
                valid = false;
        }
        
        return valid;
    }
    
    public ArrayList<TreeNode> inorder(TreeNode root, ArrayList<TreeNode> list)
    {
        if(root == null)
            return new ArrayList<TreeNode>();
        
        inorder(root.left, list);
        list.add(root);
        inorder(root.right, list);
        
        return list;
    }
}
</pre>
