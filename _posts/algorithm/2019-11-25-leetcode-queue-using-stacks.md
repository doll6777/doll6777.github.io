---
layout: post
title: (Stack) Implement Queue using Stacks
tags: [algorithm, programming, leetcode]
category : algorithm
---

> <https://leetcode.com/problems/implement-queue-using-stacks/>

본 문제는 큐를 스택만을 사용해서 구현하는 문제이다.  
스택을 여러개 사용해도 된다. 스택의 특성은 나중에 들어온것이 먼저 나가므로 큐와 반대다.  
따라서 스택을 하나 더 두고, 하나씩 원소를 빼서 다른 스택에 저장하여 큐처럼 사용한다.  
이때 주의해야 할 점은, pop/peek 할때 stack2가 비어있는지 검사해야 한다는 것이다.  
비어있지 않을때도 pop/peek를 하게되면, 큐로 사용하는 stack2에 새로운 원소가 들어와 순서가 깨지게 된다.

<pre class="prettyprint">
class MyQueue {

    Stack&lt;Integer&gt; stack1;
    Stack&lt;Integer&gt; stack2;

    /**
     * Initialize your data structure here.
     */
    public MyQueue() {
        stack1 = new Stack&lt;&gt;();
        stack2 = new Stack&lt;&gt;();
    }

    /**
     * Push element x to the back of queue.
     */
    public void push(int x) {
        stack1.push(x);
    }

    /**
     * Removes the element from in front of queue and returns that element.
     */
    public int pop() {
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }

    /**
     * Get the front element.
     */
    public int peek() {
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.peek();
    }

    /**
     * Returns whether the queue is empty.
     */
    public boolean empty() {
        return stack1.isEmpty() &amp;&amp; stack2.isEmpty();
    }
}
</pre>

