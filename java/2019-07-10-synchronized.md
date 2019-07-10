---
layout: post
title: Java Synchronized (자바 동기화), synchronized, atomic, volatile
excerpt: "Java"
tags: [programming, thread, handler, java]
permalink: /android/:year/:month/:day/:title/
category : Java
---

멀티 스레드로 인하여 동기화를 제어해야 하는 경우에 synchronized가 필요하다.  
동기화를 제어하는 경우란 여러개의 스레드가 공유자원에 접근해야 할 경우를 제어하는 것을 뜻한다.  

Java로 프로그래밍을 하는 경우라면 키워드를 사용하여 동기화를 할 수 있는데, 대표적인 키워드가  
**synchronized** 이다.

## synchronized
synchronized는 두 가지 방법을 사용할 수 있다.
1. synchronized 함수를 만드는 법
2. synchronized 블록을 사용하는 법

### synchronized를 함수에 거는 법
함수에 lock을 거는 것처럼 보이지만, 실제로는 객체에 lock을 건다. 즉, synchronized 함수는 자신이 포함된 객체에 lock을 건다. 이 방법은 간단하지만 객체에 포함된 다른 것들도 lock이 걸리기 때문에 무식한 방법이기도 하다.

<pre class="prettyprint">
public synchronized void fun(String param) {
  // 동기화 하고싶은 코드
}
</pre>

### synchronized를 블록으로 거는 법
필요한 부분에만 동기화처리를 하고, 동기화가 필요없는 부분은 lock을 걸지않고 그대로 둘 수 있다. 

<pre class="prettyprint">
synchronized(this) {
  // 동기화 하고싶은 코드
}
</pre>

이때, this는 lock의 주체이기 때문에 this가 락을 풀때까지 다른 블록들이 대기한다.  

## Atomic
lock이나 synchronized 없이도 결과의 안정성을 보장받을 수 있는 것을 **원자적(atomic)** 이라고 하고, 자바의 concurrent 패키지에서 이를 제공한다.  

AtomicInter 클래스는 **CAS(compare-and-swap)** 기반으로 되어있다.  
CAS는 특정 메모리 위치와 주어진 위치의 value를 비교하여 다르면 대체하지 않는다. 이 방법은 저수준의 하드웨어에서 제공해 준다.

<pre class="prettyprint">
private AtomicInteger c = new AtomicInteger();

public int getNextIndex() {
    return c.getAndIncrement();
}
</pre>


## Volatile
자바 변수에 volatile을 붙이면 volatile 변수를 읽어 들일 때 CPU 캐시가 아니라 컴퓨터의 메인 메모리로부터 읽어들인다.  

volatile 키워드는 **"가시성"** 문제를 해결해준다. 가시성 문제란 멀티 쓰레드 환경에서 메인 메모리로 아직 기록하지 않은 값을 보지 못하여 동기화가 깨진 상황을 말한다. 

volatile을 써야하는 경우는 한 쓰레드에서 변수의 값을 읽고 쓰며, 다른 쓰레드에서 읽기만 하는 경우에 항상 value를 보장해 줄수있다. 그렇지 않고 멀티 쓰레드가 value를 변경한다면 synchronization을 적용해야 한다.  

<pre class="prettyprint">
public class Foo {
    public volatile int c = 0;
}
</pre>
