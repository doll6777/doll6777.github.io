---
layout: post
title: Logistic (regression) classification
excerpt: "Machine Learning"
tags: [machine learning, programming, dl]
permalink: /machine-learning/:year/:month/:day/:title/
category : 머신러닝
---

# Logistic Classification

## Regression
regression (회귀)란 선형 함수 H(x) = WX라는 Hypothesis의 W값을 정하는 것이다.
W값을 정하는 방법은 실제 정답 값과 예측 값의 오차의 정도를 계산하여 오차가 최소가 되는 W를 찾는 것이다.
이때 오차의 값을 계산하는 함수는 cost함수라고 정의한다. 또한 cost함수의 값이 최소가 되는 지점의 W를 찾는 방법은
Gradient decent라는 방법으로 찾는다.

### gradient descent algorithm
그라디언트 디센트 알고리즘은 기울기 방향으로 learning rate(움직이고 싶은 만큼) 조금씩 움직여 보며 최적의 값을
찾는 방법이다.

## Classification
classification이란 값을 범주로 구별하는 것이다. 이진 분류는 0,1 인코딩이라는 방법으로 0과 1으로 표현할 수 있다.
이렇게 0과 1로 구별하는 것을 linear regression으로 구현할 수 있을까?  

그렇다면 결과값을 1보단 작게, 0보단 크게 변환해야 한다.  

### sigmoid function
선형회귀로 나온 값을 sigmoid라는 함수를 한번 더 적용하여 0과 1사이의 값으로 변환한다.

### cost function
logistic classification의 cost function은 선형 회귀와 같이 표현될까? 실제의 값과 예측값 차이의 제곱의 평균이다.
하지만 이렇게 표현할 수 없다. 왜냐하면 이런 cost function을 그려서 gradient descent를 적용하면 convex function이 아니기 때문이다.  

이 경우에는 local minimum을 찾는 방법을 진행해야 한다.  그래서 y=1인 경우와 y=0인 경우의 함수를 다르게 표현한다.  
그런 후 gradient descent 알고리즘을 실행하면 W값을 찾을 수있다.

## multinomial classification
x값이 하나가 아닌 경우에는 어떻게 할까? x1, x2.. 여러개가 존재하는 경우 말이다.  
이는 매트릭스로 표현할수 있다.  