---
layout: post
title: 왜 소멸자에 virtual 키워드를 쓰는가?
tags: [c++, programming, effective]
category : C++
---

클래스의 생성과 소멸 과정을 보면 알 수 있다.  

생성자는 부모 클래스의 생성자가 먼저 불리고, 소멸자는 자식 클래스의 소멸자가 먼저 불린다.  

그런데 다형성으로 인해 부모 클래스의 포인터로부터 자식 클래스를 호출하면 가상 함수로 정의되지 않은 자식 클래스의 오버라이딩된 함수를 호출하면 부모 클래스의 멤버 함수가 호출된다.  

소멸자도 마찬가지이다. 부모 포인터를 사용하여 객체를 삭제하면 부모 클래스의 소멸자만 호출된다.  

> 가상 함수 키워드 virtual은 자식 클래스에서 재정의될 수 있음을 명시한다

<pre class="prettyprint">
class A
{
public:
  A();
  virtual ~A();
};
class B : public A
{
public:
  B();
  ~B();
};
A::A()
{
  cout &lt;&lt; &quot;A&quot; &lt;&lt; endl;
}
A::~A()
{
  cout &lt;&lt; &quot;~A&quot; &lt;&lt; endl;
}
B::B()
{
  cout &lt;&lt; &quot;B&quot;&lt;&lt; endl;
}
B::~B()
{
  cout &lt;&lt; &quot;~B&quot; &lt;&lt; endl;
}
int main()
{
  B *B = new B;
  A *A = B;
  delete A;
  return 0;
}
</pre>

소멸자에 virtual을 쓰지 않는 경우
```
A
B
~A
```

소멸자에 virtual을 쓴 경우
```
A
B
~B
~A
```