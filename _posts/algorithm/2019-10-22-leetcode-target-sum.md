---
layout: post
title: (DP) Target Sum
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/problems/target-sum/solution/

# Solution 1: Brute Force
<pre class="prettyprint">
class Solution {

    private int result = 0;

    private void findTarget(int[] nums, int sum, int S, int n) {

        if (((nums.length) == n)) {
            if (sum == S) {
                result++;
            }
            return;
        }

        findTarget(nums, S, sum + nums[n], n+1);
        findTarget(nums, S, sum - nums[n], n+1);
    }

    int findTargetSumWays(int[] nums, int S) {
        findTarget(nums, S, 0, 0);
        return result;
    }
}
</pre>

# Solution 2 : DP

<pre class="prettyprint">
public class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int[][] dp = new int[nums.length][2001];
        dp[0][nums[0] + 1000] = 1;
        dp[0][-nums[0] + 1000] += 1;
        for (int i = 1; i < nums.length; i++) {
            for (int sum = -1000; sum <= 1000; sum++) {
                if (dp[i - 1][sum + 1000] > 0) {
                    dp[i][sum + nums[i] + 1000] += dp[i - 1][sum + 1000];
                    dp[i][sum - nums[i] + 1000] += dp[i - 1][sum + 1000];
                }
            }
        }
        return S > 1000 ? 0 : dp[nums.length - 1][S + 1000];
    }
}
</pre>
