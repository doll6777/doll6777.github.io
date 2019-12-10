---
layout: post
title: (Math) Factorial Trailing Zeros
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/factorial-trailing-zeroes/>

이 문제를 Bruth Forth로 접근하여 팩토리얼을 구하고, O(n)으로 해결할수 있지만 타임아웃이 난다. 따라서 다른 방법을 생각해야한다.  

trailing zero란 어떤 수에서 0으로 끝나는 숫자의 개수를 말한다.  

0으로 끝나는것은 10(2x5) 이 곱해졌을 때 만들어지게 된다.  

그런데, 팩토리얼에서는 5의 경우 1*2*3*4*5*.. 이전에 2가 나왔기 때문에 5가 나왔을경우 무조건 10을 만들 수 있다.  

또한 10의 배수인경우는 이미 (2*5)이기 때문에 상관없다.  

따라서, 주어진 수까지 5의 배수가 몇개인지만 구하면 된다.  

<pre class="prettyprint">
class Solution {
    public int trailingZeroes(int n) {
        int temp = n;
        int answer = 0;

        while (temp >= 5) {
            temp = temp / 5;
            answer += temp;
        }

        return answer;
    }
}
</pre>