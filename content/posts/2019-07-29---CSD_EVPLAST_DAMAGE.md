---
title: "CSD_EVPLAST_DAMAGE"
date: "2019-07-29T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/CSD_EVPLAST_DAMAGE"
category: "Computational Structural Dynamics"
tags: 
 - "Plastic Deformation"
 - "Damage Mechanism Analysis"
 - "Additive decomposition"
description: "CSD_EVPLAST_DAMAGE Manual"
---
##재료 손상을 동반한 소성 해석 프로그램 매뉴얼 Plastic Deformation with Damage Mechanism Analysis SW (Gudrson Damage Model)
###Contents
- 프로그램 개요 및 특징
- Additive decomposition 기반의 소성이론 
- 연성 재료에서의 손상 메커니즘
- Input 파일 작성 및 프로그램 실행 
- 프로그램 해석 결과 확인

##프로그램 개요 및 특징
###프로그램 개요
####재료 손상을 동반한 소성 해석 프로그램
- 미소변형 및 대변형 소성 해석 프로그램
- 재료 손상 메커니즘 해석 프로그램
- Additive decomposition 기반의 프로그램 
- Gurson damage model 기반의 프로그램

###프로그램 특징
- Solver로 intel의 pardiso solver 사용
- 프로그램의 input 파일 형식은 ABAQUS input 파일 형식과 유사
- 해석 결과는 텍스트 파일의 <input_name>.out과 Tecplot 파일의 <input_name>.plt 생성 
- 2차원(평면응력/평면변형률)과 3차원(continuum) 요소 사용
- 사면체와 육면체 1차 및 2차 요소의 적용 가능

##Additive decomposition 기반의 소성이론
###Theory of Plasticity
####Basic Assumptions
- Additive decomposition: $\dot{\varepsilon}=\dot{\varepsilon}^{e}+\dot{\varepsilon}^{p}$

- Stress-strain relation (objective stress rate, hypoelastic response): 

$$
\dot{\sigma}^{J}=\boldsymbol{c} : \dot{\mathcal{\varepsilon}}^{e}
$$

- Consistency condition: stress remains on the yield surface during plastic deformation

####Isotropic yield surface
![Aspect ratio](/media/POST/00008/0.jpg)


$$
f\left(\sigma^{\prime}, \alpha, \overline{\varepsilon}^{p}\right)=\frac{1}{2}\left(\sigma_{i j}^{\prime}-\alpha_{i j}\right)\left(\sigma_{i j}^{\prime}-\alpha_{i j}\right)-\frac{1}{3} \sigma_{y}^{2}\left(\overline{\varepsilon}^{p}\right)=0
$$

$$
\text { where } \quad \sigma^{\prime}=\sigma-\frac{1}{3} \operatorname{tr}(\sigma) \boldsymbol{I}
$$

$$
\dot{\overline{\varepsilon}}^{p}=\sqrt{\frac{2}{3} \dot{\boldsymbol{\varepsilon}}^{p} : \dot{\boldsymbol{\varepsilon}}^{p}}
$$

####Associated flow rule
: the principle of maximum plastic dissipation
$$
\dot{\varepsilon}^{p}=\dot{\lambda} \frac{\partial f}{\partial \sigma^{\prime}}
$$

####Consistency condition
$$
\dot{f}=\frac{\partial f}{\partial \sigma^{\prime}} : \dot{\sigma}^{o}+\frac{\partial f}{\partial \alpha} : \dot{\alpha}+\frac{\partial f}{\partial \overline{\varepsilon}^{p}} \dot{\vec{\varepsilon}}^{p}=M_{i j} \dot{\sigma}_{i j}^{0}-M_{i j} \dot{\alpha}_{i j}-\frac{2}{3} \sigma_{y} h \dot{\overline{\varepsilon}}^{p}=0
$$

$$
\text { where } \quad M_{i j}=\frac{\partial f}{\partial \sigma_{i j}^{\prime}}=\sigma_{i j}^{\prime}-\alpha_{i j} \quad h=\frac{d \sigma_{y}}{d \overline{\varepsilon}^{p}}
$$

$$
\text { By using } \quad \dot{\alpha}_{i j}=c \dot{\varepsilon}_{i j}^{p}=c \lambda M_{i j} \quad \dot{\overline{\varepsilon}}^{p}=\lambda \sqrt{\frac{2}{3} M_{i j} M_{i j}}
$$

$$
\dot{\lambda}=\frac{M_{k l} \dot{\sigma}_{k l}^{O}}{c M_{p q} M_{p q}+\frac{2}{3} h \sigma_{y} \sqrt{\frac{2}{3} M_{m n} M_{m n}}}
$$

##연성 재료에서의 손상 메커니즘
###Theory of Damage mechanism 
####Yield function (Gurson model)
$$
\phi=\sigma_{e q}^{2}-\left[1-2 q_{1} f^{*} \cosh \left(\frac{3 \sigma_{h}}{2 \sigma_{y}} q_{2}\right)+q_{3}\left(f^{*}\right)^{2}\right] \sigma_{y}^{2}
$$

![Aspect ratio](/media/POST/00008/1.jpg)

####Modified void volume fraction
$$
f^{*}=\left\{\begin{array}{ll}{f} & {f \leq f_{c}} \\ {f_{c}+\eta\left(f-f_{c}\right)} & {f>f_{c}}\end{array}\right.
$$

####Equivalent plastic strain in the matrix
$$
\dot{\overline{\varepsilon}}_{m}^{p}=\frac{\sigma_{e q} \dot{\overline{\varepsilon}}^{p}}{(1-f) \sigma_{0}}
$$

####Evolution of void volume fraction
$$
\dot{f}=(1-f) \dot{\mathcal{E}}_{i j}^{p} \delta_{i j}+A \dot{\overline{\varepsilon}}_{m}^{p}
$$

$$
A=\frac{f_{n}}{s_{n} \sqrt{2 \pi}} \exp \left[-\frac{1}{2}\left(\frac{\overline{\varepsilon}_{m}^{p}-\varepsilon_{n}}{s_{n}}\right)^{2}\right]
$$

![Aspect ratio](/media/POST/00008/2.jpg)

###Computational Formulations
####Update state variables
![Aspect ratio](/media/POST/00008/3.jpg)

####Consistent tangent modulus
![Aspect ratio](/media/POST/00008/4.jpg)

###Gurson damage parameters (AL-5052 H32) 
####Tensile test
![Aspect ratio](/media/POST/00008/5.jpg)

####FE analysis
![Aspect ratio](/media/POST/00008/6.jpg)

##Input 파일 작성 및 프로그램 실행
###Input 파일 작성
![Aspect ratio](/media/POST/00008/7.jpg)

- 키워드는 항상 *로 시작하고, 다음 줄에 나타내는 데이터가 무엇인지를 나타냄
- 키워드 입력 시 대문자 또는 소문자 한 가지로 통일하여 입력 (혼용 시 에러 발생) 
- 키워드와 키워드 사이는 띄지 않고 연속적으로 입력
- 주석 및 코멘트 처리는 ** (* 두 번 이상) 입력 후 표시

![Aspect ratio](/media/POST/00008/8.jpg)
   
####사용 가능한 요소 타입
![Aspect ratio](/media/POST/00008/9.jpg)

![Aspect ratio](/media/POST/00008/10.jpg)

![Aspect ratio](/media/POST/00008/11.jpg)

![Aspect ratio](/media/POST/00008/12.jpg)
- SET에서 절점 또는 요소 번호의 마지막에 , 가 있는지 반드시 확인
- 절점기반의 NSET과 요소기반의 ELSET이 있고, 설정 해주지 않거나 여러 개 생성해도 무관 
- MPC의 기준 절점에 변위 경계조건 부여 가능

![Aspect ratio](/media/POST/00008/13.jpg)

####집중하중 부여 방법
![Aspect ratio](/media/POST/00008/14.jpg)

####분포하중 부여 방법
![Aspect ratio](/media/POST/00008/15.jpg)

##Input 파일 작성 및 프로그램 실행
###EDISON 프로그램 실행
![Aspect ratio](/media/POST/00008/16.jpg)

업로드 할 경우 ‘Menu’ 클릭 → ‘Upload’ 클릭(해석 input 파일(~.inp 파일) 업로드)
  

![Aspect ratio](/media/POST/00008/17.jpg)

![Aspect ratio](/media/POST/00008/18.jpg)

![Aspect ratio](/media/POST/00008/19.jpg)
- .msg : 해석 중 발생하는 에러 내용 표시
- .out : 해석 정보 및 결과 파일 (text 형식)
- .plt : 해석 결과 파일 (요소간 불연속적인 응력 contour) 
- _s.plt : 해석 결과 파일 (요소간 연속적인 응력 contour)

##프로그램 해석 결과 확인
###가시화 프로그램 설치
![Aspect ratio](/media/POST/00008/20.jpg)
![Aspect ratio](/media/POST/00008/21.jpg)
![Aspect ratio](/media/POST/00008/22.jpg)

###가시화 프로그램 실행
- 설치 완료 후 바탕화면에 생성된 ‘CSDPost.exe’ 아이콘 더블 클릭 
- 시작 → 모든 프로그램 → CSD → CSDPost 실행

   
##프로그램 해석 결과 확인
###프로그램 조작 및 설정 방법 
####마우스 조작 방법
- 이동 : Ctrl + 마우스 오른쪽 버튼 
- 회전 : Ctrl + 마우스 왼쪽 버튼
- 확대 및 축소 : 마우스 휠 이동

####배경 설정 방법
File → Option → Preference → Background → 배경색 설정

####Contour 및 Scale factor 설정 방법
- File → Option → Result → Set Contour → Contour 설정 
- File → Option → Result → Set Deform → Deform 설정
![Aspect ratio](/media/POST/00008/23.jpg)

    
###해석 결과 가시화
![Aspect ratio](/media/POST/00008/24.jpg)

- 메뉴 → File 클릭 → New 클릭
- OK 버튼을 클릭하여 새로운 프로젝트 생성(프로젝트 이름 및 경로는 임의로 지정 가능)
    
![Aspect ratio](/media/POST/00008/25.jpg)
   
- 메뉴 → Import 클릭 → Seoul Tech Output 클릭
- EDISON 프로그램으로 해석 완료 후 다운받은 결과 파일 (_s.plt 파일) 열기

   
###해석 결과 가시화 – 변위 분포
![Aspect ratio](/media/POST/00008/26.jpg)

- 화면 왼쪽 하단의 Result 탭 클릭
- 왼쪽 트리에서 결과 확인 하고자 하는 CASE 선택 → Displacement → 좌표계 선택

###해석 결과 가시화 – 응력 분포
![Aspect ratio](/media/POST/00008/27.jpg)

- 결과 확인 하고자 하는 CASE 선택 → Stress → 응력 분포 선택
- 화면 오른쪽 상단의 Smooth 설정을 AVERAGE로 선택 → 하단의 Load 클릭

   