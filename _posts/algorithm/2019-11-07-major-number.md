---
layout: post
title: (Array) Majority Element 
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/majority-element/>

<pre class="prettyprint">
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int majorityElement(int[] nums) {
        HashMap&lt;Integer, Integer&gt; hashMap = new HashMap&lt;&gt;();

        for (int num : nums) {
            if (!hashMap.containsKey(num)) {
                hashMap.put(num, 0);
            } else {
                int val = hashMap.get(num);
                hashMap.put(num, val + 1);
            }
        }

        int maxKey = -1;
        int maxValue = -1;

        for (Map.Entry&lt;Integer, Integer&gt; entry : hashMap.entrySet()) {
            if(entry.getValue() &gt; maxValue) {
                maxValue = entry.getValue();
                maxKey = entry.getKey();
            }
        }
        return maxKey;
    }
}
</pre>

간단하게 해쉬맵을 사용하여 구현하였다. 하지만 이 방법 외의 방법과 Bruth Force 외에 다른 방법이 있을까?

<https://www.geeksforgeeks.org/majority-element/>

GeeksForGeeks에도 방법들이 나와있는데, 그중 한가지 방법이 보이어 무어 방법이다. 아래를 참고하자.

<pre class="prettyprint">
class Solution {
public int majorityElement(int[] nums) {

    int count, ele;
    count = 0; ele = 0;
    
    for(int n: nums){
        
        if(count ==0){
            ele = n;
            count = 1;
        }
        else if(ele == n){
            count++;
            
        }
        else{
            count--;
        }
    }
    return ele;
    }
}
</pre>