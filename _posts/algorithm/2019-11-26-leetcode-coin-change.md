---
layout: post
title: (DP) Coin Change
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/coin-change/>

주어진 동전이 있을때, 최소한의 동전을 사용하였을 때 거스름돈을 줄 수 있는 개수를 구하는 문제이다.  

모든 경우의 수를 탐색하여 완전탐색으로 구할 수 있지만, 타임아웃이 난다.  
따라서 DP를 사용하여 Bottom-up으로 접근해야 한다.  

<pre class="prettyprint">
class Solution {
    public int coinChange(int[] coins, int amount) {
        int max = amount + 1;
        int[] dp = new int[amount + 1];

        dp[0] = 0;
        for (int i = 1; i &lt;= amount; i++) {

            int minValue = max;
            for (int j = 0; j &lt; coins.length; j++) {
                if (coins[j] &lt;= i) {
                    if (minValue &gt; (dp[i - coins[j]] + 1)) {
                        minValue = (dp[i - coins[j]] + 1);
                    }
                }

            }
            dp[i] = minValue;
        }

        return dp[amount] &gt; amount ? -1 : dp[amount];
    }
}
</pre>
