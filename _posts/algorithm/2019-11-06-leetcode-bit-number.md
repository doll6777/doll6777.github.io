---
layout: post
title: (Bit) Number of 1 Bits 
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/number-of-1-bits/>

주어진 10진수의 수를 2진수로 변환한 뒤, 1의 개수를 찾는 문제이다.  
2진수로 변환해가며 1을 카운팅 할 수도 있지만, Bit Shifting을 사용하고 싶다.  
이 문제는 음수도 들어올 수 있기 때문에 2의 보수에 대하여 알아야 한다.

<pre class="prettyprint">
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;

        int m = n;
        if (m &lt; 0) {
            m = ~m;
        }

        while (m &gt; 0) {
            if ((m &amp; 1) &gt; 0) {
                count++;
            }

            m = m &gt;&gt; 1;
        }

        if (n &lt; 0) {
            count = 32 - count;
        }
        return count;
    }
}
</pre>

맨 마지막 수를 AND 연산자를 사용하여 있는지 체크하였다. 또한 양수의 경우엔 RIGHT SHIFT를 사용하면  
앞의 수가 0으로 채워지기 때문에 모든 수가 0이 될때까지 이것을 반복하였다.  
수가 32비트 정수이기 때문에 음수의 경우에는 2진수를 반전시켜서 0의 개수가 아닌것을 찾고 32에서 뺏다.

하지만 ARITHMETIC SHIFT 연산과 LOGICAL SHIFT 연산의 차이에 대해 안다면 음수에 대한 처리를  
이렇게 하지 않아도 된다.  

<https://stackoverflow.com/questions/2811319/difference-between-and>

위 링크에서 자세히 설명하고 있는데, 

```
>> is arithmetic shift right, >>> is logical shift right.

In an arithmetic shift, the sign bit is extended to preserve the signedness of the number.

For example: -2 represented in 8 bits would be 11111110 (because the most significant bit has negative weight). Shifting it right one bit using arithmetic shift would give you 11111111, or -1. Logical right shift, however, does not care that the value could possibly represent a signed number; it simply moves everything to the right and fills in from the left with 0s. Shifting our -2 right one bit using logical shift would give 01111111.

```

아래의 코드처럼 작성하면, 음수의 경우에도 SHIFT를 했을 경우 빈공간이 1로 채워지지 않고 0으로 채워진다. 따라서 훨씬 간단하게 코드를 작성할 수 있다.

<pre class="prettyprint">
public class Solution {
    public int hammingWeight(int n) {
        int result = 0;
        while (n != 0) {
            if ((n &amp; 1) == 1) {
                result++;
            }
            n = n &gt;&gt;&gt; 1;
        }
        return result;
    }
}
</pre>