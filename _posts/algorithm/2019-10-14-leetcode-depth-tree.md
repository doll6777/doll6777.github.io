---
layout: post
title: (Tree) Maximum Depth of Binary Tree

tags: [algorithm, programming, leetcode]
category : algorithm
---

바이너리 트리가 있을때, 트리의 깊이를 구하라.  

> https://leetcode.com/explore/interview/card/top-interview-questions-easy/94/trees/555/
<pre>
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        int max = (left>right)? left: right;
        
        return max+1;
    }
}
</pre>

해설 영상
> https://www.youtube.com/watch?v=D2cFSDfg0So

예를 들어 영화관에 앉아 있다고 생각할 때, 몇 열인지 어떻게 알 수 있을까?  
Recursion을 사용하여 앞에 사람에게 첫째 열인지 계속 물어본다.  
첫째 열이 아닌 경우엔 계속하여 열의 개수를 카운트한다. 이 때 첫째 열일 때 반복을 중단하는 것을 Base Condition이라고 한다.  

이 문제에서도 재귀를 종료할 수 있는 기본 조건을 정의하고, 다음에 물어볼 곳인 왼족 자식과 오른쪽 자식에게 뎁스가 무엇인지 물어보고, 그것을 알았다면 1을 더해서 리턴하였다.  