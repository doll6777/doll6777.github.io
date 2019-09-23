---
layout: post
title: Effective Java 2/E, 리플렉션 대신 인터페이스를 이용하라
excerpt: "Java"
tags: [programming, effective, java]
permalink: /android/:year/:month/:day/:title/
category : Java
---

리플렉션을 사용하면 메모리에 적재된 클래스의 정보를 가져와 활용할 수 있다. 메소드나 필드같은 것들을 가져올 수 있고, 메서드를 호출하거나 객체를 생성할 수 있다.  
하지만 리플렉션을 이용하면 단점이 따른다.  

- 컴파일 시점에 자료형을 검사함으로써 얻을 수 있는 이점들을 포기해야 한다
- 리플렉션 기능을 이용하는 코드는 가독성이 떨어진다
- 성능이 낮다. 리플렉션을 통한 메서드 호출은 일반적인 메서드 호출에 비해 성능이 훨씬 낮다.

> <font color='red'>명심할 점은, 일반적인 프로그램은 프로그램 실행 중에 리플렉션을 통해 객체를 이용하려 하면 안 된다</font>

리플렉션이 필요한 복잡한 프로그램이 몇개 있긴 하다. 클래스 브라우저, 객체 검사 도구, 코드 분석 도구 등이 그렇다.  

컴파일 시점에는 존재하지 않는 클래스를 이용해야 하는 프로그램 가운데 상당수는, 해당 클래스 객체를 참조하는 데 사용할 수 있는 인터페이스나, 상위 클래스는 컴파일 시점에 이미 갖추고 있는 경우가 많다.  

> <font color='red'>객체 생성은 리플렉션으로 하고, 객체 참조는 인터페이스나 상위 클래스를 사용하라</font>

<pre class="prettyprint">
public class Main {

    public static void main(String[] args)
    {
        Class&lt;?&gt; cl = null;
        try {
            cl = Class.forName(args[0]);
        } catch (ClassNotFoundException e) {
            System.out.println(&quot;Class Not found&quot;);
            System.exit(1);
        }

        Set s = null;
        try {
          s = (Set) cl.newInstance();
        } catch (IllegalAccessException e) {
            System.err.println(&quot;Class not accessible.&quot;);
            System.exit(1);

        } catch (InstantiationException e) {
            System.err.println(&quot;Class not instantiable.&quot;);
            System.exit(1);
        }

        s.addAll(Arrays.asList(args).subList(1, args.length));
        System.out.println(s);
    }
}
</pre>

드물게 쓰이긴 하지만, 리플렉션은 실행시점에 존재하지 않는 클래스나 메서드, 필드에 대한 종속성을 관리하는 데 적합하다.  
패키지의 버전이 여러 가지이고, 그 전부를 지원하는 또 다른 패키지를 구현해야 될 때 최소한 필요한 환경만 컴파일하고, 새로운 클래스나 메서드는 리플렉션을 통해 접근하게 하는 것이다.