---
layout: post
title: [LinkedList] Delete Node in a Linked List
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/explore/interview/card/top-interview-questions-easy/93/linked-list/553/

## Accepted
<pre>
class Solution {
    public void deleteNode(ListNode node) {
        ListNode nextNode = node.next;
        node.next = nextNode.next;
        node.val = nextNode.val;
    }
}
</pre>

단방향 링크드리스트에서 노드를 제거하는것은 굳이 메모리를 비워주거나 이전 노드의 next에 다음 노드를 연결시키지 않아도 된다. 현재 지우려는 노드를 다음 노드로 대체시키면 간단하게 해결 가능하다.