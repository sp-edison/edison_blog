---
title: "CSD_EPLAST"
date: "2019-08-06T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/CSD_EPLAST"
category: "Computational Structural Dynamics"
tags: 
 - "Multiplicative decomposition"
 - "Hyperelastic response"
description: "Elastic-Plastic Analysis SW Manual"
---
##탄소성 구조해석 프로그램 매뉴얼
##Elastic-Plastic Analysis SW

##Contents
- 프로그램 개요 및 특징
- Multiplicative decomposition 기반의 소성이론
- Input 파일 작성 및 프로그램 실행
- 예제

##프로그램 개요 및 특징
###프로그램 개요
###탄소성 구조해석 프로그램
- 미소변형 및 대변형 탄성 해석 프로그램
- 미소변형 및 대변형 소성 해석 프로그램
- Multiplicative decomposition 기반의 프로그램
- Hyperelastic response 기반의 프로그램

###프로그램 특징
- Solver로 intel의 pardiso solver 사용
- 프로그램의 input 파일 형식은 ABAQUS input 파일 형식과 유사
- 해석 결과는 텍스트 파일의 <input_name>.out과 Tecplot 파일의 <input_name>.plt 생성 
- 2차원(평면응력/평면변형률)과 3차원(continuum) 요소 사용
- 사면체와 육면체 1차 및 2차 요소의 적용 가능

##Multiplicative decomposition 기반의 소성이론
###Theory of Plasticity
####Multiplicative decomposition of the deformation gradient
$$
\boldsymbol{F}=\boldsymbol{F}^{e} \boldsymbol{F}^{p}
$$

Stress-free configuration : $\boldsymbol{F}^{e^{-1}}$

####Cauchy tensors
Right Cauchy tensor :
$$
\boldsymbol{C}^{p}=\boldsymbol{F}^{p^{T}} \boldsymbol{F}^{p}
$$

Left Cauchy tensor :
$$
\boldsymbol{b}^{e}=\boldsymbol{F}^{e} \boldsymbol{F}^{e^{T}}=\boldsymbol{F} \boldsymbol{C}^{p^{-1}} \boldsymbol{F}^{T}
$$

Lie derivative of $\boldsymbol{b}^{e}$ :
$$
\mathcal{L}_{y} \boldsymbol{b}^{e}=\boldsymbol{F} \dot{\boldsymbol{C}}^{p^{-1}} \boldsymbol{F}^{T}
$$

Let $\overline{\boldsymbol{F}}$ denote the volume-preserving part of the deformation gradient

$$
\overline{\boldsymbol{F}}=J^{-1 / 3} \boldsymbol{F} \quad \text { where } \quad \operatorname{det}(\boldsymbol{F})=J \quad \operatorname{det}(\overline{\boldsymbol{F}})=1
$$

$$
\overline{\boldsymbol{b}}^{e}=J^{-2 / 3} \boldsymbol{b}^{e}
$$

####Associated flow rule
: the principle of maximum plastic dissipation

$$
\mathcal{L}_{v} \boldsymbol{b}^{e}=-\frac{2}{3} \dot{\gamma} \operatorname{tr}\left(\boldsymbol{b}^{e}\right) \boldsymbol{n} \quad \text { or } \quad \frac{\partial}{\partial t} \overline{\boldsymbol{C}}^{p^{-1}}=-\frac{2}{3} \dot{\gamma} \operatorname{tr}\left(\boldsymbol{b}^{e}\right) \boldsymbol{F}^{-1} \boldsymbol{n} \boldsymbol{F}^{-T}
$$

$$
\text { where } \quad \boldsymbol{n}=\boldsymbol{s} /\|\boldsymbol{s}\| \quad \boldsymbol{s}=\operatorname{dev}(J \boldsymbol{\sigma})=\operatorname{dev}(\boldsymbol{\tau}) \quad \dot{\overline{\boldsymbol{\varepsilon}}}^{p}=\sqrt{\frac{2}{3}} \dot{\gamma}
$$

$$
\operatorname{tr}\left(\boldsymbol{b}^{e}\right)=\boldsymbol{I} : \boldsymbol{F} \boldsymbol{C}^{p^{-1}} \boldsymbol{F}^{T}=\boldsymbol{C}^{p^{-1}} : \boldsymbol{C}
$$

####Yield Condition
$$
f\left(\tau, \overline{\varepsilon}^{p}\right)=\|\operatorname{dev} \tau\|-\sqrt{\frac{2}{3}}\left(\sigma_{y}+h \overline{\varepsilon}^{p}\right)
$$

$$
\text { where } \quad \dot{\gamma} \geq 0 \quad f \leq 0 \quad \dot{\gamma} f=0
$$

####Hyperelastic response

Stored-energy function : $W=U(J)+\overline{W}\left(\overline{\boldsymbol{b}}^{e}\right)$

$$
\text { where } U(J)=\frac{1}{2} \boldsymbol{\kappa}\left[\frac{1}{2}\left(J^{2}-1\right)-\ln J\right] \quad \overline{W}\left(\overline{\boldsymbol{b}}^{e}\right)=\frac{1}{2} \mu\left(t r \overline{\boldsymbol{b}}^{e}-3\right)
$$

$$
t r \overline{b}^{e}=t r \overline{\boldsymbol{C}}^{e}=\operatorname{tr}\left(\overline{\boldsymbol{C}} \overline{\boldsymbol{C}}^{p^{-1}}\right)
$$

![Aspect ratio](/media/POST/000022/0.jpg)

###Computational Plasticity
####Update state variables
![Aspect ratio](/media/POST/000022/1.jpg)

Returen mapping algorithm based on backward Euler method
![Aspect ratio](/media/POST/000022/2.jpg)


- Update Stress
![Aspect ratio](/media/POST/000022/3.jpg)
![Aspect ratio](/media/POST/000022/4.jpg)
![Aspect ratio](/media/POST/000022/5.jpg)

####Consistent tangent modulus with the int{egration scheme
![Aspect ratio](/media/POST/000022/6.jpg)
![Aspect ratio](/media/POST/000022/7.jpg)

##Input 파일 작성 및 프로그램 실행
###Input 파일 작성
![Aspect ratio](/media/POST/000022/8.jpg)

- 키워드는 항상 *로 시작하고, 다음 줄에 나타내는 데이터가 무엇인지를 나타냄
- 키워드 입력 시 대문자 또는 소문자 한 가지로 통일하여 입력 (혼용 시 에러 발생) 
- 키워드와 키워드 사이는 띄지 않고 연속적으로 입력
- 주석 및 코멘트 처리는 ** (* 두 번 이상) 입력 후 표시

![Aspect ratio](/media/POST/000022/9.jpg)

- 사용 가능한 요소 타입
![Aspect ratio](/media/POST/000022/10.jpg)


![Aspect ratio](/media/POST/000022/11.jpg)

- 첫 번째 키워드에서의 재료 이름과 두 번째 키워드에서의 재료 이름은 반드시 일치해야 함
- 현재 프로그램에서는 탄소성 소변형 및 대변형 해석이 가능
- SET은 절점기반의 NSET과 요소기반의 ELSET을 만들 수 있고, 설정 해주지 않거나 여러 개 생성해도 무관

![Aspect ratio](/media/POST/000022/12.jpg)

####집중하중 부여 방법
![Aspect ratio](/media/POST/000022/13.jpg)

####분포하중 부여 방법
![Aspect ratio](/media/POST/000022/14.jpg)

하중의 부여는 집중하중의 경우 CLOAD, 분포하중의 경우 DLOAD를 이용

###Edison 프로그램 실행

##프로그램 해석 결과 확인
###<input_name>.out 파일 확인
- ![Aspect ratio](/media/POST/000022/15.jpg)

해석에 사용된 데이터 (절점 번호 및 좌표, 재료 정보)

- ![Aspect ratio](/media/POST/000022/16.jpg)

요소 정보 및 적용된 물성 정보 

경계조건 (구속 및 하중) 정보

- ![Aspect ratio](/media/POST/000022/17.jpg)

해석 후의 결과 값으로, 각 절점에 대한 변위 값

- ![Aspect ratio](/media/POST/000022/18.jpg)

해석 후의 결과 값으로, 각 요소의 적분점에서의 응력 값

S11, S22, S33, S12, S23, S31 값 도출

Reduced 또는 Full integration이 적용된 경우에 따라 각 적분점에서의 응력 값 도출

- ![Aspect ratio](/media/POST/000022/19.jpg)

경계조건이 부여된 절점에서의 반력 값 

구속된 자유도에 따라 각각의 값 도출

##프로그램 해석 결과 확인
![Aspect ratio](/media/POST/000022/20.jpg)

![Aspect ratio](/media/POST/000022/21.jpg)

![Aspect ratio](/media/POST/000022/22.jpg)

![Aspect ratio](/media/POST/000022/23.jpg)

###가시화 프로그램 설치
![Aspect ratio](/media/POST/000022/24.jpg)

![Aspect ratio](/media/POST/000022/25.jpg)

###가시화 프로그램 실행
- 설치 완료 후 바탕화면에 생성된 ‘CSDPost.exe’ 아이콘 더블 클릭 
- 시작 → 모든 프로그램 → CSD → CSDPost 실행

![Aspect ratio](/media/POST/000022/26.jpg)

##프로그램 해석 결과 확인
###프로그램 조작 및 설정 방법
####마우스 조작 방법
- 이동 : Ctrl + 마우스 오른쪽 버튼 
- 회전 : Ctrl + 마우스 왼쪽 버튼
- 확대 및 축소 : 마우스 휠 이동

####배경 설정 방법
- File → Option → Preference → Background → 배경색 설정

####Contour 및 Scale factor 설정 방법
- File → Option → Result → Set Contour → Contour 설정 
- File → Option → Result → Set Deform → Deform 설정

![Aspect ratio](/media/POST/000022/27.jpg)

###해석 결과 가시화
![Aspect ratio](/media/POST/000022/28.jpg)

- 메뉴 → File 클릭 → New 클릭
- OK 버튼을 클릭하여 새로운 프로젝트 생성(프로젝트 이름 및 경로는 임의로 지정 가능)

![Aspect ratio](/media/POST/000022/29.jpg)

- 메뉴 → Import 클릭 → Seoul Tech Output 클릭
- EDISON 프로그램으로 해석 완료 후 다운받은 결과 파일 (_s.plt 파일) 열기

###해석 결과 가시화 – 변위 분포
![Aspect ratio](/media/POST/000022/30.jpg)

- 화면 왼쪽 하단의 Result 탭 클릭
- 왼쪽 트리에서 CASE2 → Displacement → 원하는 좌표계 선택

###해석 결과 가시화 – 응력 분포
![Aspect ratio](/media/POST/000022/31.jpg)

- 왼쪽 트리에서 CASE2 → Stress → 응력 분포 선택
- 화면 오른쪽의 Smooth 설정을 AVERAGE로 바꿈 → Load 클릭










