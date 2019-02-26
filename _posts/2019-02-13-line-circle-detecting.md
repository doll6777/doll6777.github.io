---
layout: post
title: Line, Circle Detecting
excerpt: "Computer Vision"
tags: [vision, programming, cv]
modified: 2019-01-28
comments: true
category : vision
---


# Line, Circle Detecting

## Hough Transgorm
xy 좌표에서 직선의 방정식을 파라미터 공간으로 변환하여 직선을 찾을 수 있다.

y = ax+b를  
b = -xa+y 로 바꿔 쓸수 있다.

허프 변환을 이용하여 직선의 방정식을 찾으려면 xy 공간에서 에지로 판별된 모든 점을 이용하여 ab 파라미터 공간에 직선을 표현하고, 직선이 많이 교차되는 좌표를 찾으면 된다. 많이 교차하는 점은 축적 배열(accumulation array)를 사용하여 찾을 수 있다.