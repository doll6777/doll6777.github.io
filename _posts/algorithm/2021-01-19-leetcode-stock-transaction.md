---
layout: post
title: (DP) Best Time to Buy and Sell Stock with Transaction Fee
tags: [algorithm, programming, leetcode]
category : algorithm
---

특정 기간동안에 주식을 매수/매도 하였을 때, 최대 이익을 구하는 문제이다.  
단, 거래는 하루당 하나, 매수 후 매도가 일어나야 하고 수수료 (fee) 가 붙는다.  
이 문제는 인덱스 i 번쨰의 날에 최대 이익을 구하려면 부분 문제로 나누어
- 현재 주식을 하나 가지고 있다
- 현재 주식이 하나도 없다
로 나눌 수 있다. 현재 하나 가지고 있다면 
하나 가지고 있다 : MAX(이전에 하나 매수했고 이번꺼 패스, 이전에 매수하지 않았고 이번에 매수)
주식이 하나도 없다 : MAX(이전에 매도해서 하나도 없고 이번에 패스, 이전에 매수하였고 이번에 매도)
이렇게 식을 세울수 있고 답은 맨 마지막 날에 매도하였을 때 최대일 것이기 때문에 맨 마지막에 하나 가지고 있을 떄의 수익을 리턴하면 된다.

<pre class="prettyprint">
class Solution {
    public int maxProfit(int[] prices, int fee) {
        if(prices.length &lt; 1) {
            return 0;
        }
        int oneStock = -prices[0];
        int noStock = 0;
        
        for(int i=1; i&lt; prices.length; i++) {
            noStock = Math.max(noStock, oneStock + prices[i] - fee);
            oneStock = Math.max(oneStock, noStock - prices[i]);
        }
        return noStock;
    }
}
</pre>