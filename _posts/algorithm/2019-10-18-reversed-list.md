---
layout: post
title: (LinkedList) Reverse Linked List
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/explore/interview/card/top-interview-questions-easy/93/linked-list/560/


링크드리스트 뒤집기 문제. Extra Space를 사용한다면 스택을 사용하여 구현할 수 있다. 그러나 공간을 사용하지 않고 어떻게 구현할까?

리스트의 노드는 이렇게 정의되어 있다.
<pre class="prettyprint">
public class ListNode {
     int val;
     ListNode next;
     ListNode(int x) { val = x; }
 }
</pre>


## Solution 1 : Iteration
링크드 리스트를 순회하는데, 이전 노드, 현재 노드, 다음 노드를 모두 기억하고 있다가 다음으로 향하는 포인터만 이전 노드로 바꿔준다.

<pre class="prettyprint">
class Solution {
    public ListNode reverseList(ListNode head) {

        ListNode cur = head;
        ListNode pre = null;
        ListNode next = head;
        
        while(cur != null) {
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
</pre>

## Solution 2 : Recursion
<pre class="prettyprint">
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}
</pre>
