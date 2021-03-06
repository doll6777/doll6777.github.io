---
layout: post
title: 객체지향의 사실과 오해 (위키북스)
excerpt: "book"
tags: [book]
permalink: /etc/:year/:month/:day/:title/
category : Book
---
![image](http://image.yes24.com/momo/TopCate511/MidCate005/51040273.jpg)
객체지향의 사실과 오해 : 역할, 책임, 협력 관점에서 본 객체지향  
조영호 지음  
위키북스

책을 이전 직장에 다닐때 구매하였으나 계속 미루고 있다가 최근에 읽게 되었다.  
학부시절 교과서에서만 배우던 객체지향의 개념을 이상한 나라의 앨리스에 은유하며 설명해 준다. 강의 시간에 주로 객체지향을 설명할 때 사용하는 <u> '현실 세계를 모방한 것' 이 객체 지향이다! </u> 라는 개념을 깨주었다. 이런 개념이 어렵게 느껴질 수 있지만 이상한 나라의 앨리스에 비유하여 이해를 도왔다.


# 객체지향의 개념
 객체지향 어플리케이션은 실세계를 모방하지 못한다. 오히려 새로운 세계를 창조하는 것이다. 많은 책들이 객체지향의 개념을 설명할 때 모방이라는 개념을 사용하는 이유는 객체지향의 기본 사상을 이해하고 학습하는 데 매우 효과적이기 때문이다.  

 훌륭한 객체지향 설계자가 되지 위해서는 클래스의 관점에서 메시지를 주고받는 객체의 관점으로 사고의 중심을 전환하는 것이다. 어떤 클래스가 필요한지는 중요하지 않다.  

 - 객체지향이란 시스템을 상호작용하는 자율적인 객체들의 공동체를 바라보고 객체를 이용해 시스템을 분할하는 방법이다
 - 자율적인 객체란 상태와 행위를 함께 지니며 **스스로 자기 자신을 책임지는 객체를 의미한다** --> 객체는 자율적인 존재다!
 - 객체는 시스템의 행위를 구현하기 위해 다른 객체와 협력한다. 각 객체는 협력 내에서 정해진 역할을 수행하며 역할은 관련된 책임의 집합이다
 - 객체는 다른 객체와 협력하기 위해 메시지를 전송하고, 메시지를 수신한 객체는 메시지를 처리하는 데 적합한 메서드를 자율적으로 선택한다

# 객체의 정의
> 객체란 식별 사능한 개체 또는 사물이다. 객체는 자동차처럼 만질 수 있는 구체적인 사물일 수도 있고, 시간처럼 추상적인 개념일 수도 있다. 객체는 구별 가능한 식별자, 특징적인 행동, 변경 가능한 상태를 가진다. 소프트웨어 안에서 객체는 저장된 상태와 실행 가능한 코드를 통해 구현된다.    
## 상태
상태는 특별 시점에 객체가 가지고 있는 정보의 집합으로 객체의 구조적 특징을 표현한다. 객체의 상태는 객체에 존재하는 정적인 프로퍼티와 동적인 프로퍼티 값으로 구성된다. 객체의 프로퍼티는 단순한 값과 다른 객체를 참조하는 링크로 구분할 수 있다. 

## 행동
행동이란 외부의 요청 또는 수신된 메시지에 응답하기 위해 동작하고 반응하는 활동이다. 행동의 결과로 객체는 자신의 상태를 변경하거나 다른 객체에게 메시지를 전달할 수 있다. 객체는 행동을 통해 다른 객체와의 협력에 참여하므로 행동은 외부에 가시적이어야 한다. 

## 식별자
식별자란 어떤 객체를 다른 객체와 구분하는 데 사용하는 객체의 프로퍼티다. 값은 식별자를 가지지 않기 때문에 상태를 이용한 동등성 검사를 통해 두 인스턴스를 비교해야 한다. 객체는 상태가 변경될 수 있기 때문에 식별자를 이용한 동일성 검사를 통해 두 인스턴스를 비교할 수 있다.  

# 타입과 추상화
## 추상화
어떤 양상, 세부 사항, 구조를 좀 더 명확하게 이해하기 위해 특정 절차나 물체를 의도적으로 생략하거나 감춤으로써 복잡도를 극복하는 방법이다.  
복잡성을 다루기 위해 추상화는 두 차원에서 이뤄진다
- 첫 번째 차원은 구체적인 사물들 간의 공통점은 취하고 차이점은 버리는 일반화를 통해 단순하게 만드는 것이다
- 두 번째 차원은 중요한 부분을 강조하기 위해 불필요한 세부 사항을 제거함으로써 단순하게 만드는 것이다  
모든 경우에 추상화의 목적은 복잡성을 이해하기 쉬운 수준으로 단순화하는 것이라는 점을 기억하라

## 타입
타입은 개념과 동일하다. 따라서 타입이란 우리가 인식하고 있는 다양한 사물이나 객체에 적용할 수 있는 아이디어나 관념을 의미한다. 어떤 객체에 타입을 적용할 수 있을 때 그 객체를 타입의 인스턴스라고 한다. 타입의 인스턴스는 타입을 구성하는 외연인 객체 집합의 일원이 된다.  

> 개념은 특정한 객체가 어떤 그룹에 속할 것인지를 결정한다. 객체의 분류 장치로서 개념이 작동한다.  

# 역할, 책임, 협력
## 협력
협력의 본질은 요청과 응답으로 연결되는 사람들의 네트워크다. 협력은 한 사람이 다른 사람에게 도움을 요청할 때 시작된다.요청을 받은 사람은 일을 처리한 후 요청한 사람에게 필요한 지식이나 서비스를 제공하는 것으로 요청에 응답한다.  

## 책임
어떤 객체가 어떤 요청에 대해 대답해 줄 수 있거나, 적절한 행동을 할 의무가 있는 경우 해당 객체가 책임을 가진다고 말한다. 책임은 객체의 외부에 제공해 줄 수 있는 정보(아는 것의 측면)과 외부에 제공해 줄 수 있는 서비스(하는 것의 측면)의 목록이다. 따라서 책임은 객체의 공용 인터페이스를 구성한다.  

## 역할
역할은 책임의 집합이다. 협력 내에서 다른 객체로 대체할 수 있음을 나타내는 일종의 표식이다. 협력 안에서 역할은 "이 자리는 해당 역할을 수행할 수 있는 어떤 객체라도 대신할 수 있습니다" 라고 말하는 것과 같다.  
> 객체지향 프로그래밍에서 행동은 수행할 책임을 지닌 객체에게 전송된 메시지에 의해 시작된다. 메시지는 행동에 대한 요청을 표현하고, 요청을 수행하는 데 필요한 추가적인 정보를 인자를 통해 전달한다. 수신자는 메시지를 수신하는 객체를 가리킨다. 수신자가 메시지를 받아들인다는 것은 해당 행동을 수행할 책임을 받아들인다는 것을 의미한다. 객체는 메시지에 대한 응답으로 요청을 만족하기 위한 어떤 메서드를 수행할 것이다

# 객체 설계
> 안정적인 모데인 모델을 기반으로 시스템의 기능을 수현하라. 도메인 모델과 코드를 밀접하게 연관시키기 위해 노력하라. 그것이 유지보수하기 쉽고 유연한 객체지향 시스템을 만드는 첫걸음이 될 것이다

유즈케이스는 단지 사용자가 바라보는 시스템의 외부 관점만을 표현한다. 유스케이스는 시스템이 외부에 제공해야 하는 행위만 포함하기 때문에 유스케이스로부터 시스템의 내부 구조를 유추할 수 있는 방법은 존재하지 않는다. **사실 유스케이스는 객체지향과도 상관이 없다**  

## 책임-주도 설계 방법
책임-주도 설계 방법은 시스템의 기능을 역할과 책임을 수행하는 객체들의 협력 관계로 바라보게 함으로써 두 가지 기본 재료인 유스케이스와 도메인 모델을 통합한다. 물론 책임-주도 설계를 위해 유스케이스와 도메인 모델이 반드시 필요한 것은 아니고 유스케이스와 도메인 모델이 책임-주도 설계에서만 사용되는 것은 아니다. 여기서 중요한 것은 견고한 객체지향 애플리케이션을 개발하기 위해서는 사용자의 관점에서 시스템의 기능을 명시하고, 사용자와 설계자가 공유하는 안정적인 구조를 기반으로 기능을 책임으로 변환하는 체계적인 절차를 따라야 한다는 것이다.  

# 설계 후 구현하기
> 객체지향 설계의 첫 번째 목표는 훌륭한 객체를 설계하는 것이 아니라 훌륭한 협력을 설계하는 것이다!

## 협력 찾기
협력에 필요한 객체의 종류와 책임, 주고받아야 하는 메시지에 대한 대략적인 윤곽을 잡는다. 도메인 모델 안에 책임을 수행하기에 적절한 타입이 있는지 살펴보라. 적절한 타입을 발견했다면 책임을 수행할 객체를 그 타입의 인스턴스로 만들라. 현실 속의 객체와 소프트웨어 객체가 완전히 동일할 수는 없겠지만 적어도 소프트웨어 객체에게 현실 객체와 유사한 이름을 붙여 놓으면 유사성을 통해 소프트웨어 객체가 수행해야 하는 책임과 상태를 좀 더 쉽게 유추할 수 있다. 

## 인터페이스 정리
객체가 수신한 메시지가 객체의 인터페이스를 결정한다. 각 객체를 협력이라는 문맥에서 떼어내어 수신 가능한 메시지만 추려내면 객체의 인터페이스가 된다. 객체에게 책임을 할당하고 인터페이스를 결정할 때는 가급적 객체 내부의 구현에 대한 어떤 가정도 하지 말아야 한다. 객체가 어떤 책임을 수행해야 하는지를 결정한 후에야 책임을 수행하는 데 필요한 객체의 속성을 결정하라. 이것이 객체의 구현 세부 사항을 객체의 공용 인터페이스에 노출시키지 않고 인터페이스와 구현을 깔끔하게 분리할 수 있는 기본적인 방법이다.

## 구현하기
클래스의 인터페이스를 식별했으므로 이제 오퍼레이션을 수행하는 방법을 메서드로 구현한다. 이 과정에서 인터페이스가 변경될 수 있다. 

> 소프트웨어는 항상 변한다. 설계는 변경을 위해 존재한다. 여러 개의 클래스로 기능을 분할하고 클래스 안에서 인터페이스와 구현을 분리하는 이유는 변경이 발생했을 때 코드를 좀 더 수월하게 수정하길 간절히 원하기 때문이다. 소프트웨어 클래스가 도메인 개념을 따르면 변화에 쉽게 대응할 수 있다.  

## 인터페이스와 구현을 분리하라
명세 관점과 구현 관점이 뒤섞여 머릿속을 함부로 어지럽히지 못하게 하라. 인터페이슨느 구현 세부 사항을 노출하면 안된다. 마틴 파울러는 개념적인 관점과 명세 관점 사이는 중요하지 않은 경우가 많지만, 명세 관점과 구현 관점을 분리하는 것은 매우 중요하다고 주장했다.  

캡슐화를 위반하여 구현을 인터페이스 밖으로 노출하지 말고, 인터페이스와 구현을 명확하게 분리하지 않고 흐릿하게 섞어도 안된다.  

