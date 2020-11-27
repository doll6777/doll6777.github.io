---
layout: post
title: (DP) Longest palindrome Subsequence 
tags: [algorithm, programming, leetcode]
category : algorithm
---

DP로 푸는 가장 긴 펠린드롬 문자열 찾기. 중간에 문자열이 끊어져도 상관없다.  
2차원 배열로 했을 때 문자열의 길이가 1이 된다면 최대 길이는 1
문자열의 길이가 2가 된다면 두 문자가 같은 문자일 경우 2, 다른 문자일 경우 1
문자열의 길이가 3이 된다면 처음과 끝의 문자가 같은 문자일 경우 그 안에 들어있는 문자열의 최대 펠린드롬의 길이 + 2가 될것이다.  

처음과 끝의 문자가 다르다면 전체가 펠린드롬은 아니기 때문에, 첫 번째 문자를 포함하고 끝 문자를 포함하지 않은것과 반대로 마지막 문자를 포함하고 첫 번째 문자를 포함하지 않은 것의 subsequence 의 최대 길이 중 더 큰것을 사용하면 된다.  

이 문제는 2차원 배열을 순회할 때의 순서가 중요했다. 대각선 부터 시작해서 순회를 해야 하기 때문에 순차적으로 0부터 size까지 순회하며 채우면 안된다. 순서는 아래 코드에서 확인할 수 있다.

<pre class="prettyprint">
class Solution {
    public int longestPalindromeSubseq(String s) {
        int dp[][] = new int[s.length()][s.length()];

        for (int i = 0; i &lt; s.length(); i++) {
            for (int j = 0; j &lt; s.length(); j++) {
                if (i == j) {
                    dp[i][j] = 1;
                }
            }
        }
        for (int i = 0; i &lt; s.length(); i++) {
            for (int j = 0; j &lt; s.length() - i; j++) {
                int x = j + i;
                int y = j;
                if(x == y) continue;

                if (s.charAt(y) == s.charAt(x)) {
                    if ((y + 1) &lt; s.length()) {
                        dp[y][x] = dp[y + 1][x - 1] + 2;
                    }
                } else {
                    if ((y + 1) &lt; s.length()) {
                        dp[y][x] = Math.max(dp[y + 1][x], dp[y][x - 1]);
                    }
                }
            }
        }

        return dp[0][s.length() - 1];
    }
}
</pre>


<pre class="prettyprint">
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n=s.length();
        int dp[n+1][n+1];
        memset(dp,0,sizeof(dp));

        for(int i=0;i&amp;lt;n;i++)
            dp[i][i]=1;
        
        for(int l=1;l&amp;lt;=n;l++)
            for(int j=0;j&amp;lt;n-l+1;j++)
                dp[j][j+l]=s[j]==s[j+l]?2+dp[j+1][j+l-1]:max(dp[j][j+l-1],dp[j+1][j+l]);
        return dp[0][n];
    }
};
</pre>
