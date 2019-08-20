---
title: "Brownian_dynamics_simulation_of_polymers"
date: "2019-08-20T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Brownian_dynamics_simulation_of_polymers"
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
![Aspect ratio](/media/POST/000053/0.jpg)

전체 우주(universe) 중 관측자가 관심있는 대상을 계(system)이라 하며, 그 주변을 환경(surroundings)라 한다.

계와 환경 사이에 에너지(energy)의 출입 그리고 물질(material)의 출입에 따라 크게 세가지 종류의 계가 존재한다.

###내부 에너지(internal energy)
####계 내부에 존재하는 입자 사이의 작용하는 에너지 및 입자가 가지고 있는 운동 에너지의 합
$$
E=E_{k}+E_{p}
$$

- 입자의 운동 에너지(kinetic energy)
$$
E_{k}=\frac{1}{2}mv^2, m: 질량(mass), v: 속도(velocity)
$$

- 입자의 위치 에너지(potential energy)
$$
E_{p}=E_{bond}+E_{angle}+E_{dihedral}+E_{improper}+E_{elctronic transition}+E_{Coulomb}+E_{vdW}
$$

- 내부 에너지(internal energy)
![Aspect ratio](/media/POST/000053/1.jpg)
![Aspect ratio](/media/POST/000053/2.jpg)
![Aspect ratio](/media/POST/000053/3.jpg)
![Aspect ratio](/media/POST/000053/4.jpg)
![Aspect ratio](/media/POST/000053/5.jpg)
![Aspect ratio](/media/POST/000053/6.jpg)

![Aspect ratio](/media/POST/000053/7.jpg)

- 세기 성질과 크기 성질 사이의 관계
![Aspect ratio](/media/POST/000053/8.jpg)

####앙상블(ensemble)
통계역학에서, 어떤 계와 동등한 계의 모음을 의미한다.

열역학적 물리량이 동일한 계들의 집합으로 생각할 수 있다. (e.g. 분자 수(N), 부피(V), 압력(P), 온도(T), 내부 에너지(E), 엔탈피(H), 화학 포텐셜(𝜇) 등)
![Aspect ratio](/media/POST/000053/9.jpg)

- 미소 정준 앙상블(microcanonical ensemble) 
N,V,E가 같은 계들의 집합

- 등엔탈피-등압 앙상블(isoenthalpic-isobaric ensemble) 
N,P,H가 같은 계들의 집합

- 정준 앙상블(canonical ensemble) 
N,V,T가 같은 계들의 집합

- 등온-등압 앙상블(isothermal-isobaric ensemble) 
N,P,T가 같은 계들의 집합

- 대정준 앙상블(grand canonical ensemble) 
𝜇 ,V,T가 같은 계들의 집합

####앙상블(ensemble)의 의미
![Aspect ratio](/media/POST/000053/10.jpg)

![Aspect ratio](/media/POST/000053/11.jpg)

![Aspect ratio](/media/POST/000053/12.jpg)

![Aspect ratio](/media/POST/000053/13.jpg)

####브라운 운동(Brownian motion)
미소 입자들이 불규칙하게 운동하는 현상
![Aspect ratio](/media/POST/000053/14.jpg)

Stokes 및 Einstein에 의하여 입자가 움직이는 “평균거리”가 계산되었음

$$
R_{rms}=\sqrt{<R^2>}=6D\nabla t \text{where} D=\frac{k_BT}{6\pi \eta s}
$$

$$
R:\text{변위}, D:\text{확산계수}, \nabla r:\text{이동시간}, \\
k_{B}:\text{볼츠만 상수}, T:\text{온도}, \eta:\text{점성}, s:\text{입자의 반지름}
$$

 Note: Albert Einstein이 1905년도에 발표한 논문 중 하나로써 그 해 발표한 대표적인 논문은 “특수상대성 이론”, “광양자 가설”, “브라운 운동”이 있다.

####브라운 역학(Brownian dynamics)
계 내부에 용매(solvent)를 제거하는 대신 용질(solute)의 용매로 인한 움직임을 브라운 운동으로 표현한 역학

- 브라운 운동을 더한 입자의 위치
![Aspect ratio](/media/POST/000053/15.jpg)

####고분자의 두가지 물리량
- End-to-end distance
$$
R_{end}=\sum_{i}r_{i}
$$
![Aspect ratio](/media/POST/000053/16.jpg)

고분자 내 단량체 양끝 사이의 거리를 나타낸다.

- Radius of gyration
$$
R_g=\sqrt{\frac{1}{N}\sum_{i}(r_{i}-r_{mean})^2}
$$
![Aspect ratio](/media/POST/000053/17.jpg)

고분자의 중심으로부터 단량체들이 얼마나 퍼져있는지 정도를 나타냄 (단량체의 분산정도)

- 위치에너지
![Aspect ratio](/media/POST/000053/18.jpg)

##EDISON program

pre_nstep: pre-equilibration steps

####pre_nstep 값이 0이 아닐 시,
- pre_temp:temperature for pre-eq.
- pre_diff:diffusion coeff. for pre-eq.
- pre-eps:LJ-potential depth for pre-eq.(e/k_B)

- nstep: 총 timestep
- isave: 해당 step수 마다 결과값 도출
- temperature: system의 온도
- diffusion: 확산 계수
- rcut: LJ potential의 cutoff radius
- epsilon: LJ potential의 potential depth(e/k_B)
- time: 매 step 사이의 시간간격

####결과파일 목록
- *.pos: 시뮬레이션 마지막 구조 파일
- *.xyz: coordinates file (isave간격으로 저장)
- *_end.txt: end-to-end distance 결과 파일 (root mean square value) 
- *_end_vector.txt: end-to-end distance vector형식 결과 파일 
- *_rg.txt: radius of gyration 결과 파일

*표시는 -out 입력포트에 따라 달라짐

##결과 및 분석
####End-to-end distance (OUTPUTNAME_end.txt 파일)
Time steps / End-to-end distance
![Aspect ratio](/media/POST/000053/19.jpg)
![Aspect ratio](/media/POST/000053/20.jpg)

Time steps / Radius of gyration
![Aspect ratio](/media/POST/000053/21.jpg)
![Aspect ratio](/media/POST/000053/22.jpg)

###Visualization program (VMD) 설치
1. google에서 VMD 검색 후 아래 website 접속
![Aspect ratio](/media/POST/000053/23.jpg)

2. “Software” -> ”VMD” -> ”Download” 클릭
![Aspect ratio](/media/POST/000053/24.jpg)

3. 아래 빨간색 상자 클릭 (OS가 윈도우인 경우)
![Aspect ratio](/media/POST/000053/25.jpg)

4. ID 및 Password 입력 후 “Contunue with ~” 클릭 (계정 생성)
![Aspect ratio](/media/POST/000053/26.jpg)

5. 공백을 채운다.
![Aspect ratio](/media/POST/000053/27.jpg)

6. ID 및 Password 입력 후 “Contunue with ~” 클릭 (계정 생성)
![Aspect ratio](/media/POST/000053/28.jpg)

7. 다운로드 확인
![Aspect ratio](/media/POST/000053/29.jpg)

8. 다운로드한 프로그램을 실행한 후 change버튼을 통해 경로를 설정
![Aspect ratio](/media/POST/000053/30.jpg)
