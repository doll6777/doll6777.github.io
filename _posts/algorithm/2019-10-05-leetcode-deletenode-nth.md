---
layout: post
title: (LinkedList) Remove Nth Node From End of List
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/explore/interview/card/top-interview-questions-easy/93/linked-list/603/

이전에 풀었던 중간 노드 삭제하기 문제와는 다르게, tail의 삭제가 들어올 수 있기 때문에 주의해야한다. 따라서 삭제하기 전 이전노드와 다음 노드를 기억하고 있어야 한다.  
또한 가장 앞 노드도 삭제될수 있다. 이 경우에 루트를 다음 노드로 바꿔준다.  

## Accepted
<pre class="prettyprint">
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode node = head;

        int listSize = 0;
        while (node != null) {
            listSize++;
            node = node.next;
        }
        node = head;

        ListNode prev = null;
        ListNode after = null;

        for(int i=0; i&lt;listSize-n; i++) {
            prev = node;
            node = node.next;
            after = node.next;
        }

        if(prev != null) {
            prev.next = after;
        } else {
            head = node.next;
        }
        if((listSize == 1) &amp;&amp; n == 1) {
            return null;
        }
        return head;
    }
}
</pre>

# 더 최적화하기
Approach 2: One pass algorithm
Algorithm

The above algorithm could be optimized to one pass. Instead of one pointer, we could use two pointers. The first pointer advances the list by n+1n+1 steps from the beginning, while the second pointer starts from the beginning of the list. Now, both pointers are exactly separated by nn nodes apart. We maintain this constant gap by advancing both pointers together until the first pointer arrives past the last node. The second pointer will be pointing at the nnth node counting from the last. We relink the next pointer of the node referenced by the second pointer to point to the node's next next node.

<pre class="prettyprint">
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode first = dummy;
    ListNode second = dummy;
    // Advances first pointer so that the gap between first and second is n nodes apart
    for (int i = 1; i <= n + 1; i++) {
        first = first.next;
    }
    // Move first to the end, maintaining the gap
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    second.next = second.next.next;
    return dummy.next;
}
</pre>

Complexity Analysis

Time complexity : O(L)O(L).

The algorithm makes one traversal of the list of LL nodes. Therefore time complexity is O(L)O(L).

Space complexity : O(1)O(1).

We only used constant extra space.