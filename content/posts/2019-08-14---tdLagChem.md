---
title: "tdLagChem"
date: "2019-08-14T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/tdLagChem"
category: "Computational Chemistry"
tags: 
 - "Brownian dynamics"
 - "density"
 - "gaussian wavepacket"
description: "1차원 시간의존 슈레딩거 방정식 Manual"
---

##1차원 시간의존 슈레딩거 방정식 에 대한 계산 및 이해
##학습목표
###시간에 의존하는 슈레딩거 방정식에 대한 이해와 해석 
- 포텐셜에 따른 밀도의 변화를 이해
- 밀도의 변화에 따른 해석

##학습개요
####시간에 의존하는 슈레딩거 방정식의 계산 
- 시간에 의존하는 슈레딩거 방정식의 이론 
- 가우시안 파동 묶음
####1차원 시간에 의존하는 슈레딩거 방정식의 해석 
- 예시를 통한 밀도 변화의 해석

##프로그램 소개
####프로그램을 통한 1차원 슈레딩거 방정식의 계산 
- 시간에 의존하는 슈레딩거 방정식을 계산
####결과해석
- 시간에 의존하는 슈레딩거 방정식의 밀도변화를 해석

##1차원 시간의존 슈레딩거 방정식
###시간에 의존하는 1차원 슈레딩거 방정식
$$
i\hbar\frac{\partial}{\partial t}\Psi=E\Psi
$$

1차원에서 시간과 거리에 의존하는 함수 무관하다고 가정하면 고유상태를 알 수 있다.

1차원에서 시간에 의존하는 슈레딩거 방정식을 풀게 되면 시간에 따른 밀도 변화를 알 수 있다.

###가우시안 파동 묶음
$$
\Psi(x)=Ne^{ar^2}
$$

불확정성의 원리 때문에 어떤 위치에 있는 입자의 상태를 표시하기 위해서는 파동 묶음이 필요하다.

![Aspect ratio](/media/POST/000041/0.jpg)

###1차원 슈레딩거 방정식의 포텐셜에 따른 결과 예측 
1차원에서는 포텐셜을 변화시키면 시간에 따른 밀도 변화를 예측할 수 있다.

![Aspect ratio](/media/POST/000041/1.jpg)

###입력 양식(1) – 가우시안 파동묶음
- points = 1000
- length = 10
- time_step = 0.05
- total_time = 5
- y_limit = 1
- highest_value = 0.3
- x_0=0
- potential = 0.5*x*x

###입력 정보
- points : 전체길이를 적분하기 위해 grid로 나누는 항
- length : 전체 길이
- time_step : 시간 간격
- total_time : 총 시간
- y_limit: 그래프의 y 축의 값을 결정
- highest_value: 가우시간 파동묶음에서 최대값을 결정
- x_0:시간에대한진동이시작되는지점
- potential : 원하는 포텐셜을 입력 예) 조화 진동자

###입력 양식(2) – 파동함수의 선형결합
- points = 1000
- length = 10
- time_step = 0.05
- total_time = 5
- y_limit = 1
- indice = 0, 1
- coef=1,1
- potential = 0.5*x*x

###입력 정보
- points : 전체길이를 적분하기 위해 grid로 나누는 항
- length : 전체 길이
- time_step : 시간 간격
- total_time : 총 시간
- y_limit: 그래프의 y 축의 값을 결정
- indice: 원하는 상태의 파동함수를 선택할 수 있다.
- coef: 파동함수의 선형결합에서의 상수값
- potential : 원하는 포텐셜을 입력 예) 조화 진동자

###출력 양식
동영상 파일(*.mp4)의 형태로 출력
![Aspect ratio](/media/POST/000041/2.jpg)

##Simulation Manual
![Aspect ratio](/media/POST/000041/3.jpg)

####< input information >
- point: 길이의 적분을 위해 격자를 나누는 값 
- length: 전체 길이
- time_step
- total_time
- y_limit: 동영상에서 y축의 값을 조절
- indice: 원하는 상태의 파동함수를 선택
- coef: 파동함수의 선형결합에서의 상수값
- potential: 1차원의 포텐셜
ex) 조화 포텐셜, 모스 포텐셜

![Aspect ratio](/media/POST/000041/4.jpg)

![Aspect ratio](/media/POST/000041/5.jpg)

![Aspect ratio](/media/POST/000041/6.jpg)

##결과
다운로드 후, 압축을 풀면 *.mp4 형식의 파일로 결과를 볼 수 있다.

![Aspect ratio](/media/POST/000041/7.jpg)

1차원 시간의존 슈레딩거 방적식은 파동묶음의 전파를 보여준다.

1차원 문제에 대해서 풀 수 있다. 예를 들면, 상자 속 입자, 조화 포텐셜, 이중 우물 포텐셜 등. 파란 선은 밀도, 빨간 선은 진폭에 해당한다.

##예시(1)
###1. 조화 포텐셜(고유함수들의 선형결합)
![Aspect ratio](/media/POST/000041/8.jpg)

####< input format >
- point = 1000 1 2 
- length = 10
- time_step = 0.05
- total_time = 10
- y_limit = 1.0
- indice = 0,1
- coef = 1,1
- potential = 0.5*x*x

###2. 조화 포텐셜(가우시안 파동묶음)
![Aspect ratio](/media/POST/000041/9.jpg)

####< input format > 
- point = 1000
- length = 50 
- time_step = 0.05 
- total_time = 5 
- y_limit = 1.0 
- highest_value = 0.3 
- x_0 = 0
- potential = 0.5*x*x

###3. 상자 속 입자(고유함수들의 선형결합)
![Aspect ratio](/media/POST/000041/10.jpg)

####< input format > 
- point = 1000
- length = 10 
- time_step = 0.05 
- total_time = 10 
- y_limit = 5.0
- indice = 0,1
- coef = 1,1
- potential = [0,1000000][x<-2 or x>2]

###4. 이중우물 포텐셜(고유함수들의 선형결합)
![Aspect ratio](/media/POST/000041/11.jpg)

####< input format > 
- point = 1000
- length = 10 
- time_step = 0.05 
- total_time = 30 
- y_limit = 5.0
- indice = 2,3
- coef = 1,1
- potential = 100*np.power(np.cos(x/4)-np.cos(0.605),2
