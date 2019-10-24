---
layout: post
title: (Tree) Subsets
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/problems/subsets/

<pre class="prettyprint">
class Solution {

    List<List<Integer>> result = new ArrayList<>();

    private void subsets(int[] nums, boolean[] flag, int n) {

        if (n == nums.length) {
            List<Integer> list = new ArrayList<>();

            for (int f = 0; f < flag.length; f++) {
                if (flag[f]) {
                    list.add(nums[f]);
                }
            }
            result.add(list);
            return;
        }

        flag[n] = false;
        subsets(nums, flag, n + 1);

        flag[n] = true;
        subsets(nums, flag, n + 1);

    }

    public List<List<Integer>> subsets(int[] nums) {

        boolean[] flag = new boolean[nums.length];
        subsets(nums, flag, 0);

        return result;
    }
}
</pre>
