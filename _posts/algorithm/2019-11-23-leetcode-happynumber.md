---
layout: post
title: (Math) Happy Number
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/happy-number/>

Happy Number란, 입력된 수의 자리수의 제곱해서 더함을 무한 반복했을 때 결과가 1이 나오면 Happy Number라고 한다.  
자리수를 더한 값이 1이 아니라면 Set에 저장하고 Happy Number가 아니라고 저장한다. 그것을 반복하다 1이 결과값으로 나오면 Happy Number 라고 리턴한다.

<pre class="prettyprint">
import java.util.HashSet;
import java.util.Set;

class Solution {
    Set&lt;Integer&gt; notHappys = new HashSet&lt;&gt;();

    public boolean isHappy(int n) {
        if(notHappys.contains(n)) {
            return false;
        }
        int temp = n;
        int result = 0;

        while(true) {
            result += (temp%10)*(temp%10);
            if(temp/10 == 0) {
                break;
            }
            temp = temp/10;
        }

        System.out.println(temp);
        if(result == 1) {
            return true;
        } else {
            notHappys.add(n);
        }
        return isHappy(result);
    }
}
</pre>