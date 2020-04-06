---
layout: post
title: C++ Mutable 키워드란 무엇일까?
tags: [c++, programming, effective]
category : C++
---

mutable 키워드는 상수성이 적용된 (const) 구조체, 클래스, 멤버 함수에서 특정 멤버변수의 수정을 허용하는 키워드이다. 멤버 변수 자료형의 앞이나 뒤에 붙는다.  

<pre class="prettyprint">
class A {
    int a;
    mutable int b;
    void change() const {
        a = 1; // ERROR!
        b = 2; // NOT ERROR
    }
}
</pre>

mutable을 사용할 시 주의해야할 점은 **동기화를 보장** 해야한다는 점이다.  

