---
layout: post
title: (Array) find Unsorted Subarray
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/shortest-unsorted-continuous-subarray>

<pre class="prettyprint">
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] sortedNums = Arrays.copyOf(nums, nums.length);
        Arrays.sort(sortedNums);
        
        int start = -1;
        int end = -1;
        
        for(int i=0; i&lt;nums.length; i++) {
            if(nums[i] != sortedNums[i]) {
                if(start == -1) {
                    start = i;
                    end = i+1;
                } else {
                    end = i;
                }
            }    
        }
        
        if(end ==-1 &amp;&amp; start==-1){
            return 0;
        } else {
            return end-start+1;
        }
    }
}
</pre>

<https://leetcode.com/problems/shortest-unsorted-continuous-subarray/discuss/373130/Java-O(n)-time-O(1)-Space-with-Detailed-Explaination>

<pre class="prettyprint">
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int lastIndex= nums.length-1;
        int start = -1;   // or start = lastIndex;
        int end = -2;   //  or end = 0;
        int max=nums[0];      //max scan from index: 1 to lastIndex, update max and end accordingly
        int min=nums[lastIndex];    //min scan from index: (lastIndex-1) to 0, update min and start accordingly
        for(int i=1;i&lt;=lastIndex;i++){
            if(nums[i]&lt;max){
                end= i;
            }else if(nums[i]&gt;max){
                max = nums[i];
            }
            
            if(nums[lastIndex-i]&gt;min){
                start = lastIndex-i;                   
            }else if(nums[lastIndex-i]&lt;min){
                min = nums[lastIndex-i];
            }
        }        
        return (end-start+1);           
		//return end-start&lt;0?0:(end-start+1);
    }
}
</pre>
