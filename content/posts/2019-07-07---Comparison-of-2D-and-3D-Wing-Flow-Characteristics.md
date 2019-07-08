---
title: "Comparison of 2D and 3D Wing Flow Characteristics"
date: "2019-07-07T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Comparison-of-2D-and-3D-Wing-Flow-Characteristics"
category: "Computational Fluid Dynamics"
tags: 
 - "2D airfoile"
 - "3D_Incomp"
description: "EDISON 사이언스 앱을 활용해 2차원 익형과 3차원 날개 유동 특성 비교"
---

 ## 2차원 익형, 3차원 날개 비교
  - 2차원익형
    - 2차원 날개는 일정한 익형이 무한한 span을 가진다고 가정. 
    - 전체적인 날개 대신 익형 주위 유동을 해석.
  - 3차원날개
    - 3차원 날개는 유한한 span을 가짐.
    - 날개의 끝에서 생기는 wing-tip vortex로 인하여 2차원과 다른 유동 특성을 가짐.
    - Aspect ratio(가로 세로 비)가 작을수록 3차원 날개 특성을 가짐.
![Aspect ratio](/media/POST/Comparison-of-2D-and-3D-Wing-Flow-Characteristics/0.jpg)


Aspect Ratio

$$
A R=\frac{b^{2}}{A} \approx \frac{b}{c}
$$


AR : Aspect Ratio A : 날개 넓이
b : Span 길이
c : chord 길이

## 3차원 날개에서 Downwash 발생 원리

-  Wing-tip vortex
  - 날개의 아랫면의 높은 압력과 윗면의 낮은 압력이 형성되어 날개 끝에서 아래에서 위로 흐름이 발생.
  - 이러한 흐름으로 인하여 윗면에서는 날개 뿌리, 아랫면에서는 날개 끝 방향으로 흐름이 생겨 하류에서 wing-tip vortex 형성.
- Downwash
  - 하류로 이어진 wing-tip vortex (trailing vortex)와 날개에 존재하는 bound vortex로 인해 날개 뒤쪽에서 downwash(아래 방향 흐름 성분)이 생김.
  - Aspect ratio가 커질수록 downwash가 커짐.

![Aspect ratio](/media/POST/Comparison-of-2D-and-3D-Wing-Flow-Characteristics/03.jpg)


## Local relative wind, Induced AOA(Angle of Attack)

- Local relative wind
  -  Downwash와 freestream이 합쳐져 날개 앞쪽 유동이 아랫방향으로 기울어져 local relative wind 형성.
- Induced AOA (Angle of Attack)
  - Local relative wind에 의해 감소하는 AOA $\left(\alpha_{i}\right)$.
  - Local relative wind에 의한 effective AOA : $\alpha_{e f f}=\alpha-\alpha_{i}$


![Aspect ratio](/media/POST/Comparison-of-2D-and-3D-Wing-Flow-Characteristics/3.jpg)



 
## 3차원 날개의 양력 ,항력 계수

-  Induced drag (유도 항력)
  -  3차원 날개에서 양력 발생시 downwash에 의해 형성된 local relative wind에 수직한 방향으로 양력 발생.
  - 이로 인하여 freestream 방향으로 양력 성분이 존재  항력으로 작용 (Induced drag)
- 양력, 항력 계수
  - 2차원과 비교했을 때, downwash에 의해 유도 항력이 발생하여 항력이 증가하고, 이론적으로 양력이 감소.
  - Aspect ratio가 작을수록 3차원 날개의 특징, downwash가 강해져 항력이 증가하고 양력이 감소.

$$
\begin{array}{l}{\text { Lift coefficient }} \\ {\qquad C_{l, 3 d}=\frac{C_{l, 2 d}}{1+\frac{C_{l, 2 d}}{\pi A R}}}\end{array}
$$

$$
\begin{array}{l}{\text { Drag coefficient }} \\ {\qquad C_{d, 3 d}=C_{d, 2 d}+\frac{C_{l, 3 d}^{2}}{0.7 \pi A R}}\end{array}
$$

$C_{l, 3 d}$ : 3차원 lift coefficient
$C_{d, 3 d}$ : 3차원 drag coefficient
$C_{l, 2 d}$ : 2차원 lift coefficient
$C_{d, 2 d}$ : 2차원 drag coefficient
$AR$ : Aspect ratio



## Simulation Result
- 3차원 NACA0012 날개에서의 wingtip vortex
  - 3D_Incomp 해석자 활용 (Laminar flow)
  - Reynolds number: 2000
  - AOA: 2.0 degree

![Aspect ratio](/media/POST/Comparison-of-2D-and-3D-Wing-Flow-Characteristics/06.jpg)





