---
layout: post
title: (String)  Longest Common Prefix Solution
tags: [algorithm, programming, leetcode]
category : algorithm
---

<pre class="prettyprint">
class Solution {
    public String longestCommonPrefix(String[] strs) {

        if (strs.length == 0) {
            return "";
        }

        int minLength = Integer.MAX_VALUE;
        String minString = "";

        for (String str : strs) {
            if (minLength > str.length()) {
                minLength = str.length();
                minString = str;
            }
        }

        StringBuilder stringBuilder = new StringBuilder();

        for (int i = 0; i < minLength; i++) {

            boolean allTrue = true;

            for (String str : strs) {
                if (str.equals(minString)) {
                    continue;
                }
                if (str.charAt(i) != minString.charAt(i)) {
                    allTrue = false;
                }
            }

            if (allTrue) {
                stringBuilder.append(minString.charAt(i));
            } else {
                break;
            }
        }
        return stringBuilder.toString();
    }
}
</pre>

코드를 좀더 줄이면..

## Solution 1
Horizontal Scanning
<pre class="prettyprint">
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) return "";
        String prefix = strs[0];

        for (int i = 1; i < strs.length; i++) {
            while (strs[i].indexOf(prefix) != 0) {
                prefix = prefix.substring(0, prefix.length() - 1);
                if (prefix.isEmpty()) return "";
            }
        }
        return prefix;
    }
}
</pre>


## Solution 2
Vertical Scanning
<pre class="prettyprint">
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) return "";

        for (int i = 0; i < strs[0].length(); i++) {
            char c = strs[0].charAt(i);

            for (int j = 1; j < strs.length; j++) {
                if (i == strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i);
                }
            }
        }

    return strs[0];
    }
}
</pre>

## Solution 3
Divide and Conquer
<pre class="prettyprint">
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";    
        return longestCommonPrefix(strs, 0 , strs.length - 1);
}

private String longestCommonPrefix(String[] strs, int l, int r) {
    if (l == r) {
        return strs[l];
    }
    else {
        int mid = (l + r)/2;
        String lcpLeft =   longestCommonPrefix(strs, l , mid);
        String lcpRight =  longestCommonPrefix(strs, mid + 1,r);
        return commonPrefix(lcpLeft, lcpRight);
   }
}

String commonPrefix(String left,String right) {
    int min = Math.min(left.length(), right.length());       
    for (int i = 0; i < min; i++) {
        if ( left.charAt(i) != right.charAt(i) )
            return left.substring(0, i);
    }
    return left.substring(0, min);
}
</pre>
