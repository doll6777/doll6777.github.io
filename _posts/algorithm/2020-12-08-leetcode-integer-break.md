---
layout: post
title: (DP) Integer Break 
tags: [algorithm, programming, leetcode]
category : algorithm
---

만약 N이 5라고 할때, 더해서 5가 될 수 있는 경우의 수는 0+5, 2+3, 3+2, 4+1, 5+0, 1+2+2 등 많은 경우의 수가 존재한다.  

더해서 N이 되는 숫자들을 곱했을 때, 최대 곱의 값을 구하시오.  

이 문제는 처음에 완전탐색으로 접근하였으나 시간 초과가 났다. 따라서 DP로 접근해야 한다.  

문제를 추상화 했을 때 모든 부분 문제는
i는 0부터 N까지라고 하고
N=5인 수는 Max(더해서 (N-i)가되는 곱의 최대 수)*i로 정의된다.  

베이스 케이스는 (N-i)가 0보다 작아지는 경우,  
0인 경우는 1로 리턴한다 (이 이유는 아래 설명하였다) 
i가 1인 경우는 1이다.

이 부분 문제를 DP 배열에 담고 메모이제이션 방법으로 캐싱해서 가져다 쓰면 된다.  

이 때 주의해야 할 사항은, i가 0이 되는 경우 무한루프에 빠질 수 있다. 또한 i가 0이 되는 경우는 어차피 곱해서 0이기 때문에 고려하지 않아도 된다.  

그리고 i가 N인 경우는 마찬가지로 곱해서 0이 되겠지만, 부분 문제에서는 (5+0)을 0으로 처리하지 않고 (5)로 처리해야 할 필요성이 있다. 왜냐하면 수를 나누지 않고 그대로 사용해야 할 수도 있기 때문이다.  

하지만 i가 N(초기값)인 경우는 1로 처리하면 안된다. 이 때는 1로 처리하면 N 자체가 나와버리는데, 부분문제 말고는 i가 N인경우엔 곱이 0이기 때문이다.  


<pre class="prettyprint">
class Solution {

    int dp[];


    public int integerBreak(int n) {
        dp = new int[n + 1];
        return breaker(n, n);
    }

    public int breaker(int n, int real) {
        if (n &lt; 0) {
            return Integer.MIN_VALUE;
        }
        if (n == 0) {
            return 1;
        }
        if (n == 1) {
            return 1;
        }
        if (dp[n] != 0) {
            return dp[n];
        }

        int maxValue = Integer.MIN_VALUE;
        for (int i = 1; i &lt;= n; i++) {
            if(i == real) continue;
            maxValue = Math.max(breaker(n - i, real) * i, maxValue);
        }

        dp[n] = maxValue;
        return maxValue;
    }
}
</pre>