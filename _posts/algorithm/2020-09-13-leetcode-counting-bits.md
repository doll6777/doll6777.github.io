---
layout: post
title: (DP) Counting Bits
tags: [algorithm, programming, leetcode]
category : algorithm
---

<pre class="prettyprint">
class Solution {
    public int[] countBits(int num) {
        int[] result = new int[num+1];
        result[0] = 0;
        int index = 1;
        
        for(int i=1; i<= num; i++) {
            int count = 0;
            int temp = i;
            
            while(temp >= 1) {
                if(temp % 2 == 1) {
                    count++;
                }
                temp = temp / 2;
            }
            result[index++] = count;
        }
        return result;
    }
}
</pre>


with DP

<pre class="prettyprint">
class Solution {
    public int[] countBits(int num) {
        
        int[] dp = new int[num + 1];
        int ref = 1;
        dp[0] = 0;
        for(int i = 1; i <= num; i++) {
            if(ref == i) {
                ref <<= 1;
            }
            dp[i] = dp[i-(ref >> 1)] + 1;
        }
        
        return d;
    }
}

</pre>