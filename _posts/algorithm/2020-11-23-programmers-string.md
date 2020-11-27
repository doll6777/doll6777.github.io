---
layout: post
title: (Traverse) 문자열 압축
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://programmers.co.kr/learn/courses/30/lessons/60057

정해진 문자열의 길이만큼 잘라가며 문자열을 압축할 수 있다. 이 때 주의할 점은 정해진 문자열은 고정이라는 것이다.  
시간 복잡도는 O(n^2) 이고, 정해진 길이만큼 iterate 하며 왼쪽 문자열과 오른쪽 문자열을 비교하였다.  
이 떄 왼쪽 문자열과 오른쪽 문자열이 같다면 카운트를 저장하여 압축하고, 같지 않다면 continue 하며 문자열을 저장한다.  
이 때 같은 문자열이었다가 다른 문자열로 변할 때 문자열을 만들도록 코드를 짰는데, 나처럼 이렇게 짜는 경우에는 맨 마지막에 같은 문자열로 끝나는 경우에는 마지막 문자열이 저장이 안되거나 하는 문제가 생긴다.  
따라서 엣지 케이스로 처리 해줬지만 더 깔끔하게 처리 할 수 있는 방법이 있을것이다. 

<pre class="prettyprint">
import java.util.List;

class Solution {
    public int solution(String s) {
        int answer = Integer.MAX_VALUE;
        int n = s.length();
        if(n == 1) {
            return 1;
        }
        
        for (int i = 1; i <= n/2; i++) {
            String result = "";
            String prevResult = "";
            int count = 1;
            
            for (int j = i; j < n; j += i) {
                String left = s.substring(j - i, j);
                String right = s.substring(j, Math.min(j + i, n));
                
                if (left.equals(right)) {
                    count++;
                    prevResult = left;
                } else {
                    if (count > 1) {
                        result += String.valueOf(count);
                        result += prevResult;
                        count = 1;
                        prevResult = "";
                    } else {
                        result += left;

                    }
                    if (j + i >= n) {
                        result += right;
                    }
                }
            }
            
            if (!prevResult.isEmpty()) {
                result += String.valueOf(count);
                result += prevResult;
            }
            answer = Math.min(answer, result.length());

        }
        return answer;
    }
}
</pre>