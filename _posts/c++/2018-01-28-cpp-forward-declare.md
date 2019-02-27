---
layout: post
title: 파일 사이의 의존성을 최대로 줄이자
tags: [c++, programming, effective]
category : C++
---

# 파일 사이의 컴파일 의존성을 최대로 줄이자

### 전방 선언
"#include" 문은 정의한 파일과 헤더 파일들 사이에 컴파일 의존성이란 것을 엮어 버린다. 그러면 헤더 파일 셋 중 하나라도 바뀌는 것은 물론이고 이들과 또 엮여 있는 헤더 파일들이 바뀌기만 해도, 관련된 파일들이 모두 컴파일되게 된다.


#### 전방 선언으로 생길 수 있는 문제
컴파일러가 컴파일 도중에 객체들의 크기를 전부 알아야 한다.
이러한 문제를 해결하려면 

> 핸들 클래스에서 어떤 함수를 호출하게 되어 있다면, 핸들 클래스에 
대응되는 구현 클래스 쪽으로 그 함수 호출을 전달해서 구현 클래스가 
실제 작업을 수행하게 만들어라


```
#include <string> // 표준라이브러리는 전방선언하지 않는다
#include <memory>

class PersonImpl;
class Date;
class Address;

class Person {
public:
    Person(const std::string& name, const Date& birthday, const Address& addr);
    std::string name() const;
    std::string birthDate() const;
    std::string address() const;
private:
    std::tr1::shared_ptr<PersonImpl> pImpl;
};


```

이렇게 인터페이스와 구현을 둘로 나누는 열쇠는 정의부에 대한 의존성을 선언부에 대한 의존성으로 바꾸어 놓는 데 있다.

* 컴파일 의존성을 최소화하는 작업의 배경이 되는 가장 기본적인 아이디어는 정의 대신에 선언에 의존하게 만들자는 것이다. 이 아이디어에 기반한 두 가지 접근 방법은 핸들 클래스와 인터페이스 클래스이다.
* 라이브러리 헤더는 그 자체로 모든 것을 갖추어야 하며 선언부만 갖고 있는 형태여야 한다.

> 핸들 클래스 방법 대신에 다른 방법을 쓰고 싶다면 Person을 특수 형태의 추상 기본 클래스, 이른 바 **인터페이스 클래스**로 만드는
방법도 생각해 볼 수 있다.

```
class Person {
public:
    virtual ~Person();

    static std::tr1::shared_ptr<Person> create(const std::string& name, const Date& birthday, const Address& addr);

    virtual std::string name() const = 0;
    virtual std::string birthDate() const = 0;
    virtual std::string address() const = 0;
}
```

```
class RealPerson: public Person {
public:
    RealPerson(const std::string& name, const Date& birthday,
    const Address& addr) : theName(name), theBirthDate(birthday), theAddress(addr) {}

    virtual ~RealPerson() {}

    std::string name() const;
    std::string birthDate() const;
    std::string address() const;

private:
    std::string theName;
    Date theBirthDate;
    Address theAddress;
}
```

```
    std::tr1::shared_ptr<Person> Person::create(const std::string& name, const Date& birthday, const Address& addr) {
        return std::tr1::shared_ptr<Person>(new RealPerson(name, birthday, addr));
    }
```


출처 : Effective C++ 항목 31