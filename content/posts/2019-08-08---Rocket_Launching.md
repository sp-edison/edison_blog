---
title: "Rocket_Launching"
date: "2019-08-08T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Rocket_Launching"
category: "Nano Physics"
tags: 
 - "Rocket Launching"
 - "LEO"
description: "인공위성 시뮬레이션 Manual"
---
##Rocket_Launching User Manual
###앱소개
지구에서부터 2단 로켓을 발사하여 지구 저궤도(LEO)에 진입하는 과정을 시뮬레이션한다. 로켓은 연료를 분사하며 받는 반작용을 통해서 추진력을 얻는다. 이 과정에서 로켓은 매순간 분사한 연료 만큼 질량이 감소하기 때문에 로켓의 움직임을 기술하는 것은 단순한 점입 자의 운동 방정식을 푸는 것 보다 어렵다. 여기에 3차원 공간에서 지구에 의해 받는 중력과 공기저항력 등을 고려하면 해석적으로 풀 수 없기 때문에 지구에서 발사된 로켓의 궤도를 예측하기 위해서는 수치계산을 통한 시뮬레이션이 필수적이다.

이 시뮬레이션에서는 1,2단 로켓의 질량, 연료의 질량, 연료 소모 비율, 분사된 연료의 질량을 사용자가 설정할 수 있다. 그리고 지구 자전에 의해 관성으로 가지고 있던 운동량 또한 계산에 포함되었다. 또한 로켓이 발사된 이후에 고도에 따른 연료 분사의 방향과 2단 로켓의 연소 시작 시간을 설정할 수 있다. 이러한 변수들을 어떻게 설정하는지에 따라 다양하게 나타나는 궤도의 모습, 시간에 따른 로켓의 고도와 속력 등을 그래프나 이미지를 통해 확인 해볼수있다.

###배경이론
로켓은 연료를 분사할 때 받는 반작용을 통해서 추진력을 얻음. 연료를 분사하며 질량 변화
![Aspect ratio](/media/POST/000029/0.jpg)

###Method
( 𝑟 ⃗,𝑣 ⃗ )를 Runge-Kutta method를 통해 수치 계산

###시뮬레이션 실행 방법
1. ScienceApp 선택
2. 시뮬레이션 생성 : ‘Run’을 눌러 시작. 시뮬레이션 명은 사용자가 임의로 설정
3. 입력포트 : 매뉴얼의 다음페이지부터 있는 시뮬레이션 입력 변수의 설명에 따라 설정을 입력
4. 작업제출 : submit을 클릭하여 시뮬레이션 시작

![Aspect ratio](/media/POST/000029/7.jpg)

![Aspect ratio](/media/POST/000029/8.jpg)

![Aspect ratio](/media/POST/000029/9.jpg)

![Aspect ratio](/media/POST/000029/10.jpg)

![Aspect ratio](/media/POST/000029/11.jpg)


###시뮬레이션 입력 변수/input deck (포트명 –i)
(주의!) 2018년 04월 초에 있던 EDISON 홈페이지 개편 이후로 시뮬레이션에 나타나는 변수명이 사용자용 변수명에서 프로그램 내부 변수명이 나타나도록 변경되었습니다. 아래에 매뉴얼상 변 수명과 현재 시뮬레이션의 프로그램 변수명의 대응표를 보고 참고하십시오. 변수명 표시는 추후 에 복구될 가능성도 있습니다. (2018/04/18 기준 작성)

![Aspect ratio](/media/POST/000029/1.jpg)

1. launching location latitude : 발사 위치의 위도 

2. launching location longitude : 발사 위치의 경도
- 단위 :  ̊ (도, degrees)
- 분, 초 단위는 60진법을 10진법으로 바꾸기
- 부호는 위도는 북위가 (+), 남위가 (-). 경도는 동경이 (+), 서경이 (-)

ex) 북위 37 ̊28’01” → 37 + 28*(1/60) + 1*(1/3600) = 37.466944 기본 설정값은 나로 우주센터의 위치

3. mass of 1st stage rocket without fuel : 1단계 로켓에서 연료를 뺀 질량 (M1) 

4. mass of 2nd stage rocket without fuel : 2단계 로켓에서 연료를 뺀 질량 (M2) 

5. mass of fuel of 1st stage rocket : 1단계 로켓의 초기 연료 질량 (Mf1)

6. mass of fuel of 2nd stage rocket : 2단계 로켓의 초기 연료 질량 (Mf2)
- 단위: kg
- 발사 시작 순간 질량은 M = M1 + M2 + Mf1 + Mf2 
- 최종 질량은 M2

7. fuel consumption rate1 : 1단계 로켓의 연료 소모 비율 (dM_1/dt) 

8. fuel consumption rate2 : 2단계 로켓의 연료 소모 비율 (dM_2/dt)
- 단위 : kg/s

9. relative speed of exhaust1 : 1단계 로켓에서 연소되어 분사된 연료의 상대속도 크기 (V1) 

10. relative speed of exhaust2 : 2단계 로켓에서 연소되어 분사된 연료의 상대속도 크기(V2)
- 단위 : m/s

* 원하는 추력(thrust)을 얻기 위한 V(분사된 연료의 상대속도) 정하기

- 추력의 단위가 힘(N)일 때 : 
$$
\left|F_{\text {thrust}}\right|=\left|V \frac{d M}{d t}\right| \rightarrow|V|=\left|F_{\text {thrust}} / \frac{d M}{d t}\right|
$$

- 추력의 단위가 질량(kg)일 때(공학적 표기) : 
$$
|V|=\left|g * F_{\text {thrust}} / \frac{d M}{d t}\right|
$$
(g: 중력가속도)

- 비추력이 주어졌을 때 (단위 초(s)) : 
$$
|V|=I_{s p} * g
$$
($I_{s p}$ : 비추력)

11. 2nd stage rocket starting time : 2단계 로켓이 연소를 시작하는 시간

단위 : s

12. simulation ending time : 시뮬레이션 상에서 계산되는 최대 시간

13. gif file out : 시뮬레이션 결과에 대한 gif 애니매이션 이미지 생성 여부. 시뮬레이션 결과를 이해하기에 좋지만 시간이 오래 걸림

###시뮬레이션 입력 변수 / 연소 분사 방향 설정 (포트명 –c)
로켓과 지구 중심을 이은 선에 대해서 로켓의 연소된 연료가 분사되는 방향의 각도. 발사 순간에는 0도이며, 지표면으로부터 특정 높이에 도달한 순간부터 그 이후의 분사 각도를 정할 수 있다. 2단 로켓 점화 이후의 각도도 설정해야한다. 이 각도는 추력(thrust) 의 방향을 결정한다. 최종적인 가속도는 여기에 중력과 공기저항력을 합성한 방향으로 생긴다. 설정한 높이에 도달하여도 로켓이 연소 중인 시간을 벗어나면 궤도에 아무런 영향도 줄 수 없다.
 
![Aspect ratio](/media/POST/000029/2.jpg)

아래와 같은 설정에서 다음과 같이 입력

![Aspect ratio](/media/POST/000029/3.jpg)

![Aspect ratio](/media/POST/000029/4.jpg)

예시)
![Aspect ratio](/media/POST/000029/5.jpg)

###결과물 예시
![Aspect ratio](/media/POST/000029/6.jpg)

###분석참고정보
####시뮬레이션에 입력된 물리량

- 지구 질량 M_e = 5.97237 * 10^24 kg
- 지구 반지름 R_e = 6.3781 * 10^6 m 
- 중력상수 G = 6.674 * 10^(-11) N*m^2/kg^2

###매뉴얼수정기록
- 2017/04/17 Ver1.0.0 매뉴얼 최초 작성
- 2018/04/18 EDISON 홈페이지 개편에 따른 매뉴얼 수정