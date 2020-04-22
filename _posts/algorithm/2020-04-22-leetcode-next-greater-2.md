---
layout: post
title: (Traverse) Next Greater Element II 
tags: [algorithm, programming, leetcode]
category : algorithm
---


> https://leetcode.com/problems/next-greater-element-ii/

어레이가 순환된다고 했을때, 해당 인덱스의 수보다 큰 인덱스에서 해당 수보다 큰 수는 무엇인가?  


Bruth-Forth 알고리즘 사용
<pre class="prettyprint">
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int[] result = new int[nums.length];
        int i = 0;
        while(i &lt; nums.length) {
            int j = (i+1) % nums.length;
            
            while(j != i) {
                
                if(nums[i] &lt; nums[j]) {
                    result[i] = nums[j];
                    break;
                }
                j = (j+1) % nums.length;
                
            }
            if(j == i) {
                result[i] = -1;
            }
            i++;
        }
        return result;
    }
}
</pre>

스택 사용한 풀이

어레이의 크기를 두배로 늘려 복사해두고,  
이를 뒤부터 탐색한다. 스택에 현재 값보다 작거나 같으면 pop하고, 마지막 값인 인덱스를 선택한다.  
또한 i가 하나 작아질수록 스택에 값을 새로 Push한다.

<pre class="prettyprint">
public class Solution {

    public int[] nextGreaterElements(int[] nums) {
        int[] res = new int[nums.length];
        Stack&lt;Integer&gt; stack = new Stack&lt;&gt;();
        for (int i = 2 * nums.length - 1; i &gt;= 0; --i) {
            while (!stack.empty() &amp;&amp; nums[stack.peek()] &lt;= nums[i % nums.length]) {
                stack.pop();
            }
            res[i % nums.length] = stack.empty() ? -1 : nums[stack.peek()];
            stack.push(i % nums.length);
        }
        return res;
    }
}
</pre>
