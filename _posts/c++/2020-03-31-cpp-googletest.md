---
layout: post
title: C++ GoogleTest 사용하여 유닛테스트 작성하기 (UnitTest)
tags: [c++, programming, effective]
category : C++
---


> [GoogleTest Github Link](https://github.com/google/googletest/blob/master/googletest/docs/primer.md)
### Why GoogleTest?

구글에서 만든 C++ Testing Framework.  

```
1. 테스트는 독립적이고 반복할 수 있어야 한다. 다른 테스트의 결과에 따라 결과가 달라지는 테스트는 좋은 테스트가 아니다. 구글테스느는 각각의 테스트를 분리하여 다른 오브젝트로 관리할 수 있도록 도와준다.
2. 테스트는 잘 구조화되고 테스트하는 코드를 잘 반영해야 한다. 구글테스트는 관련된 테스트를 test suite로 그룹화하여 데이터와 subroutine을 공유할 수 있도록 한다.  
3. 테스트는 재사용 가능하고 플랫폼 종속되지 않아야 한다. 구글테스트는 다른 OS에서도 돌 수 있도록 한다.  
4. 테스트가 Fail했을 때 왜 실패했는지를 보고해주기 때문에 버그를 쉽게 찾을 수 있다.  
5. 테스팅 프레임워크는 테스트 작성자들의 귀찮음을 덜어주고 테스트 자체에 집중할 수 있도록 만들어준다.  
6. 테스트는 빨라야한다. 구글테스트를 이용해서 shared resource를 테스트간에 재사용 할 수 있고, 한번만 실행되는 set-up/tear-down 메소드도 사용할 수 있다.  
```
구글테스트는 xUnit 아키텍쳐에 기반하고 있기 때문에 만약 JUnit이나 PyUnit과 익숙하다면 구글테스트에 빨리 익숙해질 수 있을 것이다.

> 환경: Ubuntu 16.04

### GoogleTest Build
<pre class="prettyclass">
$ git clone https://github.com/google/googletest
$ cd googletest
$ mkdir build
$ cd build
$ cmake ..
$ make
$ make install
</pre>

### Assertion
Fatal assertion (ASSERT_) 는 테스트 실패시 바로 테스트가 중단된다.  
Nonfatal assertion (EXPECT_) 는 테스트 실패해도 모든 테스트를 실행한다.  

보통은 EXPECT_를 쓰나, 이 테스트가 실해하면 무조건 바로 중단해야 할 경우는 ASSERT_를 쓴다.


### Basic Assertion
Fatal assertion            | Nonfatal assertion         | Verifies
-------------------------- | -------------------------- | --------------------
`ASSERT_TRUE(condition);`  | `EXPECT_TRUE(condition);`  | `condition` is true
`ASSERT_FALSE(condition);` | `EXPECT_FALSE(condition);` | `condition` is false


### Binary Comparison
Fatal assertion          | Nonfatal assertion       | Verifies
------------------------ | ------------------------ | --------------
`ASSERT_EQ(val1, val2);` | `EXPECT_EQ(val1, val2);` | `val1 == val2`
`ASSERT_NE(val1, val2);` | `EXPECT_NE(val1, val2);` | `val1 != val2`
`ASSERT_LT(val1, val2);` | `EXPECT_LT(val1, val2);` | `val1 < val2`
`ASSERT_LE(val1, val2);` | `EXPECT_LE(val1, val2);` | `val1 <= val2`
`ASSERT_GT(val1, val2);` | `EXPECT_GT(val1, val2);` | `val1 > val2`
`ASSERT_GE(val1, val2);` | `EXPECT_GE(val1, val2);` | `val1 >= val2`

### String Comparison
| Fatal assertion                | Nonfatal assertion             | Verifies                                                 |
| --------------------------     | ------------------------------ | -------------------------------------------------------- |
| `ASSERT_STREQ(str1,str2);`     | `EXPECT_STREQ(str1,str2);`     | the two C strings have the same content   		     |
| `ASSERT_STRNE(str1,str2);`     | `EXPECT_STRNE(str1,str2);`     | the two C strings have different contents 		     |
| `ASSERT_STRCASEEQ(str1,str2);` | `EXPECT_STRCASEEQ(str1,str2);` | the two C strings have the same content, ignoring case   |
| `ASSERT_STRCASENE(str1,str2);` | `EXPECT_STRCASENE(str1,str2);` | the two C strings have different contents, ignoring case |

### Simple Test
**TEST()** 라는 매크로를 쓴다. 이것은 결과를 리턴하지 않는 평범한 C++ 함수이다.  
첫번째 파라미터는 test suite의 이름이고, 두번째 파라미터는 구체적인 테스트의 이름이다.  
이름에는 underscores (_)가 들어갈 수 없다.  


<pre class="prettyprint">
TEST(TestSuiteName, TestName) {
  ... test body ...
}
</pre>

### Example
<pre class="prettyprint">
// Tests factorial of 0.
TEST(FactorialTest, HandlesZeroInput) {
  EXPECT_EQ(Factorial(0), 1);
}

// Tests factorial of positive numbers.
TEST(FactorialTest, HandlesPositiveInput) {
  EXPECT_EQ(Factorial(1), 1);
  EXPECT_EQ(Factorial(2), 2);
  EXPECT_EQ(Factorial(3), 6);
  EXPECT_EQ(Factorial(8), 40320);
}
</pre>

### Test Fixtures (Setup()/TearDown())
같은 데이터 설정을 여러 테스트에서 사용하고 싶을 때 필요한 방법이다.
::testing::Test를 상속받는다. Setup()과 같은 메소드들은 protected로 정의한다.  
또한 TEST() 대신 TEST_F()를 사용한다. 

<pre class="prettyprint">
TEST_F(TestFixtureName, TestName) {
  ... test body ...
}
</pre>

### Example
<pre class="prettyprint">
template <typename E>  // E is the element type.
class Queue {
 public:
  Queue();
  void Enqueue(const E& element);
  E* Dequeue();  // Returns NULL if the queue is empty.
  size_t size() const;
  ...
};
</pre>

<pre class="prettyprint">
class QueueTest : public ::testing::Test {
 protected:
  void SetUp() override {
     q1_.Enqueue(1);
     q2_.Enqueue(2);
     q2_.Enqueue(3);
  }

  // void TearDown() override {}

  Queue<int> q0_;
  Queue<int> q1_;
  Queue<int> q2_;
};
</pre>

<pre class="prettyprint">
TEST_F(QueueTest, IsEmptyInitially) {
  EXPECT_EQ(q0_.size(), 0);
}

TEST_F(QueueTest, DequeueWorks) {
  int* n = q0_.Dequeue();
  EXPECT_EQ(n, nullptr);

  n = q1_.Dequeue();
  ASSERT_NE(n, nullptr);
  EXPECT_EQ(*n, 1);
  EXPECT_EQ(q1_.size(), 0);
  delete n;

  n = q2_.Dequeue();
  ASSERT_NE(n, nullptr);
  EXPECT_EQ(*n, 2);
  EXPECT_EQ(q2_.size(), 1);
  delete n;
}
</pre>

### main function
gtest_main과 Link했다면 메인함수를 작성할 필요는 없다. gtest_main을 링크한다면 구글 테스트가 메인 함수 기본 구현을 제공하기 때문이다. 만약 자신만의 메인을 만들고 싶다면 RUN_ALL_TEST()를 리턴하게 해야 한다.

<pre class="prettyprint">
#include "this/package/foo.h"

#include "gtest/gtest.h"

namespace my {
namespace project {
namespace {

// The fixture for testing class Foo.
class FooTest : public ::testing::Test {
 protected:
  // You can remove any or all of the following functions if their bodies would
  // be empty.

  FooTest() {
     // You can do set-up work for each test here.
  }

  ~FooTest() override {
     // You can do clean-up work that doesn't throw exceptions here.
  }

  // If the constructor and destructor are not enough for setting up
  // and cleaning up each test, you can define the following methods:

  void SetUp() override {
     // Code here will be called immediately after the constructor (right
     // before each test).
  }

  void TearDown() override {
     // Code here will be called immediately after each test (right
     // before the destructor).
  }

  // Class members declared here can be used by all tests in the test suite
  // for Foo.
};

// Tests that the Foo::Bar() method does Abc.
TEST_F(FooTest, MethodBarDoesAbc) {
  const std::string input_filepath = "this/package/testdata/myinputfile.dat";
  const std::string output_filepath = "this/package/testdata/myoutputfile.dat";
  Foo f;
  EXPECT_EQ(f.Bar(input_filepath, output_filepath), 0);
}

// Tests that Foo does Xyz.
TEST_F(FooTest, DoesXyz) {
  // Exercises the Xyz feature of Foo.
}

}  // namespace
}  // namespace project
}  // namespace my

int main(int argc, char **argv) {
  ::testing::InitGoogleTest(&argc, argv);
  return RUN_ALL_TESTS();
}
</pre>

> 만약 더 공부해보고 싶다면 [Advanced Guide](https://github.com/google/googletest/blob/master/googletest/docs/advanced.md) 을 참고한다.