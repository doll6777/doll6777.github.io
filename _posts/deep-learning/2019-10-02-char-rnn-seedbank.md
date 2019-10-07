---
layout: post
title: SeedBank Char-RNN (ing..)
excerpt: "Deep Learning"
tags: [deep learning, programming, dl]
permalink: /deep-learning/:year/:month/:day/:title/
category : 딥러닝
---

딥러닝 스터디를 진행하며 SeedBank의 Char-RNN에 대하여 발표하게 되었다. SeedBank 에서는 다양한 딥러닝 프로젝트들을 웹상에서 실행 가능하고 학습할 수 있다.

> https://research.google.com/seedbank/seed/charrnn


# Char-RNN
문자열을 사용하여 문자열을 생성해낸다. 이것을 RNN을 사용하여 구현할 수 있다. RNN이 뭔지 먼저 알아보자.

# RNN(Recurrent Neural Network)
RNN은 시퀀스 데이터를 처리하는 모델이다. 시퀀스란 순서대로 처리하는 것을 뜻하는데, 특히 음성인식이나 자연어처리 등이 포함된다. 이런 데이터들은 앞뒤 문맥을 이해해야 해석할수 있기 때문이다.  

![image](/assets/2019-10-02-char-rnn-seedbank/diags.jpeg)

RNN의 가장 간단한 모델은 바닐라(Vanilla) 모델이다. 이는 one-to-one 모델이다. 

![image](/assets/2019-10-02-char-rnn-seedbank/2.png)
![image](/assets/2019-10-02-char-rnn-seedbank/1.png)

하나의 입력을 받으면 h라는 상태에 값을 저장해두고, 다음 노드의 값을 계산할 때 이전에 저장했던 상태 h의 값을 적용해서 계산한다.  

# LSTM이란?
RNN은 관련 정보와 그 정보를 사용하는 지점 사이 거리가 멀 경우 그래디언트가 점점 줄어 학습능력이 크게 저하되는 것으로 알려져 있다.  
이를 Vanishing Gradient Problem 이라고 한다. 이 문제를 극복하기 위해서 고안된 것이 LSTM 이다.  

> LSTM이란 RNN의 hidden state에 cell-state를 추가한 구조이다.
![image](/assets/2019-10-02-char-rnn-seedbank/3.png)

# Char-RNN 실행해보기
