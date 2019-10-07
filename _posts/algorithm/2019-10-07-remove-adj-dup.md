---
layout: post
title: (String) Remove All Adjacent Duplicates in String II
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/contest/weekly-contest-156/problems/remove-all-adjacent-duplicates-in-string-ii/

문자열이 주어졌을때 k 길이만큼의 중복된 문자를 삭제해나간다. 반복하다가 삭제할 것이 없으면 멈추고 최종 문자열을 리턴한다.


<pre>
Example 1:

Input: s = "abcd", k = 2
Output: "abcd"
Explanation: There's nothing to delete.
Example 2:

Input: s = "deeedbbcccbdaa", k = 3
Output: "aa"
Explanation: 
First delete "eee" and "ccc", get "ddbbbdaa"
Then delete "bbb", get "dddaa"
Finally delete "ddd", get "aa"
Example 3:

Input: s = "pbbcggttciiippooaais", k = 2
Output: "ps"
</pre>

<pre class="prettyprint">
class Solution {
    public String removeDuplicates(String s, int k) {
        StringBuilder stringBuilder = new StringBuilder(s);

        while (true) {
            boolean duplicateSubExist = false;

            for (int i = 0; i <= (stringBuilder.length() - k); i++) {
                boolean duplicate = true;

                for (int j = 1; j < k; j++) {
                    if (stringBuilder.charAt(i + j) != stringBuilder.charAt(i)) {
                        duplicate = false;
                        break;
                    }
                }
                if (duplicate) {
                    for (int j = 0; j < k; j++) {
                        stringBuilder.deleteCharAt(i);
                    }
                    duplicateSubExist = true;
                }
            }
            if (!duplicateSubExist) {
                break;
            }
        }
        return stringBuilder.toString();
    }
}
</pre>

성능 최적화 :: 

<pre class="prettyprint">
class Solution {
    public String removeDuplicates(String s, int k) {
        int count = 1;
        for(int i = 0; i < s.length() - 1; i++){
            if(s.charAt(i) == s.charAt(i + 1)) count++;
            else count = 1;
            if(count == k) s = removeDuplicates(s.substring(0, i - k + 2) + s.substring(i + 2), k);
        }
        return s;
    }
}
</pre>
