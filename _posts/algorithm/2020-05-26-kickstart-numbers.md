---
layout: post
title: (Traverse) Kickstart - CountDown
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ff43/00000000003380d2

psudo code solution
```
answer_counter = 0
decreasing_counter = 0
for (i = 1 to N) {
if (A[i] == A[i - 1] - 1) {
decreasing_counter = decreasing_counter + 1
} else {
decreasing_counter = 0
}
if (A[i] == 1 and decreasing_counter >= K - 1) {
answer_counter = answer_counter + 1
}
}
print answer_counter
```

<pre class="prettyprint">
//3
//12 3
//2 1 3 7 9 3 2 1 8 3 2 1
//4 2
//101 100 99 98
//9 6
//100 7 6 5 4 3 2 1 100

#include &lt;stdio.h&gt;

int main() {
  int t;
  scanf("%d", &amp;t);

  for (int i = 1; i &lt;= t; i++) {
    int count = 0;

    int n;
    int length;
    int arr[2000001];

    scanf("%d", &amp;n);
    scanf("%d", &amp;length);

    for (int j = 0; j &lt; n; j++) {
      scanf("%d", &amp;arr[j]);
    }

    int j = n - 1;
    while (j &gt;= length-1) {
      if (arr[j] == 1) {
        bool check = true;

        for (int k = 1; k &lt; length; k++) {

          if((j-k) &lt; 0) {
            break;
          }
          if (arr[j - k] == arr[j] + k) {
          } else {
            check = false;
            if(j &gt;= (k-1)) {
              j -= (k-1);
            }
            break;
          }
        }
        if (check) {
          count++;
          j -= length;
        } else {
          j--;
        }
      } else {
        j--;
      }
    }

    printf("Case #%d: %d\n", i, count);
  }
  return 0;
}
</pre>