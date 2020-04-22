---
layout: post
title: SingleTon Double-Checked Locking Pattern (DCLP)
excerpt: "Design Pattern"
tags: [design pattern, programming, software]
permalink: /design-pattern/:year/:month/:day/:title/
category : 디자인패턴
---

더블 체크드 락킹은 소프트웨어 디자인 패턴이다.  

싱글톤은 누구나 아는 흔한 디자인 패턴이지만 멀티 스레드 환경에서 구현하려면 신경을 써야한다.  

락을 얻기위한 오버헤드를 줄이기 위한 것이다. 락킹 구역에서 락킹이 꼭 필요한지 체크를 하고 락을 하게 된다. (Lazy initialization)

**하지만 이 방법은 다른 하드웨어나 언어로 작성될 경우에 안전하지 않을 수 있고, 안티패턴으로 간주되기도 한다.**

CPP
<pre class="prettyprint">
#include &lt;atomic&gt;
#include &lt;mutex&gt;

class Singleton {
 public:
  Singleton* GetInstance();

 private:
  Singleton() = default;

  static std::atomic&lt;Singleton*&gt; s_instance;
  static std::mutex s_mutex;
};

Singleton* Singleton::GetInstance() {
  Singleton* p = s_instance.load(std::memory_order_acquire);
  if (p == nullptr) {
    std::lock_guard&lt;std::mutex&gt; lock(s_mutex);
    p = s_instance.load(std::memory_order_relaxed);
    if (p == nullptr) {
      p = new Singleton();
      s_instance.store(p, std::memory_order_release);
    }
  }
  return p;
}
</pre>

JAVA
<pre class="prettyprint">
public class Singleton {
  private volatile static Singleton instance;
  private Singeton() {}
  
  public static Singleton getInstance() {
    if (instance == null)  {
        synchronized(Singleton.class) {
          if (instance == null) {
              instance == new Singleton();  
          }
        }
    }
    
    return instance;
  }
}
</pre>
