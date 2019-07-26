---
layout: post
title: Abstract Factory Pattern (추상 팩토리 패턴)
excerpt: "Design Pattern"
tags: [design pattern, programming, software]
permalink: /design-pattern/:year/:month/:day/:title/
category : 디자인패턴
---

### 추상 팩토리 패턴이란?
- 구체적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공하는 패턴이다.
- 많은 수의 연관된 서브 클래스를 특정 그룹으로 묶어 한번에 교체할 수 있도록 만든 디자인 패턴이다.

### 추상 팩토리 패턴의 장단점
#### 장점
- 인터페이스 보다는 구조체에 접근할 수 있는 코드를 제공한다.
- 확장에 매우 용이한 패턴으로 쉽게 다른 서브 클래스들을 확장할 수 있다.
- 기존 팩토리 패턴의 if-else 로직에서 벗어날 수 있게 해준다.

#### 단점
- 새로운 종류의 제품을 제공하기 힘들다. AbstractProduct나 AbstractFactory에 새로운 항목을 추가해야 하는 경우가 생길수 있는데 이것은 인터페이스를 수정해야 한다.

- 객체를 만들때 객체지향 원칙인 LSP (Liskov Substitution principle) 를 해칠 수 있기 때문에 주의해야 한다.


### UML 및 클래스 다이어그램

![pattern](https://upload.wikimedia.org/wikipedia/commons/a/aa/W3sDesign_Abstract_Factory_Design_Pattern_UML.jpg)

![pattern](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/Abstract_factory.svg/517px-Abstract_factory.svg.png)

![pattern](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/Abstract_factory_UML.svg/677px-Abstract_factory_UML.svg.png)

### 추상 팩토리 패턴을 사용하는 경우
대표적인 예로, OS에 따라 달라지는 GUI 변경과 같은 설정의 변경이 필요할 때 사용할 수 있다.

### Sample
<pre class="prettyprint">
import java.util.Random;

interface GUIFactory {
    Button createButton();
}

class WinFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WinButton();
    }
}

class OSXFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new OSXButton();
    }
}

interface Button {
    void paint();
}

class WinButton implements Button {
    public void paint() {
        System.out.println("WinButton");
    }
}

class OSXButton implements Button {
    public void paint() {
        System.out.println("OSXButton");
    }
}

class Application {
    Application(GUIFactory factory) {
        Button button = factory.createButton();
        button.paint();
    }
}

public class Main {
    public static void main(String[] args) {
        int randomAppearance = new Random().nextInt(2);
        new Application(createOsSpecificFactory(randomAppearance));
    }

    private static GUIFactory createOsSpecificFactory(int appearance) {
        switch (appearance) {
            case 0:
                return new OSXFactory();
            case 1:
                return new WinFactory();
            default:
                throw new IllegalArgumentException("unknown " + appearance);
        }
    }
}
</pre>

### 다른 패턴들과 비교
- factory 패턴과 다른 점은 factory를 좀 더 생산적으로 만들어 낼 수 있다는 점이다.
  - 인풋에 대해 factory class 안에서 if-else로 다른 sub-class를 반환하는 일련의 과정을 생략할 수 있다.

### 팩토리 메서드 패턴과 너무 헷갈리는데, 뭐가 다를까?

[팩토리 메서드 패턴과 팩토리 패턴의 차이 (StackOverflow)](https://stackoverflow.com/questions/5739611/what-are-the-differences-between-abstract-factory-and-factory-design-patterns)
많은 사람들이 헷갈려 하는 부분이고, 스택오버플로우에 잘 정리가 되어 있다.

#### 팩토리 메서드 패턴
- 하나의 메소드이다
- 하나의 아이템을 만들기 위해 쓰인다
- **상속(Inheritance)**을 사용하고 객체 생성을 핸들링하기 위하여 서브클래스에 의존한다.

<pre class="prettyprint">
class A {
    public void doSomething() {
        Foo f = makeFoo();
        f.whatever();   
    }

    protected Foo makeFoo() {
        return new RegularFoo();
    }
}

class B extends A {
    protected Foo makeFoo() {
        //subclass is overriding the factory method 
        //to return something different
        return new SpecialFoo();
    }
}
</pre>

#### 추상 팩토리 패턴
- 객체이다
- 관련된 아이템들의 묶음을 만들기 위하여 만들어진다.
- 여러개의 팩토리 메소드를 가지고 있다.
- 객체 **합성(Composition)**을 통하여 객체 생성을 한다.
- 객체를 생성 가능한 객체를 만드는 추상 메소드를 가진 베이스 클래스를 만든다.


<pre class="prettyprint">
class A {
    private Factory factory;

    public A(Factory factory) {
        this.factory = factory;
    }

    public void doSomething() {
        //The concrete class of "f" depends on the concrete class
        //of the factory passed into the constructor. If you provide a
        //different factory, you get a different Foo object.
        Foo f = factory.makeFoo();
        f.whatever();
    }
}

interface Factory {
    Foo makeFoo();
    Bar makeBar();
    Aycufcn makeAmbiguousYetCommonlyUsedFakeClassName();
}
</pre>

> 가장 큰 차이점은 각 패턴의 목적이 팩토리 메서드 패턴은 객체를 생성하는데 있지 않고, 추상 팩토리 패턴은 객체를 생성해야 한다는 것이다. 


### 참고한 페이지
- [위키피디아](https://en.wikipedia.org/wiki/Abstract_factory_pattern#Java_example)
- https://yukariko.github.io/designpattern/2016/08/19/abstract-factory.html  
- https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%ED%8C%A9%ED%86%A0%EB%A6%AC_%ED%8C%A8%ED%84%B4  
- https://donxu.tistory.com/entry/Abstract-Factory-Pattern%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%ED%8C%A8%ED%84%B4  