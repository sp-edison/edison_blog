---
title: "CSD_EVPLAST_DAMAGE"
date: "2019-07-29T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/CSD_HYPERELAST_Mooney_Rivlin"
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
- Additive decomposition: $\dot{\boldsymbol{\varepsilon}}=\dot{\boldsymbol{\varepsilon}}^{e}+\dot{\boldsymbol{\varepsilon}}^{p}$
- Stress-strain relation (objective stress rate, hypoelastic response): $\dot{\boldsymbol{\sigma}}^{J}=\boldsymbol{c} : \dot{\boldsymbol{\varepsilon}}^{e}$
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

요소의 기하학적 특성 정보 및 재료 정보 [ 기본 값=1 ]
재료 물성 정보
*ELASTIC : 선형 탄성 계수 지정 [ 탄성 계수, 푸와송 비 ]
*PLASTIC : 재료의 소성 정의 [ 항복 응력, 소성 변형률 ]
Gurson Damage Parameters 정의 *POROUS METAL PLASTICITY [q1, q2, q3]
*VOID NUCLEATION [n , sn , fn ]
*POROUS FAILURE CRITERION
            [fF, fc ]
 fF fc fF  fc
q q2q f113
F
  여기서
   q3
 SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ Input 파일 작성 (4/6)
셋 정의 및 생성
*NSET, NSET=NAME : 절점 셋 [ 절점번호, ..., ... ]
*ELSET, ELSET=NAME : 요서 셋 [ 요소번호, ..., ... ]
다중 절점의 제약 조건 정의 및 생성
*MPC
[ TIE, 다중 절점 번호 또는 셋, 기준 절점 번호 ]
              TIE
     ※ SET에서 절점 또는 요소 번호의 마지막에 , 가 있는지 반드시 확인
※ 절점기반의 NSET과 요소기반의 ELSET이 있고, 설정 해주지 않거나 여러 개 생성해도 무관 ※ MPC의 기준 절점에 변위 경계조건 부여 가능
다중 절점 번호 또는 셋 기준 절점
 SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ Input 파일 작성 (5/6)
  *STEP, NLGEOM : 기하학적 비선형 고려 *STEP : 기하학적 비선형 미 고려
※ NLGEOM 키워드의 유무에 따라 ※ 기하학적 비선형 고려 여부 결정
해당 STEP에 대한 부연 설명(없어도 무관)
*STATIC : 정적 해석 정의
[ 초기 시간 증분, 한 스텝의 주기]
*BOUNDARY : 경계 조건 부여
[ 절점 번호 또는 절점 셋, 자유도, 자유도 ]
※ 자유도 적용
1, 3 : 1~3(x, y, z) 모두 구속 2, 3 : 2~3(y, z) 구속
1, 1 : 1(x) 구속
또는
자유도 뒤에 특정한 값 부여 가능
                      SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ Input 파일 작성 (6/6) ▪ 집중하중 부여 방법
      ▪ 분포하중 부여 방법
*CLOAD : 집중 하중 부여
[ 절점 번호 또는 절점 셋, 하중 방향, 하중 크기]
Input 파일 종료 선언
*DLOAD : 분포 하중 부여
[ 요소 번호 또는 요소 셋, TRVEC 면 번호, 트랙 션 크기, 방향 1, 방향 2, 방향 3]
또는
[ 요소 번호 또는 요소 셋, 하중 작용 면 번호, 분포하중 크기]
          ※ 하중의 부여는 집중하중의 경우 CLOAD, 분포하중의 경우 DLOAD를 이용
 SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ EDISON 프로그램 실행 (1/8)
     ▪ EDISON 구조동역학 홈페이지 접속 (https://csd.edison.re.kr/) ▪ 회원 가입 후 로그인
 SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ EDISON 프로그램 실행 (2/8) 1
    2
2
   ▪ 홈페이지 상단의 ‘통합검색’ 클릭
▪ 세부 목록 중 ‘초탄성 및 점탄성 구조물의 거동 해석’ 클릭
 SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ EDISON 프로그램 실행 (3/8)
  2
 ▪ 프로그램 목록 중 ‘Plastic Deformation with Damage Mechanism Analysis SW’ 클릭 ▪프로그램명우측의 ‘RUN’클릭→프로그램실행
 SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ EDISON 프로그램 실행 (4/8)
      ▪ 화면 좌측의 ‘New Simulation’ 클릭 → ‘시뮬레이션 명’ 입력 ▪ ‘Create’ 클릭하여 시뮬레이션 생성
 SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ EDISON 프로그램 실행 (5/8)
  1 2
       ▪ ‘Menu’ 클릭 → ‘Upload’ 클릭
▪ 해석 input 파일(~.inp 파일) 업로드
3
   SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ EDISON 프로그램 실행 (6/8)
   2
  1
  ▪ 화면 우측의 입력포트에서 ‘-inp’ 클릭 → 해석 하고자 하는 input 파일 선택 ▪ ‘Submit’ 클릭하여 작업 제출
3
  SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ EDISON 프로그램 실행 (7/8)
   2
  1
    ▪ 화면 좌측의 화살표 ‘   ’ 클릭 → 해석 상태 확인
▪ 또는 ‘시뮬레이션’ → ‘모니터링’ 클릭 후 상태 확인 ( S : 완료, F : 실패, R : 진행 중)
: 작업 제출 : 작업 진행 : 작업 성공
   SEOULTECH
   
 Input 파일 작성 및 프로그램 실행
 ➢ EDISON 프로그램 실행 (8/8)
    1
  2
 - <Input file>.msg : 해석 중 발생하는 에러 내용 표시
- <Input file>.out : 해석 정보 및 결과 파일 (text 형식)
- <Input file>.plt : 해석 결과 파일 (요소간 불연속적인 응력 contour) - <Input file>_s.plt : 해석 결과 파일 (요소간 연속적인 응력 contour)
▪ 해석 완료 후 결과 다운로드 아이콘 ‘   ‘ 클릭
▪ 결과 파일(_s.plt 파일) 다운받은 후 가시화 프로그램을 통해 결과 확인
 SEOULTECH
   
 프로그램 해석 결과 확인
 ➢ 가시화 프로그램 설치 1
   3
  2
▪ 홈페이지 상단의 ‘통합검색’ → 세부 목록 중 ‘기타’ 클릭 ▪ EdisonPrePost 다운로드 후 프로그램 설치 진행
➢ 가시화 프로그램 실행
▪ 설치 완료 후 바탕화면에 생성된 ‘CSDPost.exe’ 아이콘 더블 클릭 ▪ 시작 → 모든 프로그램 → CSD → CSDPost 실행
    SEOULTECH
   
 프로그램 해석 결과 확인
 ➢ 프로그램 조작 및 설정 방법 ▪ 마우스 조작 방법
- 이동 : Ctrl + 마우스 오른쪽 버튼 - 회전 : Ctrl + 마우스 왼쪽 버튼
- 확대 및 축소 : 마우스 휠 이동
▪ 배경 설정 방법
- File → Option → Preference → Background → 배경색 설정
▪ Contour 및 Scale factor 설정 방법
- File → Option → Result → Set Contour → Contour 설정 - File → Option → Result → Set Deform → Deform 설정
   SEOULTECH
    
 프로그램 해석 결과 확인
 ➢ 해석 결과 가시화
▪ 메뉴 → File 클릭 → New 클릭
▪ OK 버튼을 클릭하여 새로운 프로젝트 생성(프로젝트 이름 및 경로는 임의로 지정 가능)
      SEOULTECH
   
 프로그램 해석 결과 확인
 ➢ 해석 결과 가시화
    ▪ 메뉴 → Import 클릭 → Seoul Tech Output 클릭
▪ EDISON 프로그램으로 해석 완료 후 다운받은 결과 파일 (_s.plt 파일) 열기
 SEOULTECH
   
 프로그램 해석 결과 확인
 ➢ 해석 결과 가시화 – 변위 분포
▪ 화면 왼쪽 하단의 Result 탭 클릭
▪ 왼쪽 트리에서 결과 확인 하고자 하는 CASE 선택 → Displacement → 좌표계 선택
    SEOULTECH
   
 프로그램 해석 결과 확인
 ➢ 해석 결과 가시화 – 응력 분포
  - von-Mises : von-Mises 응력 - XX, YY, ZZ : 수직 응력
- XY, YZ, ZX : 전단 응력
- PEEQ : 등가 소성 변형률
- VOID : 공동체적분율
   ▪ 결과 확인 하고자 하는 CASE 선택 → Stress → 응력 분포 선택
▪ 화면 오른쪽 상단의 Smooth 설정을 AVERAGE로 선택 → 하단의 Load 클릭
 SEOULTECH
   