---
layout: post
title: (DP) House Robber
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/problems/house-robber/
이웃하지 않는 이웃을 털었을 때 가장 많이 훔칠 수 있는 돈은 얼마인가? 라는 문제이다.
DP 문제를 풀때는 해결하고자 하는 문제를 명확하게 두개로 쪼개야한다. 또한, 그 쪼갠 문제의 해답을
이전 상태를 활용하여 구할 수 있어야 한다.

이 문제의 경우 현재 상태는 현재 집을 턴다 / 현재 집을 털지 않는다로 구분할 수 있다.
또한 현재 집을 터는 경우엔 이전 집의 상태는 털지 않는 상태일 것이고,
현재 집을 털지 않는 경우엔 이전 집을 털거나 / 털지 않거나 두가지 상태로 나눠진다. 

이를 식으로 정리해 보면,
집을 턴다 / 털지 않는다는 i (0,1)
집의 번호는 j (0..n-1)

원소가 하나일 경우 터는 경우 / 털지 않는 경우는 집하나만 턴 값/ 0 둘로 정의된다.

이제 원소가 하나인 경우를 정의했으니, 두개인 경우부터 bottom-up으로 계산한다.

i = 0인 경우는 현재 안터는 경우이므로 Max(이전에 안턴경우 최대 돈, 이전에 턴 경우의 최대 돈)
i = 1인 경우는 현재 터는 경우이므로 (이전에 안턴 경우 최대 돈)

이런 식으로 계산하고, 맨 마지막 원소를 고르냐 안고르냐 둘중 최대값으로 리턴하면 된다.

<pre class="prettyprint">
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }

        int dp[][] = new int[2][nums.length];
        dp[0][0] = 0;
        dp[1][0] = nums[0];

        for (int j = 1; j < nums.length; j++) {
            for (int i = 0; i < 2; i++) {
                if (i == 0) {

                    dp[i][j] = Math.max(dp[i + 1][j - 1], dp[i][j - 1]);
                } else {
                    dp[i][j] = dp[i - 1][j - 1] + nums[j];
                }
            }
        }

        return Math.max(dp[0][nums.length - 1], dp[1][nums.length - 1]);
    }
}
</pre>