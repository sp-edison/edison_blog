---
title: "CSD_HYPERELAST_Mooney_Rivlin"
date: "2019-07-29T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/CSD_HYPERELAST_Mooney_Rivlin"
category: "Computational Structural Dynamics"
tags: 
 - "PRAGMA"
 - "Hyperelastic Deformation"
description: "CSD_HYPERELAST_Mooney_Rivlin Manual"
---
##초탄성 재료 거동 해석 프로그램 매뉴얼/Hyperelastic Deformation Analysis SW (Mooney-Rivlin Model)

###Contents
1. 프로그램 개요 및 특징 
2. 초탄성(hyperelastic) 재료거동 이론 
3. 대변형 유한요소 수식화
4. Input 파일 작성 및 프로그램 실행 
5. 프로그램 해석 결과 확인

##프로그램 개요 및 특징
###프로그램 개요
####초탄성(hyperelastic) 재료 거동 해석 프로그램
- 변형률에너지 기반 재료 거동 해석 프로그램
- 대변형 구조해석 프로그램
- Consistent tangent modulus 기반의 프로그램

###프로그램 특징
- Solver로 intel의 pardiso solver 사용
- 프로그램의 input 파일 형식은 ABAQUS input 파일 형식과 유사
- 해석 결과는 텍스트 파일의 <input_name>.out과 Tecplot 파일의 <input_name>.plt 생성 
- 2차원(평면응력/평면변형률)과 3차원(continuum) 요소 사용
- 사면체와 육면체 1차 및 2차 요소의 적용 가능

##초탄성(hyperelastic) 재료거동 이론
###Theory of hyperelasticity (1) 
####Definitions
초탄성(hyperelastic) 재료는 단위 체적 당 변형률 에너지로 재료 거동을 기술 

Strain energy function : $\Psi(\boldsymbol{C})=U(J)+\overline{\Psi}\left(\overline{I}_{1}, \overline{I}_{2}\right)$

where $C=\boldsymbol{F}^{T} \boldsymbol{F}$

$\boldsymbol{F} :$ deformation gradient

$$
\overline{I}_{1}=J^{-2 / 3} I_{1} \quad \overline{I}_{2}=J^{-4 / 3} I_{2} \quad \overline{\boldsymbol{F}}=J^{-1 / 3} \boldsymbol{F}
$$
$$
I_{1}=t r C \quad I_{2}=\frac{1}{2}\left((t r C)^{2}-t r C^{2}\right) \quad J^{2}=\operatorname{det} C
$$

####Mooney-Rivlin형태
$$
\Psi(\boldsymbol{C})=\frac{\mu_{1}}{2}\left(\overline{I}_{1}-3\right)+\frac{\mu_{2}}{2}\left(\overline{I}_{2}-3\right)+\frac{\kappa}{2}(J-1)^{2}
$$

####Neo-Hookean형태
$$
\Psi(\boldsymbol{C})=C_{1}\left(\overline{I}_{1}-3\right)+\frac{1}{D}(J-1)^{2}
$$

####Ogden형태
$$
\Psi(\boldsymbol{C})=\sum_{i=1}^{N} \frac{2 \mu_{i}}{\alpha_{i}^{2}}\left(\lambda_{1}^{\alpha_{i}}+\lambda_{2}^{\alpha_{i}}+\lambda_{3}^{\alpha_{i}}-3\right)+\sum_{i=1}^{N} K_{i}(J-1)^{2 i}
$$

![Aspect ratio](/media/POST/00007/0.jpg)

####Hyperelastic response
- 2nd Piola-Kirchhoff stress :
$$
\begin{array}{l}{S=2 \frac{\partial \Psi}{\partial C}} \\ {S=2 \frac{\partial U}{\partial J} \frac{\partial J}{\partial C}+2 \frac{\partial \overline{W}}{\partial \overline{C}} \frac{\partial \overline{C}}{\partial C}}\end{array}
$$

- Incremental constitutive equation : 
$$
\Delta S=\boldsymbol{C} : \frac{1}{2} \Delta \boldsymbol{C}
$$

- Fourth-order elasticity tensor :
$$
\begin{aligned} \boldsymbol{C} &=4 \frac{\partial^{2} \Psi}{\partial \boldsymbol{C} \partial \boldsymbol{C}}=2 \frac{\partial}{\partial \boldsymbol{C}}\left\{J \frac{\partial U}{\partial J} \boldsymbol{C}^{-1}+2 J^{-2 / 3}\left[\frac{\partial \overline{W}}{\partial \overline{\boldsymbol{C}}}-\frac{1}{3}\left(\frac{\partial \overline{W}}{\partial \overline{\boldsymbol{C}}} \overline{\boldsymbol{C}}\right) \overline{\boldsymbol{C}^{-1}}\right]\right\} \\ &=J \frac{\partial U}{\partial J}\left(\boldsymbol{C}^{-1} \otimes \boldsymbol{C}^{-1}-2 \boldsymbol{I}_{C^{-}}\right)+J^{2} \frac{\partial^{2} U}{\partial J^{2}} \boldsymbol{C}^{-1} \otimes \boldsymbol{C}^{-1}+\cdots \end{aligned}
$$

   
###Formulation of Mooney-Rivlin material 
####Compressible Mooney-Rivlin model
- Strain energy function : $\Psi(\boldsymbol{C})=\frac{\mu_{1}}{2}\left(\overline{I}_{1}-3\right)+\frac{\mu_{2}}{2}\left(\overline{I}_{2}-3\right)+\frac{\kappa}{2}(J-1)^{2}$

$$
S=2 \frac{\partial \Psi}{\partial C}=J^{-2 / 3}\left(\mu_{1}+\mu_{2} \overline{I}_{1}\right) \boldsymbol{I}-J^{-2 / 3} \mu_{2} \boldsymbol{C}+\left[\kappa J(J-1)-\frac{1}{3} J^{-2 / 3}\left(\mu_{1} \overline{I}_{1}+2 \mu_{2} \overline{I}_{2}\right)\right] \overline{\boldsymbol{C}}^{-1}
$$

$$
\begin{aligned} \boldsymbol{C}=4 \frac{\partial^{2} \Psi}{\partial \boldsymbol{C} \partial \boldsymbol{C}}=& J^{-4 / 3}\left[2 \mu_{2}(\boldsymbol{I} \otimes \boldsymbol{I}-\boldsymbol{I})-\frac{2}{3}\left(\mu_{1}+2 \mu_{2} \overline{I}_{1}\right)\left(\boldsymbol{I} \otimes \overline{\boldsymbol{C}}^{-1}+\overline{\boldsymbol{C}}^{-1} \otimes \boldsymbol{I}\right)\right.\\ &+\frac{4}{3} \mu_{2}\left(\overline{\boldsymbol{C}} \otimes \overline{\boldsymbol{C}}^{-1}+\overline{\boldsymbol{C}}^{-1} \otimes \overline{\boldsymbol{C}}\right)+\left(\kappa J(2 J-1)+\frac{2}{9}\left(\mu_{1} \overline{I}_{1}+4 \mu_{2} \overline{I}_{2}\right)\right) \overline{\boldsymbol{C}}^{-1} \otimes \overline{\boldsymbol{C}}^{-1} ] \\ &-\left[2 \kappa J(J-1)-\frac{2}{3}\left(\mu_{1} \overline{I}_{1}+2 \mu_{2} \overline{I}_{2}\right)\right] \boldsymbol{Q} \end{aligned}
$$

where $Q_{i j k l}=\frac{1}{2}\left(C_{i k}^{-1} C_{j l}^{-1}+C_{i l}^{-1} C_{j k}^{-1}\right)$

##대변형 유한요소 수식화
###지배방정식 (governing equations)
![Aspect ratio](/media/POST/00007/1.jpg)

Governing equation (static) with respect to the reference configuration
$$
\nabla_{X} \cdot T=0 \quad \text { in } \quad V^{0}
$$
$$
\boldsymbol{T}^{T} \cdot \boldsymbol{n}^{0}=\boldsymbol{t}^{0} \quad \text { on } \quad S_{t}^{0}
$$
$$
\begin{array}{ll}{\text { where }} & {\boldsymbol{T}=J \boldsymbol{F}^{-1} \boldsymbol{\sigma} \quad : 1^{\text { st }} \text { Piola-Kirchhoff stress }} \\ {} & {J=\operatorname{det} \boldsymbol{F}}\end{array}
$$


Weak form in the reference configuration
$$
\int_{V^{0}} \boldsymbol{T} \cdots\left(\boldsymbol{w} \otimes \nabla_{X}\right) d V=\int_{S_{t}^{0}} \boldsymbol{t}^{0} \boldsymbol{w} d S
$$


Nonlinear for large deformation & large rotation


###Formulations with objective stress rates
Updated Lagrangian method with Truesdell stress rate
![Aspect ratio](/media/POST/00007/2.jpg)

Stress-strain relation
$$
\begin{array}{c}{\ell_{d u} \tau=F d S F^{T}=F\left(\boldsymbol{C} : \frac{1}{2} d \boldsymbol{C}\right) \boldsymbol{F}^{T}=\boldsymbol{c} :(d \boldsymbol{u} \otimes \nabla)} \\ {\text { where } \quad c_{i j k l}=F_{i l} F_{j J} F_{k K} F_{L L} C_{I K L} \quad d S_{L J}=C_{U K L} d E_{K L}}\end{array}
$$

###비선형 문제의 반복 계산
![Aspect ratio](/media/POST/00007/3.jpg)
$$
\begin{array}{l}{\mathbf{K}^{(n, i)} \Delta \mathbf{u}^{(n, i+1)}=\mathbf{P}^{(n+1)}-\mathbf{F}^{(n, i)}} \\ {\Delta \mathbf{u}^{(n+i)}=\mathbf{K}^{-1}\left(\mathbf{P}^{(n+1)}-\mathbf{F}^{(n, i)}\right)} \\ {\mathbf{u}^{(n+1)}=\mathbf{u}^{(n)}+\sum_{i} \Delta \mathbf{u}^{(n, i)}}\end{array}
$$

![Aspect ratio](/media/POST/00007/4.jpg)

##대변형 유한요소 수식화
###예제를 통한 수식화 검증
Large deformation analysis using Mooney-Rivlin material
![Aspect ratio](/media/POST/00007/5.jpg)
![Aspect ratio](/media/POST/00007/6.jpg)

$$
\begin{array}{l}{\mu_{1}=0.354 \quad \kappa=40.0} \\ {\mu_{2}=0.09}\end{array} \quad \Psi(C)=\frac{\mu_{1}}{2}\left(\overline{I}_{1}-3\right)+\frac{\mu_{2}}{2}\left(\overline{I}_{2}-3\right)+\frac{K}{2}(J-1)^{2}
$$

![Aspect ratio](/media/POST/00007/7.jpg)


##Input 파일 작성 및 프로그램 실행
###Input 파일 작성
![Aspect ratio](/media/POST/00007/8.jpg)

- 키워드는 항상 *로 시작하고, 다음 줄에 나타내는 데이터가 무엇인지를 나타냄
- 키워드 입력 시 대문자 또는 소문자 한 가지로 통일하여 입력 (혼용 시 에러 발생) 
- 키워드와 키워드 사이는 띄지 않고 연속적으로 입력
- 주석 및 코멘트 처리는 ** (* 두 번 이상) 입력 후 표시

   
![Aspect ratio](/media/POST/00007/9.jpg)

####사용 가능한 요소 타입
![Aspect ratio](/media/POST/00007/10.jpg)

![Aspect ratio](/media/POST/00007/11.jpg)
![Aspect ratio](/media/POST/00007/12.jpg)

![Aspect ratio](/media/POST/00007/13.jpg)


- SET에서 절점 또는 요소 번호의 마지막에 , 가 있는지 반드시 확인
- 절점기반의 NSET과 요소기반의 ELSET이 있고, 설정 해주지 않거나 여러 개 생성해도 무관 
- MPC의 기준 절점에 변위 경계조건 부여 가능

![Aspect ratio](/media/POST/00007/14.jpg)

####집중하중 부여 방법
![Aspect ratio](/media/POST/00007/15.jpg)

####분포하중 부여 방법
![Aspect ratio](/media/POST/00007/16.jpg)

하중의 부여는 집중하중의 경우 CLOAD, 분포하중의 경우 DLOAD를 이용

![Aspect ratio](/media/POST/00007/17.jpg)

※ *OUTPUT, FREQUENCY=N의 키워드를 쓸 경우 해석 결과 파일의 크기를 줄일 수 있음
   
###EDISON 프로그램 실행
![Aspect ratio](/media/POST/00007/18.jpg)
- 파일 업로드 시 ‘Menu’ 클릭 → ‘Upload’ 클릭
해석 input 파일(~.inp 파일) 업로드

![Aspect ratio](/media/POST/00007/19.jpg)

![Aspect ratio](/media/POST/00007/20.jpg)

![Aspect ratio](/media/POST/00007/21.jpg)
- .msg : 해석 중 발생하는 에러 내용 표시
- .out : 해석 정보 및 결과 파일 (text 형식)
- .plt : 해석 결과 파일 (요소간 불연속적인 응력 contour) 
- _s.plt : 해석 결과 파일 (요소간 연속적인 응력 contour)
결과 파일(_s.plt 파일) 다운받은 후 가시화 프로그램을 통해 결과 확인

##프로그램 해석 결과 확인
###가시화 프로그램 설치
![Aspect ratio](/media/POST/00007/22.jpg)

![Aspect ratio](/media/POST/00007/23.jpg)

![Aspect ratio](/media/POST/00007/24.jpg)

###가시화 프로그램 실행
- 설치 완료 후 바탕화면에 생성된 ‘CSDPost.exe’ 아이콘 더블 클릭 
- 시작 → 모든 프로그램 → CSD → CSDPost 실행

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
![Aspect ratio](/media/POST/00007/25.jpg)
    
####해석 결과 가시화
![Aspect ratio](/media/POST/00007/26.jpg)

- 메뉴 → File 클릭 → New 클릭
- OK 버튼을 클릭하여 새로운 프로젝트 생성(프로젝트 이름 및 경로는 임의로 지정 가능)

   
![Aspect ratio](/media/POST/00007/27.jpg)

- 메뉴 → Import 클릭 → Seoul Tech Output 클릭
- EDISON 프로그램으로 해석 완료 후 다운받은 결과 파일 (_s.plt 파일) 열기
 SEOULTECH
   
####해석 결과 가시화 – 변위 분포
![Aspect ratio](/media/POST/00007/28.jpg)

- 화면 왼쪽 하단의 Result 탭 클릭
- 왼쪽 트리에서 결과 확인 하고자 하는 CASE 선택 → Displacement → 좌표계 선택

   
####해석 결과 가시화 – 응력 분포
![Aspect ratio](/media/POST/00007/29.jpg)

- 결과 확인 하고자 하는 CASE 선택 → Stress → 응력 분포 선택
- 화면 오른쪽 상단의 Smooth 설정을 AVERAGE로 선택 → 하단의 Load 클릭
   