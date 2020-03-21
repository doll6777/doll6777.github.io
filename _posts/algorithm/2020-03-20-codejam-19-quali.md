---
layout: post
title: (CodeJam) CodeJam Qualification Round-2019 Foregone Solution
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://codingcompetitions.withgoogle.com/codejam/round/0000000000051705/0000000000088231

코드잼 우승자에게 N원의 수표를 발행해 주어야 하나, 프린터기에 4자를 쓸수있는 자판이 고장났다.  

이럴 때 N을 2장으로 나누어서 배포해야한다.  나누어서 주어야 할 4가 들어가지 않은 수표 A와 B는 무엇인가?  

이 문제는 첫번째로 Bruth Forth 알고리즘을 사용하여 합이 N이되는 두 수를 구할 수 있다. 하지만 이 방법은 테스트케이스 1번까지만 통과할 수 있다.  

따라서 테스트케이스 2번을 통과하려면 다른 방법을 생각해 봐야 하는데, 2번째 방법은 이진탐색을 시도하였으나 이진탐색이 아니고 랜덤으로 수를 뽑는 것이었다.  

또한 마지막 3번째 테스트케이스는 많이 어려웠다. 3번째를 푸는 방법은 수학적으로 푸는것이다.  

N이라는 수를 분해해보면 만약 743이라고 했을 때 7*100 + 4*10 + 3으로 구성된다고 볼 수 있다.  

이 수를 두개로 나눈다는것은 4*10을 2*10의 두가지 경우로 나눈다는 것과 동일하다.  

생각보다 쉬운 방법으로 두 수를 분해할 수 있지만 3번째 테스트 케이스의 경우 N의 범위가 10의 100승까지 넘어가기 때문에 long long int까지도 커버할 수 없다.  

따라서 string 처리로 입력을 받고, 계산을 할때도 string으로만 처리해아 한다.  

이 문제는 매우 쉬워보였지만 테스트 케이스 3번까지 모두 풀려면 생각을 많이 해봐야 풀수있는 문제였다.  

<pre class="prettyprint">
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;

using namespace std;

void getB(char* str, string &amp;a, string &amp;b) {

    for(int i = 0; i &lt;strlen(str); i++) {
        if(str[i] == '4') {
            b +=  '2';
            a +=  '2';
        } else {
            a += str[i];
            b += '0';
        }
    }
}

int main() {
    int t;
    
    scanf(&quot;%d&quot;, &amp;t);
    
    for(int i=1; i&lt;=t; i++) {

        char str[1000];
        scanf(&quot;%s&quot;, str);

        string a = &quot;&quot;;
        string b = &quot;&quot;;

        getB(str, a, b);       
        
        printf(&quot;Case #%d: %s %s\n&quot;, i, a.c_str(), b.c_str());
    }
    
    return 0;
}
</pre>