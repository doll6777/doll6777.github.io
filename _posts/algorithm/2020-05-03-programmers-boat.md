---
layout: post
title: (Greedy) 구명보트
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://programmers.co.kr/learn/courses/30/lessons/42885

limit값이 주어지고, 보트에 최대한 많은 사람을 태우는 문제.
배열을 정렬해 놓고 제일 무게가 많이 나가는 사람과 가장 적게 나가는 사람을 차례대로
태워서 count를 증가시켰다.  


<pre class="prettyprint">
import java.util.Arrays;

class Solution {
    public int solution(int[] people, int limit) {
        int answer = 0;
        Arrays.sort(people);
        int left = 0;
        int right = people.length-1;
        int remain = limit;

        while(left &lt; right) {

            boolean notLeft = false;
            boolean notRight = false;

            if(people[right] &lt;= remain) {
                remain -= people[right];
                right--;
            }
            else {
                notRight = true;
            }

            if(people[left] &lt;= remain) {
                remain -= people[left];
                left++;
            } else {
                notLeft = true;
            }

            if(notRight &amp;&amp; notLeft) {
                answer++;
                remain = limit;
            }

        }
        return answer;
    }
}

</pre>