---
title: "gravityslingshot Manual"
date: "2019-07-25T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/gravityslingshot"
category: "Nano Physics"
tags: 
 - "wave condition"
 - "Plotly Viwer"
description: "Wave Simulation Manual"
---
#Gravity Slingshot 사용자 매뉴얼 
##개요
중력새총(Gravity-assist Slingshot)을 이용하여 우주선(spacecraft)을 최소한의 지구 발사 속력으로 태양계를 탈출할 수 있게 하고자 한다.

단, 태양, 지구, 목성으로만 이루어진 태양계를 가정하고 태양, 지구, 목성의 자전은 고려하지 않고 태양은 원점에 움직이지 않고 있고 지구와 목성의 공전 궤도면이 같은 평면에 있고(둘의 major axis 가 일치) 지구와 목성의 공전 궤도 타원의 왼쪽 초점에 태양이 공통으로 위치한다고 가정한다.

즉, 우주선이 목성같은 큰 행성의 궤도를 지날 때 행성의 인력에 끌려 들어가다 ‘바깥으로 튕겨져 나가듯’ 속력을 얻거나 잃는 것을 말한다(중력에 의한 탄성 충돌).

행성 뒤로 돌아가면 속력을 얻고 행성 앞으로 돌아가면 속력을 잃는다.

목성의 인력을 이용하기 위하여 지구 발사 속력에 한계치를 주고 우주선의 발사 각도, 지구의 위치, 목성의 위치를 초기 변수로 구하게 한다.

우주선, 지구, 목성의 움직임을 gif 로 그려 animation 을 구현하였고 우주선의 속력 변화 그래프도 함께 표시하였다.

##시뮬레이터 사용법
![Aspect ratio](/media/POST/00004/0.jpg)

- V0 : 우주선의 초기 발사 속력(태양기준, km/s)
- Thetav0 : 우주선의 초기 발사 각도 (지구를 중심으로 +x 축에서 반시계 방향은 +, 시계방향은 -, 단위 : degree)
- thetaE0 : 지구의 초기 위치 (각도로 표시, 위치와 속력은 이 각도로부터 계산됨, +x 축에서 반시계 방향은 +, 시계방향은 -, 원점 중심, 단위 : degree)
- thetaJ0 : 목성의 초기 위치 (각도로 표시, 위치와 속력은 이 각도로부터 계산됨, +x축에서 반시계 방향은 +, 시계 방향은 -, 원점 중심, 단위 : degree)

![Aspect ratio](/media/POST/00004/1.jpg)

![Aspect ratio](/media/POST/00004/2.jpg)

![Aspect ratio](/media/POST/00004/3.jpg)


