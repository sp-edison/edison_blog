---
title: "WaveSimulation Manual"
date: "2019-07-25T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/000030"
category: "Nano Physics"
tags: 
 - "wave condition"
 - "Plotly Viwer"
description: "Wave Simulation Manual"
---

#첨단 사이언스교육 허브 개발 (EDISON) 사업 파동 발생 장치 시뮬레이션

##개요
- 파동 실험 장비에 의해 발생하는 줄의 파동을 시뮬레이션 할 수 있는 S/W 코드
- 다양한 초기 조건이 줄의 파동에 미치는 영향 및 공명 진동 여부

####시뮬레이션
- 외부에서 가해주는 힘에 의해 발생하는 줄의 파동
- 초기 조건의 영향 및 공명 진동 여부

####출력: 
- 시간에 따른 줄의 파동 운동
- 외부 진동수와 줄의 최대 진폭

![Aspect ratio](/media/POST/00003/0.jpg)

##이론
밀도 $\rho$, 장력 $T$의 줄의 파동 $y(x, t)$를 기술하는 방정식은 다음과 같다.

![Aspect ratio](/media/POST/00003/1.jpg)

실험 장비의 설정은 $x=0$에 줄을 묶여 있고, $x=L$ 지점의 줄은 $y(L, t)=f(t)$이다. 여기서 $f(t)$는 $F_{0} \sin (\omega t+\phi)$ 등의 임의 함수로 모터 등을 이용하여 줄을 흔들어주는 역할을 한다.

주어진 경계 조건에 대해 각 시간 $t_{i}$ 각 지점 $x_{l}$에서 파동 $y\left(x_{l}, t_{i}\right)$ 의 변화를 시간 $t_{i}$와 위치 $x_{l}$의 함수로 계산하는 시뮬레이션 S/W를 개발한다.

##SW 사용방법
![Aspect ratio](/media/POST/00003/2.jpg)

![Aspect ratio](/media/POST/00003/3.jpg)

![Aspect ratio](/media/POST/00003/4.jpg)

![Aspect ratio](/media/POST/00003/5.jpg)

##출력 예시 및 실습 예제
####줄에 발생하는 파동 운동 및 공명 진동 여부 확인 가능
![Aspect ratio](/media/POST/00003/6.jpg)

![Aspect ratio](/media/POST/00003/7.jpg)

####실습 예제 및 과제 결과물 예시

시뮬레이션 입력 값은 진동수, 줄의 길이, 장력, 줄의 선밀도, damping 계수, driving force의 진폭, driving force의 phase이다. 문제에서 명시한 변수 이외에는 기본 값을 사용하면 되고, 공명 진동수 및 Q factor는 amplitude에 대해 구하면 된다.

1. 줄의 길이가 1.5m, 장력이 0.8N, 줄의 선밀도가 0.0015일 때 세번째 공명진동수까지 찾고, 줄의 길이, 장력, 줄의 선밀도가 공명 진동수에 미치는 영향에 대해 각각 설명하시오. (damping이 없다고 가정하고, 나머지 값은 기본 값을 이용)
2. 위에서 주어진 조건에 damping계수가 0.001로 주어졌을 때 Q factor을 구하고, damping 계수가 증가할 때 Q factor가 어떻게 변하는지 설명하시오. (진동수에 대한 amplitude 그리프를 그리고, fitting하여 Q factor를 구하면 된다.)

####과제 결과물 예시
![Aspect ratio](/media/POST/00003/8.jpg)












