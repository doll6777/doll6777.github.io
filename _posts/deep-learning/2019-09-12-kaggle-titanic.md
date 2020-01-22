---
layout: post
title: Kaggle Titanic Deeplearning
excerpt: "Deep Learning"
tags: [deep learning, programming, dl]
permalink: /deep-learning/:year/:month/:day/:title/
category : 딥러닝
---

> 문제 링크 : <https://www.kaggle.com/c/titanic>
 
# 문제 개요
kaggle의 타이타닉은 딥러닝/머신러닝 입문자들이 캐글을 처음 시작할 때 다루는 문제이다.  
입문자들에게 타이타닉 튜토리얼을 <https://www.kaggle.com/alexisbcook/titanic-tutorial> 에서 확인할 수 있다.

# 문제 풀이

## 머신러닝 풀이
```
from sklearn.ensemble import RandomForestClassifier

y = train_data["Survived"]

features = ["Pclass", "Sex", "SibSp", "Parch"]
X = pd.get_dummies(train_data[features])
X_test = pd.get_dummies(test_data[features])

model = RandomForestClassifier(n_estimators=100, max_depth=5, random_state=1)
model.fit(X, y)
predictions = model.predict(X_test)

output = pd.DataFrame({'PassengerId': test_data.PassengerId, 'Survived': predictions})
output.to_csv('my_submission.csv', index=False)
print("Your submission was successfully saved!")
```

### 랜덤 포레스트
머신러닝의 방법중 하나인 의사결정 트리는 학습 결과의 성능과변동의 폭이 크기때문에 이런 단점을 극복하기 위해 랜덤포레스트가 등장하였다.  
랜덤 포레스트는 우리말로 무작위 숲인데, 이 무작ㅈ위 숲은 여러개의 무작위 의사결정트리로 이루어진 숲이라고 이해하면 된다.


## 딥러닝 풀이
> 참고 풀이 링크 : <https://www.kaggle.com/linxinzhe/tensorflow-deep-learning-to-solve-titanic>

### MLP (Multiple Layer Perceptron)
```
from collections import namedtuple

def build_neural_network(hidden_units=10):
    tf.reset_default_graph()
    inputs = tf.placeholder(tf.float32, shape=[None, train_x.shape[1]])
    labels = tf.placeholder(tf.float32, shape=[None, 1])
    learning_rate = tf.placeholder(tf.float32)
    is_training=tf.Variable(True,dtype=tf.bool)
    
    initializer = tf.contrib.layers.xavier_initializer()
    fc = tf.layers.dense(inputs, hidden_units, activation=None,kernel_initializer=initializer)
    fc=tf.layers.batch_normalization(fc, training=is_training)
    fc=tf.nn.relu(fc)
    
    logits = tf.layers.dense(fc, 1, activation=None)
    cross_entropy = tf.nn.sigmoid_cross_entropy_with_logits(labels=labels, logits=logits)
    cost = tf.reduce_mean(cross_entropy)
    
    with tf.control_dependencies(tf.get_collection(tf.GraphKeys.UPDATE_OPS)):
        optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate).minimize(cost)

    predicted = tf.nn.sigmoid(logits)
    correct_pred = tf.equal(tf.round(predicted), labels)
    accuracy = tf.reduce_mean(tf.cast(correct_pred, tf.float32))

    # Export the nodes 
    export_nodes = ['inputs', 'labels', 'learning_rate','is_training', 'logits',
                    'cost', 'optimizer', 'predicted', 'accuracy']
    Graph = namedtuple('Graph', export_nodes)
    local_dict = locals()
    graph = Graph(*[local_dict[each] for each in export_nodes])

    return graph

model = build_neural_network()
```

### 필요한 라이브러리

> <https://teddylee777.github.io/machine-learning/sklearn%EC%99%80-pandas%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%EA%B0%84%EB%8B%A8-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B6%84%EC%84%9D>

### sklearn
python의 머신러닝 라이브러리  
1. 지도학습, 비지도학습 모듈  
2. 모델 선택 및 평가 모듈  
3. 데이터 변환 및 데이터 불러오기 위한 모듈 
4. 계산 성능 향상을 위한 모듈

### pandas
```
import pandas as pd
data = pd.read_csv('data/train.csv')
print(data.describe())
print(data.info())
```

#### Feature Engineering
Feature Engineering은 머신러닝 알고리즘을 작동하기 위해 데이터에 대한 도메인 지식을 활용하여 특징을 만들어내는 과정이다. 데이터 사이언스 단계 중 데이터를 준비하는 과정 중 마지막 단계, 즉 모델에 데이터를 넣기 전 단계가 바로 Feature Engineering의 단계이다.

#### Loss Function
Cost Function이라고도 한다. 예측된 결과와 정답의 차이를 수치화한 것이다. 흔히 평균제곱오차(MSE)와  
교차 엔트로피 오차가 사용된다.  

#### Optimizer
딥러닝에서 Optimization은 학습속도를 빠르고 안정적이게 하는 것이라고 말할 수 있다.  

Neural network의 weight를 조절하는 과정에는 보통 'Gradient Descent'라는 방법을 사용한다.

- AdamOptimizer
- GradientDescent
- Stochastic Gradient Descent

#### Overfitting
과도하게 데이터에 대해 모델을 learning한 경우를 의미한다.  

#### Batch Normalization
기본적으로 Gradient Vanishing / Gradient Exploding이 일어나지 않도록 하는 아이디어 중의 하나이다.  
Batch Normalization의 경우 자체적인 regularization 효과가 있다. 나아가 Dropout을 제외할 수 있게 한다. (Dropout 효과아 Batch Normalization의 효과가 같다). 이를 제거함으로서 학습 속도도 향상된다.

알고리즘의 개요는, 각 feature별로 평균과 표준편차를 구해준 다음 normalize 해주고, scale factor와 shift factor를 시용하여 새로운 값을 만들어준다.

#### Batch, Epoch
다루어야 할 데이터가 너무 많기도 하고, 한번의 계산으로 최적화된 값을 찾는 것은 힘들기 때문에 이런 개념이 필요하다. 머신 러닝에서의 최적화를 할 때는 일반적으로 여러 번 학습 과정을 거친다.  
Epoch는 인공 신경망에서 전체 데이터 셋에 대해 forward pass/backward pass 과정을 거친 것을 말한다. 즉, 전체 데이터 셋에 대해 한 번 학습을 완료한것이다.  

batch size는 한 번의 batch마다 주는 데이터 샘플의 size이다. 