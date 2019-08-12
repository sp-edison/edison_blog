---
title: "coulombdart"
date: "2019-08-08T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/coulombdart"
category: "Nano Physics"
tags: 
 - "Lorentz force"
 - "Runge-Kutta"
description: "Coulomb Dart Manual"
---
##Coulomb Dart 사용자 매뉴얼

###개요
질량 분석기에 사용되는 Lorentz force를 이용하여 질량 분석기 안으로 입사된 이온의 경로를 같은 크기의 전하를 가진 입자 3개로 조정하여 우리가 원하는 과녁 안을 지나가게 한다. Lorentz force와 Coulomb force에 관련된 이차 coupled 미분 방정식을 4차 Runge-Kutta 방법을 이용하여 해를 구한다.

주어진 전하를 조정하여 질량 분석기 안으로 입사된 이온의 경로를 임의로 조정할 수 있다.

###시뮬레이터 사용법
![Aspect ratio](/media/POST/000028/1.jpg)
시뮬레이션 수행시 필요한 파라미터를 입력하는 화면이 나온다. 시뮬레이션을 위해서는 총 6 개의 파라미터가 필요하고, 각 파라미터의 역할은 다음과 같다.

- Q1 : 첫 번째 전하
- Position_1- : 첫 번째 전하의 위치 (x, y, z) 
- Q2 : 두 번째 전하
- Position_2 : 두 번째 전하의 위치 (x, y, z) 
- Q3 : 세 번째 전하
- Position_3 : 세 번째 전하의 위치 (x, y, z)

여기서 입사한 이온은 Cs+ (mass=132.905 amu, 1 amu=1.6605x10-27kg)이고 Cs+의 전하 q=1.6022x10-19C, 입사 속도 v0=2.5x103m/s 이고 평면에 수직방향(+z 축)으로 걸린 자기장은 B=0.01T 이다.

![Aspect ratio](/media/POST/000028/2.jpg)

![Aspect ratio](/media/POST/000028/3.jpg)

![Aspect ratio](/media/POST/000028/4.jpg)

![Aspect ratio](/media/POST/000028/5.jpg)

![Aspect ratio](/media/POST/000028/6.jpg)

