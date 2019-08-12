---
title: "TRUSSSolver"
date: "2019-08-08T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/TRUSSSolver"
category: "Computational Design"
tags: 
 - "Latin Hypercube Design"
 - "Reynolds"
description: "Latin Hypercube Design을 활용한 프로펠러 성능 추정식 기반의 프로펠러 형상 최적화 프로그램 Manual"
---
##2D Truss Solver 매뉴얼

##프로그램 설명
###프로그램 특징
![Aspect ratio](/media/POST/000026/0.jpg)

- 2차원 Truss 모델의 정적 해석을 수행
- 유저가 노드, 요소, 물성치, 외력, 경계조건을 텍스트 파일로 입력
- 해석 후 Compliance, 각 노드의 변위, 각 요소의 응력을 텍스트 파일(.dat)로, 변형 형상은 그림 파일(.png)로 저장
- 특별한 프로그램 없이 Compliance, 변위, 응력, 변형 형상 확인 가능

##Input 파일 설명
###Input 파일 구성
![Aspect ratio](/media/POST/000026/1.jpg)

###주의사항
- 모든 Label은 대문자로 작성
- 각 성분들의 구분은 공백으로 구분
- NODE, ELEMENT, MPROP, LOADS, BOUND의 순서대로 입력 (순서가 변경되면 해석 불가)

##프로그램 실행 방법
![Aspect ratio](/media/POST/000026/2.jpg)

![Aspect ratio](/media/POST/000026/3.jpg)

![Aspect ratio](/media/POST/000026/4.jpg)

![Aspect ratio](/media/POST/000026/5.jpg)

##해석 결과 확인
###compliance.dat 파일
![Aspect ratio](/media/POST/000026/6.jpg)

###total_length.dat 파일
![Aspect ratio](/media/POST/000026/7.jpg)

###displacement.dat 파일
![Aspect ratio](/media/POST/000026/8.jpg)

###stress.dat 파일
![Aspect ratio](/media/POST/000026/9.jpg)

###Deformed_shape.png
![Aspect ratio](/media/POST/000026/10.jpg)

###Deformed_shape_with_undeformed.png
![Aspect ratio](/media/POST/000026/11.jpg)

- 변형 형상을 .png 파일로 확인 가능
- 원은 노드, 선은 요소를 의미
- plot_scale값은 자동으로 계산하여 변형 형상을 plot 
- 파란 색은 변형 전 형상을 plot한 것

###Deformed_shape_with_stress.png
![Aspect ratio](/media/POST/000026/12.jpg)

- 변형 결과 형상에 각 요소의 응력에 따라 색을 다르게 해서 그린 것 
- 응력의 최소값은 푸른색, 최대값은 붉은색으로 표현