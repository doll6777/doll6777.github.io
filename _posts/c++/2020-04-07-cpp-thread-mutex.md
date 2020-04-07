---
layout: post
title: C++ 스레드와 뮤텍스 (recursive_mutext), 데드락 방지
tags: [c++, programming, effective]
category : C++
---

C++에서 스레드를 만드는 방법은 다음과 같다.  
C++의 람다를 사용하여 5개의 스레드를 만든다.

<pre class="prettyprint">
#include &lt;thread&gt;
using namespace std;

auto main() -&gt; int {

    thread threads[5];

    for(int i=0; i&lt;5; i++) {
        threads[i] = thread([]() {
            cout &lt;&lt; Thread ID: &lt;&lt; this_thread::get_id() &lt;&lt; endl;
        })
    }

}
</pre>

멀티스레드 프로그래밍을 하다보면 동시성 문제 (데드락) 가 생길 수 있다.  
lock을 사용하면 unlock을 걸어줘야 하지만 lock_guard를 사용하면 함수 스코프를 빠져나갈 때 락을 해제한다.  
보통 공유자원을 mutext를 사용하여 보호하지만, 만약 mutex를 하나만 사용한다면 데드락 상황이 생길 수 있다. 이때 recursive_mutext를 사용하여 여러번 락을 거는것이 가능하다.  

<pre class="prettyprint">
#include &lt;thread&gt;
#include &lt;mutex&gt;
#include &lt;iostream&gt;

using namespace std;

struct Math {
  recursive_mutext mtx;

  int m_content;
  Math() : m_content(1) {}

  void Multiplexer(int i) {
    lock_guard&lt;recursive_mutex&gt; lock(mtx);
    m_content *= i;
  }

  void Divisor(int i) {
    lock_guard&lt;recursive_mutex&gt; lock(mtx);
    m_content /= i;
  }

  void RunAll(int a) {
    lock_guard&lt;recursive_mutex&gt; lock(mtx);
    Multiplexer(a);
    Divisor(a);
  }
};

auto main() -&gt; int {
  Math math;
  math.RunAll(10);
  return 0;
}
</pre>

> 출처 : 모던C++로 배오는 함수형 프로그래밍