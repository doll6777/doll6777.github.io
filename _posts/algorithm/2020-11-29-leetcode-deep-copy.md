---
layout: post
title: (Clone) Copy List with Random Pointer
tags: [algorithm, programming, leetcode]
category : algorithm
---

> https://leetcode.com/problems/copy-list-with-random-pointer/

임의의 리스트를 제공해 주고, 이것을 Deep Copy 하는 문제이다.  
리스트를 순회하며 새로운 노드의 주소값을 미리 저장해 놓고  
다시 그 리스트를 순회하며 미리 맵에 저장된 새로운 노드의 주소를 next와 random에 넣어주면 된다.  

<pre class="prettyprint">
class Solution {
    public Node copyRandomList(Node head) {
        
        Map&#x3C;Node, Node&#x3E; map = new HashMap&#x3C;&#x3E;();
        
        Node temp = head;
        while (temp != null) {
            Node newNode = new Node(temp.val);
            map.put(temp, newNode);
            temp = temp.next;
        }
        
        Node temp2 = head;
        Node result = map.get(temp2);
        
        while (temp2 != null) {
            Node newNode = map.get(temp2);
            newNode.next = map.get(temp2.next);
            newNode.random = map.get(temp2.random);
            
            temp2 = temp2.next;
        }
        return result;
        
    }
}
</pre>