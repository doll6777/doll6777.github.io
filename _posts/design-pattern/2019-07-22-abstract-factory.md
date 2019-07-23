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

### 참고한 페이지
- https://en.wikipedia.org/wiki/Abstract_factory_pattern#Java_example  
- https://yukariko.github.io/designpattern/2016/08/19/abstract-factory.html  
- https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%ED%8C%A9%ED%86%A0%EB%A6%AC_%ED%8C%A8%ED%84%B4  
- https://donxu.tistory.com/entry/Abstract-Factory-Pattern%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%ED%8C%A8%ED%84%B4  