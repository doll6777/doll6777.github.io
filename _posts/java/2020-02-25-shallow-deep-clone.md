---
layout: post
title: Effective Java 2/E, Clone을 재정의 할 때는 신중하라
excerpt: "Java"
tags: [programming, effective, java]
permalink: /android/:year/:month/:day/:title/
category : Java
---

모든 클래스의 부모인 Object 클래스는 clone() 메소드를 가지고 있다. clone()은 객체 생성의 다른 방법으로, new 연산자를 사용하지 않고 객체 생성을 하게된다.  

clone() 을 재정의 하기 위해서는, Cloneable 이라는 인터페이스를 구현해야 하는데,  
Cloneable 인터페이스를 보면 내용이 아무것도 없다.  

<pre class="prettyprint">
package java.lang;

public interface Cloneable {
}
</pre>

만약 Clonable를 구현하지 않는다면, **CloneNotSupportedException** 예외가 호출된다.  

<pre class="prettyprint">
class ListNode implements Cloneable {
    int val;

    ListNode(int x) {
        val = x;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
</pre>

이렇게 clone을 재정의하면, primitive 원소인 val의 값이 복사되고 복사된 객체가 생긴다.  

> 하지만, clone을 재정의할 때는 주의해야 한다.

<pre class="prettyprint">
class ListNode implements Cloneable {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
</pre>

만약 링크드 리스트이 다음 노드를 가지고 있는 ListNode가 있다고 하고, clone() 을 진행하게 된다면

<pre class="prettyprint">
    ListNode clonedNode = (ListNode) node.clone();
</pre>

clone()의 기본 동작은 shallow copy이기 때문에, next가 새로 생성이 되며 copy되는 것이 아니고, 
next의 주소값을 저장하며 copy되게 된다.  
이렇게 된다면 문제점은 다음과 같다.  

<pre class="prettyprint">

    public static void main(String[] args) {

        ListNode node = new ListNode(1);
        ListNode node2 = new ListNode(2);
        node.next = node2;

        try {
            ListNode clonedNode = (ListNode) node.clone();

            System.out.println(node.array);
            System.out.println(clonedNode.array);

            clonedNode.array[0] = 10;

            System.out.println(node.array[0]);
            System.out.println(clonedNode.array[0]);

        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
</pre>

```
[I@7dc36524
[I@7dc36524
10
10
```

복사한 노드의 array의 주소값이 같으므로, copy한 객체의 array 값을 바꿔도 원본 객체의 array에 영향을 받는것이다.  
이를 해결하는 방법은 다음과 같다.  

<pre class="prettyprint">
    @Override
    protected Object clone() throws CloneNotSupportedException {

        ListNode object = (ListNode) super.clone();
        object.array = array.clone();
        return object;
    }
</pre>

다시 코드를 돌려보면, array의 주소가 달라져 있고 값도 다르게 찍힌다.  

```
[I@7dc36524
[I@35bbe5e8
1
10
```

> 객체를 복사해야 하는 경우에는 Cloneable 인터페이스를 구현하는 클래스를 계승한다면 제대로 된 clone 메서드를 구현해야 한다.    하지만, 그렇지 않다면 객체를 복사할 대안을 제공하거나, 아예 복제 기능을 제공하지 않는 것이 낫다. 객체 복제를 지원하는 좋은 방법은, 복사 생성자나 복사 팩터리 메서드를 제공하는 것이 좋다.

<pre class="prettyprint">
    public Yum(Yum yum);
</pre>

<pre class="prettyprint">
    public static Yum newInstance(Yum yum);
</pre>