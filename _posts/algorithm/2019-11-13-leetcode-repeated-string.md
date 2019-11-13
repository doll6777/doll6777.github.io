---
layout: post
title: (String) Repeated Substring Pattern
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/repeated-substring-pattern>

문자열이 주어졌을 때, 반복되는 부분 문자열로 구성되어 있는지 판별하는 함수를 작성하는 것이다.  

아래 함수는 스트링에서 반복되는 문자를 계속 지워나가며 검사한다. 하지만 <i>매우 비효울적이다.</i>

<pre class="prettyprint">
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        if (s.length() == 1) {
            return false;
        }

        boolean repeated = false;

        for (int i = 1; i &lt;= (s.length() / 2); i++) {
            StringBuilder stringBuilder = new StringBuilder(s);
            String subString = stringBuilder.substring(0, i);

            for (int j = 0; j &lt; (s.length()) / i; j++) {
                if (stringBuilder.substring(0, i).equals(subString)) {
                    stringBuilder.delete(0, i);
                }
            }

            if (stringBuilder.toString().isEmpty()) {
                repeated = true;
                break;
            }
        }

        return repeated;
    }
}
</pre>

KMP 알고리즘을 사용해서 구현하면 훨씬 빨라진다. KMP 소스는 아래를 참고하면 된다.

<pre class="prettyprint">
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        if (s == null || s.length() &lt;= 1){
            return false;
        }
        int[] next = getNextArray(s);
        int n = next.length;
        return (n % (n - next[n - 1])) == 0 &amp;&amp; next[n - 1] &gt; 0; 
    }
    public int[] getNextArray(String s){
        char[] array = s.toCharArray();
        int[] next = new int[array.length];
        next[0] = 0;
        int i = 1;
        int j = 0;
        while (i &lt; array.length){
            if (array[i] == array[j]){
                next[i] = j + 1;
                j++;
            }else{
                // array[i] != array[j]
                while (j &gt; 0 &amp;&amp; array[j] != array[i]){
                    j = next[j - 1];
                }
                if (array[i] != array[j]){
                    next[i] = 0;
                }else{
                    next[i] = j + 1;
                    j++;
                }
            }
            i++;
        }
        return next;
    }
}

</pre>
