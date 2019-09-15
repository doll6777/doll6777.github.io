---
layout: post
title: Kaggle DeepLearning 강의, 문제풀기 (ing..)
excerpt: "Deep Learning"
tags: [deep learning, programming, dl]
permalink: /deep-learning/:year/:month/:day/:title/
category : 딥러닝
---

> 본 포스팅은 https://www.kaggle.com/learn/deep-learning 강의를 학습하고 정리하였습니다.

# 1. Intro to DL for Computer Vision
텐서플로우와 케라스에 대해 설명한다. 텐서플로우는 유명한 딥러닝 툴이고, 케라스는 유명한 딥러닝 모델을 구체화하는 API 또는 인터페이스이다.  
케라스는 텐서플로우에서 돌아갈 수 잇는 딥러닝 모델을 만들어준다. 

이 강의에서 다룰 것은 딥러닝에서 어떻게 이미지를 인식하는지 배워본다. 이미지는 간단하게 매트릭스(행렬)로 표현한다. 흑백 이미지의 경우에는 행렬 하나에 이미지의 밝기값을 담고 있을 것이고,  
컬러 이미지의 경우에는 다차원 배열로 구성되며 R,G,B 채널로 이루어져 있다.  

이 강의에서 말하는 'tensor'의 경우에는 보통 다차원 행렬을 의미하지만 나중에 더 자세히 다룬다.  
컨볼루션이란 작은 'tensor'이고 필터라고도 부르는데 작은 컨볼루션 필터를 이미지에 거치고 나면 특정한 패턴을 추출할 수 있기 때문이다.  

## Exercise 1
이것은 horizontal line을 찾는 컨볼루션 필터이다. vertical line을 찾으려면 행렬을 어떻게 변경해야 하는가?
![image](/assets/2019-09-15-kaggle-dl-problem/1.png)

이렇게 -1의 위치를 세로로 바꿔주면 된다. 
![image](/assets/2019-09-15-kaggle-dl-problem/2.png)

## Exercise 2
지금까지 봤던 컨볼루션의 크기는 2x2 였다. 하지만 더 큰 컨볼루션 사이즈도 가질 수 있다.  
3x3이나 4x4도 가능한 것이다. 또한 정사각형 일 필요도 없다. 그렇다면 큰 사이즈의 컨볼루션이 더 좋을까? 꼭 그렇지만은 않다.  
다음 강의에서 딥러닝 모델이 다수의 컨볼루션을 합쳐서 복잡한 패턴을 찾아내는 것을 배울것이다.

# 2. Building Models From Convolutions
한 이미지를 가지고 여러개의 필터를 적용하는것이 가능하다. 이때 여러개의 필터를 한번에 적용하는 과정을 하나의 레이어라고 한다.  
이 레이어들이 다음 레이어의 컨볼루션으로 변경되어 전달되게 되고, 결국 더 복잡한 패턴들을 찾을 수 있다.  

![image](/assets/2019-09-15-kaggle-dl-problem/3.png)

## Exercise
캐글에 미리 학습되어 있는 모델을 가지고 이미지를 분류하는 것을 실습해본다. 먼저 이미지를 읽고 전처리를 하는 과정을 거친다.  
여기서는 ResNet50을 사용한다.  

- tensorflow keras의 load_image를 사용하여 이미지를 불러온다
- numpy의 array를 사용하여 이미지를 array로 변경, 필요한 전처리 한다
- keras 라이브러리를 통해 미리 학습된 가중치 파일을 통하여 ResNet50 모델을 생성한다
- 모델을 사용하여 예측한다
- decode_predictions를 통하여 결과를 시각화해 본다 (라벨 출력)

![image](/assets/2019-09-15-kaggle-dl-problem/4.png)


# 3. TensorFlow Programming
## Exercise 1
TV쇼 '실리콘 밸리'는 'Sea Food'라는 음식을 구분하는 앱을 가지고 있다. 이것을 구현해 보자.

- 이미지 패스를 만든다. 핫도그와 핫도그가 아닌 그림들이 구분되어 있다.
- 데이터와 미리 학습된 모델을 불러오고, 예측한다
- 예측을 시각화한다

 이에대한 코드가 이미 구현되어 있고, 예측된 결과를 가지고 새로운 함수를 구현해본다.  
예측된 라벨을 가지고 핫도그인지 아닌지를 Boolean값으로 변환하는 함수를 만들어본다.  

![image](/assets/2019-09-15-kaggle-dl-problem/5.png)
![image](/assets/2019-09-15-kaggle-dl-problem/6.png)

## Exercise 2
모델의 정확도를 평가하는 함수를 작성하라.  

# 4. Transfer Learning
# 5. Data Augmentation
# 6. A Deeper Understanding of Deep Learning
# 7. Deep Learning From Scratch
# 8. Dropout and Strides for Larger Models