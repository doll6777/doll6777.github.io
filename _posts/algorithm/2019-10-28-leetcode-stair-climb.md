---
layout: post
title: (DP) Climbing Stairs
tags: [algorithm, programming, leetcode]
category : algorithm
---

정답을 단순화해서 쪼갤 수 없는지 생각해보면,  
계단을 2번 또는 1번 오를 수 있으니, n이 3이상이라면, 모든 케이스에 계단을 2번 오르고 나머지 계단을 오르는것과,  계단을 1번 오르고 나머지 계단을 오르는 경우가 존재한다.  
따라서 이를 점화식으로 세워보면,  An = An-1 + An-2 이므로, 피보나치 수열의 값을 구하면 된다.

<pre class="prettyprint">
class Solution {
    public int climbStairs(int n) {
        if(n == 1) {
            return 1;
        }
        else if(n == 2) {
            return 2;
        }

        int[] dp = new int[n];

        dp[0] = 1;
        dp[1] = 2;

        for(int i=2; i<n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n-1];
    }
}
</pre>