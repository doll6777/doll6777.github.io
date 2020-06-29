---
layout: post
title: (BSearch) Minimum number of days to make m bouquets 
tags: [algorithm, programming, leetcode]
category : algorithm
---

https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/

<pre class="prettyprint">
public class Solution {
    // m : count, k : adjacent

    public int minDays(int[] A, int m, int k) {
        int n = A.length, left = 1, right = (int)1e9;
        if (m * k > n) return -1;

        while (left < right) {
            int mid = (left + right) / 2, flow = 0, bouq = 0;
            for (int j = 0; j < n; ++j) {
                if (A[j] > mid) {
                    flow = 0;
                } else if (++flow >= k) {  
                    bouq++;
                    flow = 0;
                }
            }
            if (bouq < m) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }
}
</pre>