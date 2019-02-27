---
layout: post
title: Edge Detecting Algorithm
excerpt: "Computer Vision"
tags: [vision, programming, cv]
category : vision
---


# Edge Detecting Algorithm

에지 검출은 컴퓨터 비전 시스템에서 객체 판별을 위한 전처리로 사용  
영상은 2차원 평면에서 정의되었기 때문에 가로 방향과 세로 방향으로 각각   미분이 필요하다. (x방향으로만 편미분, y방향으로만 편미분)

## Sobel Mask
미분을 필터 마스크를 사용하여 수행한다.  
보통 잡음을 줄이기 위하여 큰 크기의 마스크를 사용한다. 
OpenCV Sobel() 함수 지원

## Scharr Mask
소벨 마스크보다 정확한 미분 계산을 수행하는 것으로 알려져 있다 OpenCV Scharr() 함수 지원


### 마스킹 후 에지를 찾는 법
x방향으로 마스킹(미분)한 결과, y방향으로 마스킹(미분)한 결과를 모두 사용하고 이것을 벡터로 표현한다. (크기와 방향을 가짐)  
이 벡터는 그래티언트라고 표현하며 에지 여부는 그래디언트 크기가 임계값보다  큰 픽셀을 찾는것이다.


### Sobel Matrix

3x3 크기의 행렬을 사용하여 연산을 하였을때 중심을 기준으로 각방향의 앞뒤의 값을 비교하여서
변화량을 검출하는 알고리즘이다.

### Canny Filtering
