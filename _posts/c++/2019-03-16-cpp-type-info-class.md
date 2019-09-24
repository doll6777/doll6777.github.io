---
layout: post
title: 템플릿 메타프로그래밍과 특성정보 클래스
tags: [c++, programming, effective]
category : C++
---

##  특성정보 클래스
STL은 기본적으로 컨테이너, 반복자, 알고리즘의 템플릿으로 구성되어 있지만 이 외에 유틸리티라고 불리는 템플릿도 가지고 있다. 이들 중 하나가 advance라는 이름의 템플릿인데, 이 템플릿이 하는 일은 지정된 반복자를 지정된 거리만큼 이동시키는 것이다.

<pre class="prettyprint">
template<typename IterT, typename DistT>
void advance(IterT&, DistT d);
</pre>

advance는 그냥 iter += d만 하면 될 것 같지만, 사실 이렇게 구현할수 없다.  
왜냐하면 += 연산을 지원하는 반복자는 임의 접근 반복자밖에 없기 때문이다. 임의 접근 반복자보다 기능적으로 떨어지는 다른 반복자 타입의 경우에는 ++ 혹은 -- 연산을 d번 적용하는 것으로 advance를 구현해야 한다.  

STL 반복자에는 여러 종류가 있다. 반복자가 지원하는 연산에 따라 다섯 가지로 나뉘는데,  

- 입력 반복자 (input iterator) - istream_operator
- 출력 반복자 (output iterator) - ostream_operator
- 순방향 반복자 (forward iterator) - TR1의 해시 컨테이너를 가리키는 반복자
  - 입력 반복자와 출력 반복자가 하는 일은 기본적으로 다 할 수 있고, 여기에 덧붙여 자신이 가리키고 있는 위치에서 읽기와 쓰기를 동시에 할 수 있으며, 그것도 여러 번이 가능하다.
- 양방향 반복자 (bidirectional iterator) - STL의 list에 쓰는 빤복자
  - 순방향 반복자에 뒤로 갈 수 있는 기능을 추가한 것이다.
- 임의 접근 반복자 (random access iterator) - 양방향 반복자에 반복자 산술 연산 수행 기능을 추가한 것, vector, deque, string에 사용하는 반복자
  - 주어진 반복자를 임의의 거리만큼 앞뒤로 이동시키는 일을 상수 시간에 할 수 있다. 포인터 산술 연산과 비슷함.


  C++ 표준 라이브러리에는 다섯 개의 반복자 범주를 식별하는 데 쓰이는 태그 구조체가 정의되어 있다.  
  - struct input_iterator_tag {};
  - struct output_iterator_tag {};
  - struct forward_iterator_tag: public input_iterator_tag {};
  - struct bidirectional_iterator_tag: public forward_iterator_tag {};
  - struct random_access_iterator_tag: public bidirectional_iterator_tag {};

<pre class="prettyprint">
template<typename IterT, typename DistT>
void advance(Iter& iter, DistT d) 
{
  if (iter가 임의 접근 반복자이다) {
      iter += d;  // 임의 접근 반복자에 대해서는 반복자 산술 연산을 쓴다.
  }
  else {
      if (d >= 0) { while(d--) ++iter; } // 다른 종류의 반복자에 대해서는 ++ 혹은 -- 연산의 반복 호출을 사용한다.
      else { while (d++) --iter; }
  }
}
</pre>

iterT가 임의 접근 반복자 타입인지를 알아야 한다. 어떤 타입에 대한 정보를 어떻게 얻어낼 수 있을까?  

바로 **특성정보(traits)**  를 이용하면 가능하다.  

특성정보는 문법구조도 아니고 키워드도 아니다. 그냥 C++ 프로그래머들이 따르는 구현 기법이며, 관례이다. 특성정보가 되려면 몇 가지 요구사항이 지켜져야 하는데, **기본제공 타입과 사용자 정의 타입에서 모두 돌아가야 한다.**  
그 말은, 어떤 타입 내에 중첩된 정보 등으로는 구현이 안 된다는 말이다.  
특성 정보를 다루는 표준적인 방법은 해당 특성정보를 템플릿 및 그 템플릿의 1개 이상의 특수화 버전에 넣는 것이다.  

반복자의 경우, 표준 라이브러리의 특성정보용 템플릿이 iterator_traits라는 이름으로 준비되어 있다.

iterator_trits<IterT> 안에는 IterT 타입 각각에 대해 iterator_category라는 이름의 typedef 타입이 선언되어 있다. 이렇게 선언된 typedef 타입이 바로 IterT의 반복자 범주를 가리키는 것이다.  

이 클래스는 반복자 범주를 두 부분으로 나누어 구현한다. 첫 번째는 사용자 정의 반복자 타입에 대한 구현으로, iterator_category라는 이름의 typedef 타입을 내부에 가질 것을 요구사항으로 든다.  
이때 typedef 타입은 해당 태그 구조체에 대응되어야 한다. 

<pre class="prettyprint">
template < ... >
public:
  class iterator {
     public:
       typedef random_access_iterator_tag iterator_category;
  }
</pre>

 이 iterator 클래스가 내부에 지닌 중첩 typedef 타입을 따라한 것이 iterator_traits이다.

<pre class="prettyprint">
template<typename IterT>
struct iterator_traits
</pre>

보다시피, iterator_traits는 구조체 템플릿이다. 특성정보는 항상 구조체로 구현하는 것으로 굳어져 있다.  

특성정보 클래스의 설계 및 구현 방법
1. 다른 사람이 사용하도록 열어 주고 싶은 타입 관련 정보를 확인한다
2. 그 정보를 식별하기 위한 이름을 선택한다.
3. 지원하고자 하는 타입 관련 정보를 담은 템플릿 및 그 템플릿의 특수화 버전 (예: iterator_traits) 를 제공한다.

<pre class="prettyprint">
template<typename IterT, typename DistT>
void advance(IterT& iter, DistT d) 
{
    if(typeid(typename std::iterator_traits<IterT>::iterator_category) == 
    typeid(std::random_access_iterator_tag))
    ...
}
</pre>

이 코드는 문제가 있다.
- IterT의 타입은 컴파일 도중에 파악되기 때문에, iterator_traits<IterT>를 파악할 수 있는 때도 컴파일 또중이다. 하지만 if문을 사용하면 프로그램 실행 중에 평가된다.  

이것을 대신하는 방법은 오버로딩이다. 

<pre class="prettyprint">
template<typename IterT, typename DistT> 
void doAdvance(IterT& iter, DistT d, std::random_access_iterator_tag) {
    iter += d;
}

template<typename IterT, typename DistT> 
void doAdvance(IterT& iter, DistT d, std::bidirectional_iterator_tag) {
    if (d >= 0) { while(d--) ++iter; }
    else { while (d++) --iter; }
}


template<typename IterT, typename DistT> 
void doAdvance(IterT& iter, DistT d, std::input_iterator_tag) {
    if (d < 0) {
        throw std::out_of_range("Negative distance");
    }
    while (d--) ++iter;
}

</pre>

## 템플릿 메타 프로그래밍
컴파일 도중에 실행되는 템플릿 기반의 프로그램을 작성하는 일을 말한다.  
그 말은, C++ 컴파일러가 실행시키는 C++로 만들어진 프로그램이라는 뜻이다.  
C++은 템플릿 메타프로그래밍을 염두에 두고 설계되지는 않았지만 1990년대 초에 TMP 개념이 발굴되고 유용성이 하나 둘 드러나면서 마침내 C++ 언어 및 표준 라이브러리에 TMP를 용이하게 만드는 확장요소가 추가될 여지까지 생겼다.  

TMP에 있는 엄청난 강점 두가지는 다음과 같다.
- 다른 방법으로는 까다롭거나 불가능한 일을 굉장히 쉽게 할 수 있다.
- 템플릿 메타프로그램은 C++ 컴파일이 진행되는 동안에 실행되기 때문에, 기존 작업을 런타임 영역에서 컴파일 타임 영역으로 전환할 수 있다. 따라서 일반적으로 프로그램 실행 도중에 잡혀 왔던 몇몇 에러들을 컴파일 도중에 찾을 수 있다.

또한 TMP를 써서 만든 C++ 프로그램이 확실히 모든 면에서 효율적일 여지가 많다. 컴파일 타임에 동작을 다 해 가지고 오기 때문에 실행 코드가 작아지고, 실행 시간도 짭아지며, 메모리도 적게 잡아먹는다. (하지만 컴파일 시간은 길어진다.)





출처: Effective C++ 항목 48