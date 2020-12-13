---
layout: post
title: (Traverse) 3 Sum 
tags: [algorithm, programming, leetcode]
category : algorithm
---


> https://leetcode.com/problems/task-scheduler/

숫자 3개의 합이 0이 되는 요소들의 리스트를 구해야한다.  
조금 어려웠던 점은 리스트를 "중복되지 않게" 넣어야 한다.  
처음에는 완전탐색으로 접근하였는데, 그럴 경우 시간초과가 난다.  
따라서 다르게 접근해야하는데, 리트코드 문제중 "2 Sum"이라는 문제를  
Solution에 있는 방법대로 먼저 풀고 이 문제를 접근해야 쉽게 풀 수 있다.  

수를 중복되지 않게 3개 골라야 하므로, 일단 배열을 정렬한다.  
정렬 후 밖의 for문에서는 첫 번째 숫자를 고른다.  
첫 수를 골랐으니 나머지 두 숫자는 "2 Sum" 문제 풀 때의 방법으로  
2 pointer를 사용하여 풀면 된다.  

low 포인터와 high pointer를 움직이면서 0-첫번째숫자 와 num[low]+num[high]가 같아지는 지점을 찾는다. 

이진 탐색할 때 찾는 값보다 작으면 low 포인터를 오른쪽으로 움직이고 high 포인터를 왼쪽으로 움직였던 방법과 비슷하게 포인터를 움직여 가며 탐색한다.  


<pre class="prettyprint">
class Solution {
    public List&#x3C;List&#x3C;Integer&#x3E;&#x3E; threeSum(int[] nums) {
        List&#x3C;List&#x3C;Integer&#x3E;&#x3E; result = new ArrayList&#x3C;&#x3E;();
        Arrays.sort(nums);
        
        for(int i=0; i&#x3C; nums.length-2; i++) {
            int remain = 0 - nums[i];
            int low = i+1;
            int high = nums.length-1;
            if(i &#x3E; 0 &#x26;&#x26; (nums[i] == nums[i-1])) continue;
            
            while(low &#x3C; high) {
                if((low != i+1) &#x26;&#x26; (nums[low] == nums[low-1])) {
                    low++;
                    continue;
                 }
                if((high+1) &#x3C; nums.length-1 &#x26;&#x26; nums[high] == nums[high+1]) {
                    high--;
                    continue;
                }
                int sum = nums[low]+nums[high];
                if(sum == remain) {
                    List&#x3C;Integer&#x3E; list = new ArrayList&#x3C;&#x3E;();
                    list.add(nums[i]);
                    list.add(nums[low]);
                    list.add(nums[high]);
                    result.add(list);
                    low++;
                }
                else if(sum &#x3E; remain) {
                    high--;
                }
                else if(sum &#x3C; remain) {
                    low++;
                }
            }
        }
        return result;
    }
}
</pre>