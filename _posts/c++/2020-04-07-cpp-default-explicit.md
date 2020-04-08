---
layout: post
title: C++11 Default와 Explicit, Delete 키워드란 무엇일까?
tags: [c++, programming, effective]
category : C++
---

### explicit
C++에서 클래스의 객체를 생성할 때 생성자를 사용해서 형번환을 할 수 있다.  

<pre class="prettyprint">
class B{
public:
    B() { }
    B(A&amp; _a){}
    B(int n){}
    B(char d){}
};
</pre>

B = 1;
B = 'a';

이와같이 호출할 때, 해당 타입을 인자로 가진 생성자가 있는 경우에는 형변환이 이루어지며 대입된다.  
이런 대입방법은 의도하지 않은 버그를 낳을 수 있기 때문에 생성자에 **explicit** 키워드를 지정한다면  
**명시적인 생성자**만 호출할 수 있다.  

> 무조건 new B(1) 로 호출하여 대입 가능하다!



### default
default 키워드를 사용하여 명시적으로 default 생성자를 선언할 수 있다.

<pre class="prettyprint">
class B
{
    public:
      B() = default; // B(){}와 동일하다.
    private:
      B(const B&amp; b) = default; // 복사생성자도 정의할 필요가 없이 기본을 사용할 수 있다.
}
</pre>

### Delete
이와 반대로 명시적으로 생성자를 사용하고 싶지 않은 경우엔 delete를 사용한다. 또한 암시적 형변환이 일어나는 것도 막을 수 있다.

<pre class="prettyprint">
class B
{
    public:
      B() {}
      ~B() {}
    
    B(const B&amp; b) = delete;
    void foo(int f) = delete;    
}
</pre>
