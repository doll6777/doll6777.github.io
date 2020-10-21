---
layout: post
title: (Traverse) Convert a given Binary Tree to Doubly Linked List 
tags: [algorithm, programming, leetcode]
category : algorithm
---

이진 트리를 중위순회를 사용하여 탐색하고 더블링크드리스트로 반환하는 문제이다.  
메인함수에서 트리 노드를 모두 넣었다고 가정한다. 만들어진 리스트의 루트는 head.

<pre class="prettyprint">
class Node  
{ 
    int value; 
    Node left, right; 
   
    public Node(int value)  
    { 
        this.value = value; 
        left = right = null; 
    } 
} 
   
class Solution  
{ 
    Node root; 
    Node head; 
    Node prev = null; 
   
    void traverse(Node root)  
    { 
        if (root == null) 
            return; 
   
        traverse(root.left); 
   
        if (prev == null)  
            head = root; 
        else
        { 
            root.left = prev; 
            prev.right = root; 
        } 
        prev = root; 
   
        BinaryTree2DoubleLinkedList(root.right); 
    }  
}  
</pre>