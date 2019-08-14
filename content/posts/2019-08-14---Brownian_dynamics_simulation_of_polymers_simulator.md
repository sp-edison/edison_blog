---
title: "Brownian dynamics simulation of polymers simulator"
date: "2019-08-14T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Brownian_dynamics_simulation_of_polymers_simulator"
category: "Computational Chemistry"
tags: 
 - "Brownian dynamics"
 - "Homopolymer"
 - "Coarse-grained model"
description: "고분자의 브라운 동력학 시뮬레이션 Manual"
---
##고분자의 브라운 동력학 시뮬레이션

##학습목표
1. 계(system)에 관하여 알아보고 열역학적 변수들이 동일한 계들의 집합인 앙상블(ensemble)의 개념을 이해한다.
2. 브라운 운동(Brownian motion)을 통하여 이를 이용한 브라운 역학(Brownian dynamics)을 이해한다
3. 고분자가 가지는 두가지 물리량을 알아보고 앙상블 평균을 구해본다.

##이론
###계(system)와 환경(surroundings)
![Aspect ratio](/media/POST/000040/0.jpg)

전체 우주(universe) 중 관측자가 관심있는 대상을 계(system)이라 하며, 그 주변을 환경(surroundings)라 한다.

계와 환경 사이에 에너지(energy)의 출입 그리고 물질(material)의 출입에 따라 크게 세가지 종류의 계가 존재한다.

###내부 에너지(internal energy)
계 내부에 존재하는 입자 사이의 작용하는 에너지 및 입자가 가지고 있는 운동 에너지의 합

$$
E=E_{k}+E_{p}
$$

####입자의 운동 에너지(kinetic energy)

$$
E_{k}=\frac{1}{2}mv^2
$$

- m : 질량(mass)
- v : 속도(velocity)

####입자의 위치 에너지(potential energy)

$$
E_{p}=E_{bond}+E_{angle}+E_{dihedral}+E_{improper}+E_{electronic transition}+E_{Coulomb}+E_{vdW}
$$

![Aspect ratio](/media/POST/000040/1.jpg)

![Aspect ratio](/media/POST/000040/2.jpg)

![Aspect ratio](/media/POST/000040/3.jpg)

![Aspect ratio](/media/POST/000040/4.jpg)

![Aspect ratio](/media/POST/000040/5.jpg)

![Aspect ratio](/media/POST/000040/6.jpg)

![Aspect ratio](/media/POST/000040/7.jpg)

- 𝑇 = 온도(𝑡𝑒𝑚𝑝𝑒𝑟𝑎𝑡𝑢𝑟𝑒) 
- 𝑆 = 엔트로피(𝑒𝑛𝑡𝑟𝑜𝑝𝑦) 
- 𝑃 = 압력(𝑝𝑟𝑒𝑠𝑠𝑢𝑟𝑒)
- 𝑉 = 부피(𝑣𝑜𝑙𝑢𝑚𝑒)
- 𝜇 = 화학 포텐셜(𝑐h𝑒𝑚𝑖𝑐𝑎𝑙 𝑝𝑜𝑡𝑒𝑛𝑡𝑖𝑎𝑙) 
- 𝑛 = 몰 수(𝑚𝑜𝑙𝑒 𝑛𝑢𝑚𝑏𝑒𝑟)

####세기 성질과 크기 사이의 관계
![Aspect ratio](/media/POST/000040/8.jpg)

###앙상블
통계역학에서, 어떤 계와 동등한 계의 모음을 의미한다.

열역학적 물리량이 동일한 계들의 집합으로 생각할 수 있다. (e.g. 분자 수(N), 부피(V), 압력(P), 온도(T), 내부 에너지(E), 엔탈피(H), 화학 포텐셜(𝜇) 등)
![Aspect ratio](/media/POST/000040/9.jpg)

####미소 정준 앙상블(microcanonical ensemble) 
N,V,E가 같은 계들의 집합

####등엔탈피-등압 앙상블(isoenthalpic-isobaric ensemble) 
N,P,H가 같은 계들의 집합

####정준 앙상블(canonical ensemble) 
N,V,T가 같은 계들의 집합

####등온-등압 앙상블(isothermal-isobaric ensemble) 
N,P,T가 같은 계들의 집합

####대정준 앙상블(grand canonical ensemble) 
𝜇 ,V,T가 같은 계들의 집합

###앙상블(ensemble)의 의미
![Aspect ratio](/media/POST/000040/10.jpg)

![Aspect ratio](/media/POST/000040/11.jpg)

![Aspect ratio](/media/POST/000040/12.jpg)

![Aspect ratio](/media/POST/000040/13.jpg)

###브라운 운동(Brownian motion)
미소 입자들이 불규칙하게 운동하는 현상

![Aspect ratio](/media/POST/000040/14.jpg)

Stokes 및 Einstein에 의하여 입자가 움직이는 “평균거리”가 계산되었음

###브라운 역학(Brownian dynamics)
계 내부에 용매(solvent)를 제거하는 대신 용질(solute)의 용매로 인한 움직임을 브라운 운동으로 표현한 역학

####브라운 운동을 더한 입자의 위치
![Aspect ratio](/media/POST/000040/15.jpg)

###고분자의 두가지 물리량
####End-to-end distance
![Aspect ratio](/media/POST/000040/16.jpg)

고분자내 단량체 양끝 사이의 거리를 나타낸다.

####Radius of gyration
![Aspect ratio](/media/POST/000040/17.jpg)

고분자의 중심으로부터 단량체들이 얼마나 퍼져있는지 정도를 나타냄 (단량체의 분산정도)

##EDISON program
###위치 에너지
![Aspect ratio](/media/POST/000040/18.jpg)

- $k_{FENE}$ = 7.0$k_{B}T/\sigma^2$
- $k_{B}$ : 볼츠만 상수
- $\sigma$ : 단량체의 직경 
- $R_{\sigma}$ = 2 $\sigma$
- $r_{cut}$ = 2.5 $\sigma$

