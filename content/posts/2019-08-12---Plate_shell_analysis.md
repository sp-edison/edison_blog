---
title: "Plate_shell_analysis"
date: "2019-08-12T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Plate_shell_analysis"
category: "Computational Structural Dynamics"
tags: 
 - "Plate"
 - "Membrane"
 - "Shell"
description: "Plate/Shell 구조해석 프로그램 Manual"
---
## Plate / Shell 구조 해석 이론 프로그램 매뉴얼

##목차
1. 개요
2. Plate의 굽힘 거동 강성행렬 
3. Membrane의 거동 강성행렬 
4. 행렬 안정화 기법
5. 프로그램 입력문 작성 양식식 
6. 예제
  
##1. 개요
###Plate / Shell 구조해석
- Plate : 평판의 굽힘 거동을 나타내는 요소
- Shell : 평판의 굽힘거동과 함께 평판의 막(membrane) 내 거동이 결합된 요소

###프로그램 개요
Plate, Shell 요소로 구성된 구조물의 구조해석 지원

###가정
- 평판의 중심면(midsurface)을 기준으로 하여 해석 수행 
- 중심면에 수직한 법선벡터는 변형 후에도 직선으로 존재
- 평판의 굽힘 거동을 나타내는 자유도 : $w, \beta_{x}, \beta_{y}$
- 평판의 막 내 거동을 나타내는 자유도 : $u, v$

![Aspect ratio](/media/POST/000032/0.jpg)

##2. 굽힘 및 전단 거동 강성행렬
###Equilibrium equation
$$
\begin{aligned} T_{x, x}+T_{y, y}+f_{z} &=0 \\ M_{x, x}+M_{x y y}-T_{x} &=0 \\ M_{x y x}+M_{y y}-T_{y} &=0 \end{aligned}
$$

- $T_{x}, T_{y}$ : 전단하중
- $M_{x}, M_{y}, M_{x y}$ : 굽힘 모멘트

참고 : 평판 굽힘거동에서 $u=z \beta_{x}, v=z \beta_{y}$,

$$
\begin{array}{ll}{T_{x}=\int_{-\frac{h}{2}}^{\frac{h}{2}} \sigma_{x z} d z,} & {T_{y}=\int_{-\frac{h}{2}}^{\frac{h}{2}} \sigma_{y z} d z} \\ {M_{x}=\int_{-\frac{h}{2}}^{\frac{h}{2}} \sigma_{x} z d z,} & {M_{y}=\int_{-\frac{h}{2}}^{\frac{h}{2}} \sigma_{y} z d z} \\ {M_{y}=} {\int_{-\frac{h}{2}}^{\frac{h}{2}} \sigma_{x y} z d z}\end{array}
$$

###Plate 강성행렬
굽힘 강성(bending stiffness)과 전단 강성(shear stiffness)의 결합으로 구성
$$
K_{\text {plate}}=K_{\text {bend}}+K_{\text {shear}}
$$

###Curvature vector $\chi$
곡률벡터 $\chi$는 중심면 법선벡터의 회전량으로 정의된다.
$$
x=\left\langle\beta_{x, x}, \quad \beta_{y, y}, \quad\left(\beta_{x, y}+\beta_{y, x}\right)\right\rangle
$$

###Constitutie relations
굽힘모멘트는 곡률벡터에 의해 다음과 같이 정리된다.
$$
\left\{\begin{array}{l}{M_{x}} \\ {M_{y}} \\ {M_{x y}}\end{array}\right\}=\frac{E h^{3}}{12\left(1-v^{2}\right)}\left[\begin{array}{ccc}{1} & {v} & {0} \\ {v} & {1} & {0} \\ {0} & {0} & {\frac{1}{2}(1-v}\end{array}\right]\left\{\begin{array}{c}{x_{x}} \\ {x_{y}} \\ {x_{x y}}\end{array}\right\}
$$

###Assumed shear strain
구성방정식(Constitutive relation)에서 전단하중 $T$는 전단 변형률 $\gamma$로 나타낼 수 있다.

$\gamma$대신 가정 전단 변형률 $\overline{\gamma}$를 이용하여 전단변형을 독립적인 자유도로 고려하면 다음과 같이 나타낼 수 있다.
$$
\left\{\begin{array}{l}{T_{x}} \\ {T_{y}}\end{array}\right\}=k G h\left\{\begin{array}{l}{\overline{\gamma}_{x z}} \\ {\overline{\gamma}_{y z}}\end{array}\right\}=D_{s}\left[\begin{array}{cc}{1} & {0} \\ {0} & {1}\end{array}\right]\left\{\begin{array}{l}{\overline{V}_{x z}} \\ {\overline{\gamma}_{y z}}\end{array}\right\}, \quad \text { where } G=\frac{E}{2(1+v)}
$$

$$
D_{s}=k G h, \quad k=\frac{5}{6}(\text { shear correction factor })
$$

###중심면 수직 회전의 고차 근사
중심면 수직 회전 $\beta_{x}, \beta_{y}$는 요소 모서리 절점의 회전량 $\beta_{x i}, \beta_{y i}$와 모서리 중점의 접선방향 회전량 $\Delta \beta_{S k}$로 근사될 수 있다.

![Aspect ratio](/media/POST/000032/1.jpg)

$$
\beta_{x}=\sum_{i=1}^{n n d} N_{i} \beta_{x i}+\sum_{i=n n d}^{n n d \cdot 2} P_{k} C_{k} \Delta \beta_{s k}
$$

$$
\beta_{y}=\sum_{i=1}^{n n d} N_{i} \beta_{y i}+\sum_{i=n n d}^{n n d \cdot 2} P_{k} S_{k} \Delta \beta_{s k}
$$

- $N_{i}$ :꼭지점의 형상함수
- $P_{k}$ :중앙 절점을 포함한 모서리만을 2차 근사한 형상함수 
- $C_{k}$ :모서리 중앙 절점$k$가 포함된 변의 cosine
- $S_{k}$ :모서리 중앙 절점$k$가 포함된 변의 sine
- $n n d$ : 요소당 절점 수

###중심면 수직 회전의 고차 근사
![Aspect ratio](/media/POST/000032/2.jpg)

- $N_{i}$ : 꼭지점의 형상함수
- $P_{k}$ : 중앙 절점을 포함한 모서리만을 2차 근사한 형상함수 
- $C_{k}$ : 모서리 중앙 절점$k$가 포함된 변의 cosine
- $S_{k}$ : 모서리 중앙 절점$k$가 포함된 변의 cosine
- $n n d$ : 요소당 절점 수

사각형 요소인 경우 $N_{1}=\frac{1}{4}(1-\xi)(1-\eta), P_{5}=\frac{1}{2}\left(1-\xi^{2}\right)(1-\eta)$

###곡률 벡터 $\chi$
곡률벡터 $\chi$는 $\boldsymbol{\beta}$의 미분으로 나타나므로 고차 근사한 $\boldsymbol{\beta}$를 이용하여 $\chi$를 구성할 수 있다.

![Aspect ratio](/media/POST/000032/3.jpg)

###Assumed shear strain field
앞서 정의된 가정 전단 변형률 \(\overline{\gamma}\)는 \(x z\)좌표계(전역좌표계)에서 정의되어 있다.

요소의 모서리마다 모서리 방향($𝑠$축)과 수직한 방향($𝑛𝑛$축)으로 이루어진 \(\overline{\gamma}_{s z}\)로 변환한다면
$$
\overline{\gamma}_{\mathrm{sz}}=\frac{T_{s}}{D_{s}}
$$

전단하중 \(T_{s}\)는 평형방정식에 의하여
$$
T_{s}=M_{s, s}+M_{n s, n}
$$

모멘트 \(M_{s, s}, M_{n s, n}\)는 고차 거동으로 근사된 중심면 수직회전량 \(\beta_{s}, \beta_{n}\)로 나타낼 수 있다.
$$
M_{s}=D_{b}\left(\beta_{s, s}+v \beta_{n, n}\right), \quad M_{n s}=\frac{D_{b}(1-v)}{2}\left(\beta_{s, n}+\beta_{n, s}\right)
$$

정리하면
$$
\overline{\gamma}_{\mathrm{sz}}=\frac{D_{b}}{D_{s}}\left(\beta_{s, s s}+v \beta_{n, n s}+\frac{(1-v)}{2}\left(\beta_{s, n n}+\beta_{n, n s}\right)\right)
$$

$$
\beta_{n}=\left(1-\frac{s}{L_{k}}\right) \beta_{n i}+\left(\frac{s}{L_{k}}\right) \beta_{n j}, 
$$

$$
\quad \beta_{s}=\left(1-\frac{s}{L_{k}}\right) \beta_{s i}+\left(\frac{s}{L_{k}}\right) \beta_{s j}+4 \frac{s}{L_{k}}\left(1-\frac{s}{L_{k}}\right) \Delta \beta_{s k}
$$

![Aspect ratio](/media/POST/000032/4.jpg)

$$
\overline{\gamma}_{\mathrm{sz}}=\frac{D_{b}}{D_{s}}\left(\beta_{s s s}+v \beta_{n, n s}+\frac{(1-v)}{2}\left(\beta_{s, n n}+\beta_{n, n s}\right)\right)
$$

$$
\overline{\gamma}_{\mathrm{szk}}=\frac{D_{b}}{D_{s}} \Delta \beta_{s, s s}=-\frac{2}{3} \phi_{k} \Delta \beta_{s k}
$$

$$
\quad \text { where } \phi_{k}(\text { shear influence factor })=\frac{2}{k(1-v)}\left(\frac{h^{2}}{L_{k}^{2}}\right)
$$

각 모서리 중점의 전단 변형률은 모서리 중점의 접선방향 회전량 \(\Delta \beta_{s k}\)로 나타낼 수 있으므로 전단 변형률 \(\overline{\gamma}_{x z}, \overline{y}_{y z}\)는 \(\Delta \beta_{S k}\)로 표현될 수 있다.

$$
\{\overline{\gamma}\}=\left\{\begin{array}{l}{\overline{\gamma}_{x z}} \\ {\overline{\gamma}_{y z}}\end{array}\right\}=\left[B_{s \Delta \beta}\right]\left\{\Delta \beta_{n}\right\}
$$

###Hu-Washizu functional
Plate의 굽힘 거동과 전단 거동에 대한 에너지는 범함수 (functional)로 나타낼 수 있다.

가정 전단 변형률을 이용하여 거동을 근사하였기 때문에 Hu-Washizu 범함수를 이용하여 전단과 굽힘 거동에 대한 에너지 범함수를 구성할 수 있다.

$$
\Pi=\Pi_{b}\left(\beta_{x}, \beta_{y}\right)+\Pi_{s}\left(w, \beta_{x}, \beta_{y},\{\overline{\gamma}\},\{T\}\right)-\int_{A^{e l e m e n t}} f_{z} w d A+\Pi_{e x t}
$$

$$
\text { where } \Pi_{b}=\frac{1}{2} \int_{A^{\text { clement }}}\{x\}^{T}\left[H_{b}\right]\{x\} d A,
$$

$$
\quad \Pi_{s}=\frac{1}{2} \int_{A^{\text { element }}}\{\overline{\gamma}\}^{T}\left[H_{s}\right]\{\overline{\gamma}\} d A+\int_{A^{\text { element }}}\{T\}^{T}(\{\gamma\}-\{\overline{\gamma}\}) d A
$$

변분법 (Variational method) 을 이용하면

$$
\int_{0}^{L_{k}} \delta T_{s}\left(\gamma_{s z}-\overline{\gamma}_{s z}\right) d s=0, \quad k=n n d+1, \ldots, 2 * n n d, \quad \gamma_{s z}=w_{s z}+\beta_{s}
$$


####1.
$$
\int_{0}^{L_{k}} \delta T_{s}\left(\gamma_{s z}-\overline{\gamma}_{s z}\right) d s=0, \quad k=n n d+1, \ldots, 2 * n n d, \quad \gamma_{s z}=w_{s}+\beta_{s}
$$

위 식을 풀면 모서리 중간 회전량 \(\Delta \beta_{n}\)은 절점 자유도의 식으로 구할 수 있다.
$$
\left\{\Delta \beta_{n}\right\}=\left[A_{n}\right]\left\{U_{n}\right\}
$$

$$
\left[A_{n}\right]=\left[A_{\Delta \beta}\right]^{-1}\left[A_{w}\right]
$$

$$
\text { where }\left[A_{\Delta \beta}\right]=\left[\begin{array}{cccc}{\frac{2}{3} L_{n n d+1}\left(1+\phi_{n n d+1}\right)} & {0} & {\cdots} & {0} \\ {0} & {\frac{2}{3} L_{n n d+2}\left(1+\phi_{n n d+2}\right)} & {\cdots} & {0} \\ {0} & {0} & {\ddots} & {0} \\ {0} & {0} & {\cdots} & {\frac{2}{3} L_{2+n n d}\left(1+\phi_{2 * n n d}\right)}\end{array}\right]
$$

####2.
$$
\int_{0}^{L_{k}} \delta T_{s}\left(\gamma_{s z}-\overline{\gamma}_{s z}\right) d s=0, \quad k=n n d+1, \ldots, 2 * n n d, \quad \gamma_{s z}=w_{s}+\beta_{s}
$$

위 식을 풀면 모서리 중간 회전량 $\Delta \beta_{n}$은 절점 자유도의 식으로 구할 수 있다.

$$
\left\{\Delta \beta_{n}\right\}=\left[A_{n}\right]\left\{U_{n}\right\}
$$

$$
\left[A_{n}\right]=\left[A_{\Delta \beta}\right]^{-1}\left[A_{w}\right]
$$

$$
\text { where }\left[A_{w}\right]=\left[\begin{array}{cccccccccc}{1} & {-\frac{x_{21}}{2}} & {-\frac{y_{21}}{2}} & {-1} & {-\frac{x_{21}}{2}} & {-\frac{y_{21}}{2}} & {\dots} & {0} & {0} & {0} \\ {0} & {0} & {0} & {1} & {-\frac{x_{32}}{2}} & {-\frac{y_{32}}{2}} & {\dots} & {0} & {0} & {0} \\ {\vdots} & {\vdots} & {\vdots} & {\vdots} & {\vdots} & {\vdots} & {\vdots} & {\vdots} & {\vdots} & {\vdots} \\ {-1} & {-\frac{x_{1(n n d)}}{2}} & {-\frac{y_{1(n n d)}}{2}} & {0} & {0} & {0} & {\cdots} & {1} & {-\frac{x_{1(n n d)}}{2}} & {-\frac{y_{1(n n d)}}{2}}\end{array}\right]
$$

- 굽힘 강성 행렬
$$
\{\chi\}=\left[B_{b}\right]\left\{U_{n}\right\}, \quad\left[B_{b}\right]=\left[B_{b \beta}\right]+\left[B_{b \Delta \beta}\right]\left[A_{n}\right]
$$

$$
K_{\text {bending}}=\int_{A \text { element }}\left[B_{b}\right]^{T}\left[H_{b}\right]\left[B_{b}\right] d A
$$

- 전단 강성 행렬
$$
\{\overline{\gamma}\}=\left[B_{s}\right]\left\{U_{n}\right\}, \quad\left[B_{s}\right]=\left[B_{s \Delta \beta}\right]\left[A_{n}\right]
$$

$$
K_{\text {shear}}=\int_{\text {A} \text { element }}\left[B_{s}\right]^{T}\left[H_{s}\right]\left[B_{s}\right] d A
$$

##3. 평면 막 거동 강성행렬
###Drilling degree of freedom

막 (membrane) 거동은 면에 평형한 변위로 나타난다.

Drilling D.O.F (회전자유도)를 이용하면 2차 요소보다 계산량이 적으면서 선형 요소에 비해 성능이 우수한 요소 구현이 가능하다.

![Aspect ratio](/media/POST/000032/5.jpg)

Drilling D.O.F (회전자유도)를 이용하면 2차 요소보다 계산량이 적으면서 선형 요소에 비해 성능이 우수한 요소 구현이 가능하다.

$$
\left\{\begin{array}{l}{u_{m}} \\ {v_{m}}\end{array}\right\}=\frac{1}{2}\left\{\begin{array}{l}{u_{i}} \\ {v_{i}}\end{array}\right\}+\frac{1}{2}\left\{\begin{array}{l}{u_{j}} \\ {v_{j}}\end{array}\right\}+\frac{\omega_{j}-\omega_{i}}{8}\left\{\begin{array}{l}{y_{j}-y_{i}} \\ {x_{i}-x_{j}}\end{array}\right\}
$$


$$
\left\{u_{1} v_{1} u_{2} v_{2} \ldots u_{2 * n n d-1} v_{2 * n n d-1} u_{2 * n n d} v_{2 * n n d}\right\} =[T]\left\{u_{1} v_{1} \omega_{1} u_{2} \quad v_{2} \quad \omega_{2} \quad \ldots \quad u_{n n d} \quad v_{n n d} \quad \omega_{n n d}\right\}^{T}
$$

![Aspect ratio](/media/POST/000032/6.jpg)

Drilling D.O.F (회전자유도)를 이용하면 2차 요소보다 계산량이 적으면서 선형 요소에 비해 성능이 우수한 요소 구현이 가능하다.

$$
\left[K_{\text {membrane}}\right]=\left[K_{\text {drilling do.f}}\right]=[T]^{T}\left[K_{\text {quadratic element}}\right][T]
$$

##4. 강성행렬 안정화 기법
###전체 강성행렬
평판의 굽힘, 전단거동과 평면막거동의 강성행렬을 합하면 요소의 전체 강성행렬을 구성할 수 있다.

$$
\left[K_{\text {element}}\right]=\left[K_{\text {bending}}\right]+\left[K_{\text {shear}}\right]+\left[K_{\text {membrane}}\right]
$$

막거동에 회전 자유도를 넣어 요소를 구성할 경우 에너지 없이 변형이 일어나는 영에너지모드 (zero energy mode)가 생길 수 있다.

꼭지점의 회전량이 참회전량을 갖도록 벌칙함수를 이용하여 면내 비틀림 강성을 부여하여 영에너지 모드를 제한할 수 있다.
$$
\left[\mathrm{K}_{\mathrm{pen}}\right]_{\mathrm{ij}}=\frac{\partial^{2} P}{\partial u_{i} \partial v_{j}}, \quad P=V G \gamma \Theta, \quad \theta=\frac{1}{3} \sum_{i=1}^{n n d}\left(\omega_{i}-\phi_{0}\right)=\frac{1}{3} \sum_{i=1}^{n n d}\left(\omega_{i}-\left(\frac{\partial v}{\partial x}-\frac{\partial u}{\partial y}\right)\right)
$$

$$
V=\text { volume, } \quad G=\text { shear modulus, } \quad \gamma \cong 10^{-6}
$$

##5. 프로그램 입력문 작성 양식
###(1) CONTROL PARAMETER
####사용 규칙
![Aspect ratio](/media/POST/000032/7.jpg)

$표시로 시작되는 줄 : 주석처리

###(2) NODE
####사용 규칙
![Aspect ratio](/media/POST/000032/8.jpg)

####사용 예
![Aspect ratio](/media/POST/000032/9.jpg)
![Aspect ratio](/media/POST/000032/10.jpg)

###(3) ELEMENT
####사용 가능 요소
![Aspect ratio](/media/POST/000032/11.jpg)

####사용 규칙
![Aspect ratio](/media/POST/000032/12.jpg)

####사용 예
![Aspect ratio](/media/POST/000032/13.jpg)

###(4) BOUNDARY
####특정 절점에 변위 경계조건을 부여할 때 사용
####사용 규칙
![Aspect ratio](/media/POST/000032/14.jpg)

####사용 예
![Aspect ratio](/media/POST/000032/15.jpg)

###(5) LOAD
![Aspect ratio](/media/POST/000032/16.jpg)
![Aspect ratio](/media/POST/000032/17.jpg)

####사용 예
![Aspect ratio](/media/POST/000032/18.jpg)

###(6) PHYSICAL
####사용 규칙
![Aspect ratio](/media/POST/000032/19.jpg)

####단면 특성의 종류
![Aspect ratio](/media/POST/000032/20.jpg)

####사용 예
![Aspect ratio](/media/POST/000032/21.jpg)

###(7) MATERIAL
####사용 규칙
![Aspect ratio](/media/POST/000032/22.jpg)

####사용 예
영계수 E =10000000, 포아송비 v = 0.3, 밀도  =1000의 특성을 가지는 선형 등방성 재료 특성을 정의하는 경우
![Aspect ratio](/media/POST/000032/23.jpg)

##Simulation Manual
![Aspect ratio](/media/POST/000032/27.jpg)

![Aspect ratio](/media/POST/000032/28.jpg)

![Aspect ratio](/media/POST/000032/29.jpg)

![Aspect ratio](/media/POST/000032/30.jpg)

##6. 예제
1. 문제의 정의 : Hemispherical shell
- 출처: Richard H. Macneal and Robert L. Harder, “ A proposed standard set of problems to test finite element accuracy”, Finite Elements in Analysis and Design, vol.1, pp.3-20, 1985
- 반구의 집중하중에 대한 변위 해석을 수행한다. 대칭성을 고려하여 1/4 모델을 사용한다.

![Aspect ratio](/media/POST/000032/24.jpg)

- 구 반지름 : 10m
- 붉은 점(x축 상에 위치)과 원점, 검은점 사이 각도 = 72°
- 두께 : 0.04mm
- 영계수 : 68.25Pa
- 포아송 비 : 0.3
- y=0인 평면 : xz plane symmetry($v$=0, $\theta_x$ = 0, $\theta_z$ = 0)
- x=0인 평면 : yz plane symmetry($x$=0, $\theta_y$ = 0, $\theta_z$ = 0)
- 좌측 상단 표시된 검은 점 : rigid body motion 막기 위하여 $w$ = 0
- $x$축 상의 점과 $y$축 상의 점에 각각 2$N$의 하중 부여

2. 입려문 양식 
함께 첨부된 Hemispherical.csd 참조

3. 해셕 결과(참조 해 : $x$ 방향 최대 변위 0.094)
![Aspect ratio](/media/POST/000032/25.jpg)

![Aspect ratio](/media/POST/000032/26.jpg)