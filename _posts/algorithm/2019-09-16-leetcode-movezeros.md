---
layout: post
title: (Arrays) Move Zeros
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/explore/interview/card/top-interview-questions-easy/92/array/567/

0인 원소들을 모두 뒤로 보내는 moveZeros를 구현하라. 새로운 메모리를 할당하지 않고 스왑야 한다. 내 접근법의 경우에는 for문을 돌며 0이 아닌것들을 모두 앞으로 swap 하였다.

## Accepted
<pre class="prettyprint">
class Solution {
    public void moveZeroes(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {

                int j = i;
                while (j > 0 && nums[j-1] == 0) {
                    int temp = nums[j-1];
                    nums[j-1] = nums[j];
                    nums[j] =temp;
                    j--;
                }
            }
        }
        System.out.println(Arrays.toString(nums));
    }
}
</pre>

## Best Solution  


Move Zeroes
Solution
Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Example:

Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
Note:

You must do this in-place without making a copy of the array.
Minimize the total number of operations.

Approach #3 (Optimal) [Accepted]
The total number of operations of the previous approach is sub-optimal. For example, the array which has all (except last) leading zeroes: [0, 0, 0, ..., 0, 1].How many write operations to the array? For the previous approach, it writes 0's n-1n−1 times, which is not necessary. We could have instead written just once. How? ..... By only fixing the non-0 element,i.e., 1.

The optimal approach is again a subtle extension of above solution. A simple realization is if the current element is non-0, its' correct position can at best be it's current position or a position earlier. If it's the latter one, the current position will be eventually occupied by a non-0 ,or a 0, which lies at a index greater than 'cur' index. We fill the current position by 0 right away,so that unlike the previous solution, we don't need to come back here in next iteration.

In other words, the code will maintain the following invariant:

All elements before the slow pointer (lastNonZeroFoundAt) are non-zeroes.

All elements between the current and slow pointer are zeroes.

Therefore, when we encounter a non-zero element, we need to swap elements pointed by current and slow pointer, then advance both pointers. If it's zero element, we just advance current pointer.

With this invariant in-place, it's easy to see that the algorithm will work.

C++

<pre class="prettyprint">
void moveZeroes(vector<int>& nums) {
    for (int lastNonZeroFoundAt = 0, cur = 0; cur < nums.size(); cur++) {
        if (nums[cur] != 0) {
            swap(nums[lastNonZeroFoundAt++], nums[cur]);
        }
    }
}
</pre>

Complexity Analysis

Space Complexity : O(1)O(1). Only constant space is used.

Time Complexity: O(n)O(n). However, the total number of operations are optimal. The total operations (array writes) that code does is Number of non-0 elements.This gives us a much better best-case (when most of the elements are 0) complexity than last solution. However, the worst-case (when all elements are non-0) complexity for both the algorithms is same.