---
layout: post
title: (Math) Sqrt(x)
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/sqrtx>

Sqrt(x)를 구하는 문제이다. 기본 Math 라이브러리에서 제공하기 때문에 쉽게 구현할 수 있다.

<pre class="prettyprint">
class Solution {
    public int mySqrt(int x) {
        
        return (int)Math.sqrt((double)x);                 
    }
}
</pre>

하지만 문제 출제 의도는 그게 아닐것이다. 따라서 이렇게 구현할 수 있다.

<pre class="prettyprint">
class Solution {
    public int mySqrt(int x) {
        
        for (int i=1; i&lt;=x; i++) {
            int ii = i*i;
            if(ii == x) {
                return i;
            }
            if(ii &gt; x) {
                return i-1;
            }        
        }
        return 0;
    }
}
</pre>

하지만 이는 숫자가 매우 커질경우 타임리밋을 유발한다. 따라서 바이너리 서치를 사용하면 시간복잡도를 줄일 수 있다.

KMP 알고리즘을 사용해서 구현하면 훨씬 빨라진다. KMP 소스는 아래를 참고하면 된다.

<pre class="prettyprint">
class Solution {
    public int mySqrt(int x) {
        if(x &lt; 2){
            return x;
        }
        long l = 1L;
        long r = x / 2;
        long res = l;
        while(l &lt;= r){
            long mid = (l + r) / 2;
            if(mid * mid == x){
                return (int)mid;
            }
            if(mid * mid &gt; x){
                r = mid - 1;
            }else{
                res = mid;
                l = mid + 1;
            }
        }
        return (int)res;
    }
}
</pre>
