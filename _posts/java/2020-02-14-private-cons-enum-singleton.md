---
layout: post
title: Effective Java 2/E, private 생성자나 enum 자료형은 싱글턴 패턴을 따르도록 설계하라
excerpt: "Java"
tags: [programming, effective, java]
permalink: /android/:year/:month/:day/:title/
category : Java
---

### JDK 1.5 이전의 싱글턴 구현 방법
<pre class="prettyprint">
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis() {...}
    public void leaveTheBuilding() {...}
}
</pre>

<pre class="prettyprint">
public class Elvis {
    private static final Elvis INSTANCE = new Elvis();
    private static Elvis getInstance() { return INSTANCE; }
    public void leaveTheBuilding() {...}
}
</pre>

만약 이런 싱글턴 클래스를 직렬화 가능 클래스로 만들려면 모든 필드를 transient로 선언하고 readResolve 메소드를 추가해야 한다. 또한 클래스 선언에 implements Serialize를 추가해야 한다.  

### JDK 1.5 이후의 싱글톤 구현
<pre class="prettyprint">
    public enum Elvis {
        INSTANCE;
        public void leaveTheBuilding() {...}
    }
</pre>

이 접근법은 기능적으론 public 필드를 사용하는 구현법과 동등하지만 좀 더 간결하고, 직렬화가 자동으로 처리된다는 장점이 있다. 직렬화가 복잡해도 여러 객체가 생길 일이 없고, 리플렉션을 통한 공격에도 안전하다.  

**원소가 하나뿐인 Enum 자료형이야말로 싱글턴을 구현하는 가장 좋은 방법이다.**