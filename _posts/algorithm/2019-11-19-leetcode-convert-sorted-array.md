---
layout: post
title: (Tree) Convert Sorted Array to Binary Search Tree
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/>

정렬된 배열을 이진 탐색 트리로 변환한다.  
정렬된 배열에서 가운데 값을 뽑고 머지소트 원리를 사용하여 왼쪽 부분어레이, 오른쪽 부분어레이에서도 가운데를 뽑아서 왼쪽자식과 오른쪽 자식으로 연결하면 된다.  



<pre class="prettyprint">
class Solution {

    private TreeNode traverse(int[] nums, int l, int r) {
        if(r &lt; l) {
            return null;
        }

        int mid = l + (r - l) / 2;

        TreeNode midNode = new TreeNode(nums[mid]);

        midNode.left = traverse(nums, l, mid - 1);
        midNode.right = traverse(nums, mid + 1, r);

        return midNode;
    }

    public TreeNode sortedArrayToBST(int[] nums) {
        return traverse(nums, 0, nums.length - 1);
    }
}
</pre>