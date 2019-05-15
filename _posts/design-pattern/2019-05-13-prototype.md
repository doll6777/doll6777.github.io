---
layout: post
title: Prototype (프로토타입 패턴)
excerpt: "Design Pattern"
tags: [design pattern, programming, software]
permalink: /design-pattern/:year/:month/:day/:title/
category : 디자인패턴
---

# Prototype Pattern

인스턴스를 생성할 때 클래스 이름을 지정하지 않고 인스턴스를 생성할 때도 있다.  
클래스로부터 인스턴스를 만드는 것이 아니라 인스턴스를 복사해서 새로운 인스턴스를 만든다.  

- 종류가 많아 클래스로 정리되지 않거나
- 클래스로부터 인스턴스 생성이 어렵거나
- framework와 생성할 인스턴스를 분리하고 싶을 때 

완구점에서 토끼 인형, 고양이 인형의 인스턴스를 만들어놓고 복사하여 만든뒤 사용한다고 생각하고 구현한 예제

### Main.java
<pre class="prettyprint">
public class Main {
    public static void main(String[] args) {
        Manager manager = new Manager();
        BunnyDoll bunnyDoll = new BunnyDoll("BND1011");
        CatDoll catDoll = new CatDoll("CDL1012");

        manager.register("my bunny doll", bunnyDoll);
        manager.register("my cat doll", catDoll);

        Product p1 = manager.create("my bunny doll");
        p1.use("tom");

        Product p2 = manager.create("my cat doll");
        p2.use("sally");
    }
}

</pre>

### Manager.java
<pre class="prettyprint">
public class Manager {
    private HashMap showcase = new HashMap();

    public void register(String name, Product proto) {
        showcase.put(name, proto);
    }

    public Product create(String protoname) {
        Product p = (Product)showcase.get(protoname);
        return p.createClone();
    }
}

</pre>

### Product.java
<pre class="prettyprint">
public interface Product extends Cloneable {
    public abstract void use(String s);
    public abstract Product createClone();
}
</pre>

### CatDoll.java
<pre class="prettyprint">
public class CatDoll implements Product {
    private String modelNumber;

    public CatDoll(String modelNumber) {
        this.modelNumber = modelNumber;
    }

    @Override
    public void use(String s) {
        System.out.println("catDoll modelNumber: " + modelNumber + ", name: " + s + "\"");
    }

    @Override
    public Product createClone() {
        Product p = null;

        try {
            p = (Product) clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }

        return p;
    }
}
</pre>

### BunnyDoll.java
<pre class="prettyprint">
public class BunnyDoll implements Product {
    private String modelNumber;

    public BunnyDoll(String modelNumber) {
        this.modelNumber = modelNumber;
    }

    @Override
    public void use(String s) {
        System.out.println("bunnyDoll modelNumber: " + modelNumber + ", name: " + s + "\"");
    }

    @Override
    public Product createClone() {
        Product p = null;
        try {
            p = (Product) clone();
        } catch(CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return p;
    }
}

</pre>