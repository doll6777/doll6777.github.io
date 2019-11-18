---
layout: post
title: (Math) Count primes
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/count-primes/>

해당하는 수보다 작은 수들중에서 소수의 개수 구하는 문제이다.  
에라토스테네스의 체를 사용해서 구현할 수 있다.  

<pre class="prettyprint">
class Solution {
    
    boolean[] isNotPrimes;
        
    public int countPrimes(int n) {
        isNotPrimes = new boolean[n+1];
        int count = 0;
        
        for(int i=2; i<=n; i++) {
            if(isNotPrimes[i]) {
                continue;
            }
            for(int j=2; j<=n/i; j++) {
                isNotPrimes[i*j] = true;
            }    
        }
        
        for(int i=2; i<n; i++) {
            if(!isNotPrimes[i])
            {
                count++;
            }            
        }
        return count;
    }
}
</pre>
