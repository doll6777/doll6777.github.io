---
layout: post
title: (DP) Coin Change 2
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/coin-change-2/>

거스름돈을 거슬러주는 모든 경우의 수를 구하는 문제이다. Coin Change 1과 비슷하지만, Coin Change는 거스름 돈을 최소로 할수있는 개수를 구했다면 2는 모든 경우의 수를 구한다.  

먼저, Top-down 접근방식으로 이 문제또한 접근할 수 있는데, 이 문제는 유명한 0-1 knapsack 문제 풀이방법과 동일하게 풀 수 있으니 패턴을 기억해두는것이 좋다.  

<pre class="prettyprint">
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length + 1][amount + 1];

        for (int i = 0; i &lt;= amount; i++) {
            dp[0][i] = 0;
        }
        for (int i = 0; i &lt;= coins.length; i++) {
            dp[i][0] = 1;
        }

        for (int i = 1; i &lt;= amount; i++) {
            for (int j = 1; j &lt;= coins.length; j++) {
                if (i &gt;= coins[j-1]) {
                    dp[j][i] += dp[j][i - coins[j-1]];
                }
                dp[j][i] += dp[j - 1][i];
            }
        }
        return dp[coins.length][amount];
    }
}
</pre>
