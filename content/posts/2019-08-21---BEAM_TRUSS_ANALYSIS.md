---
title: "BEAM_TRUSS_ANALYSIS"
date: "2019-08-21T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/BEAM_TRUSS_ANALYSIS"
category: "Computational Structural Dynamics"
tags: 
 - "Beam"
 - "Truss"
 - "GA"
description: "Beam/Truss linear analysis program Manual"
---
##Beam/Truss 구조 해석 프로그램 매뉴얼
##목차
###1. 유한요소법
####1.1 유한요소법의 개념
####1.2 변분 방정식

###2. 빔/트러스 요소 설명
####2.1 트러스 요소 
- 2.1.1 형상 함수 
- 2.1.2 강성 행렬

####2.2빔요소
- 2.2.1 형상 함수 
- 2.2.2 강성 행렬

###3. 입력문 형식 세부 설명
####3.1입력문 예제
- 3.1.1 CONTROL PARAMETER 
- 3.1.2 ELEMENT
- 3.1.3 NODE
- 3.1.4 BOUNDARY
- 3.1.5 LOAD
- 3.1.6 MATERIAL
- 3.1.7 PHYSICAL

###4. 튜토리얼
####4.1 집중하중을 받는 외팔보의 해석

##1. 유한요소법
###1.1. 유한요소법의 개념
유한요소법은 탄성체문제 또는 장(field equation)문제를 풀기 위한 기법이다. 외팔보에 전단력이 가해졌을 경우 이론적인 변위와 응력은 수계산을 통해 직접 구할 수 있다. 그러나 복잡한 형상을 가지는 모델에 대한 해석이 필요할 경우, 수계산으로 해를 구하기 어렵다. 이를 해결하기 위하여 복잡한 형상의 모델을 삼각형, 사각형, 사면체 또는 육면체로 구성된 작은 요소(element)들로 분할하고 요소 내와 요소 간의 힘의 평형을 고려하면, 모델의 응력, 변위를 구할 수 있는 행렬 방정식을 구성할 수 있다. 이 때, 유한요소법을 이용하여 구성한 행렬 방정식은 실제 모델을 유한요소로 이산화한 것이기 때문에 정 해가 아닌 근사해(approximate solution)가 된다. 이 근사해는 요소의 크기가 작을수록, 요소의 성능이 좋을수록 유한요소법을 이용한 해석결과는 정 해에 더 가까워진다. 다음 그림과 같은 문제가 있을 경우, 문제의 영역은 2 개의 유한 요소로 이산화되었고 이산화 된 요소들은 요소를 구성하는 절점(node)으로 구성되어있다. 모든 요소에서 변위와 하중의 관계를 나타내주는 강성행렬을 구성한 후, 이를 하나의 행렬로 조립하면 해석할 모델 의 이산화된 행렬 방정식을 만들 수 있다.

![Aspect ratio](/media/POST/000056/0.jpg)

![Aspect ratio](/media/POST/000056/1.jpg)

이산화된 요소는 매우 간단하므로 요소의 평형방정식과 요소의 강성행렬등은 표준화 된 요소 함수에 의해 쉽게 계산된다. 행렬의 조립과 비슷한 방법으로 외력힘 벡터{$\overline{F_b}$} 도 조립된다. 조립된 시스템 방정식에서 SPC(단일 절점) 변위 경계조건{$\overline{U_a}$}을 부여한다. 그리고 미지의 변위{$U_b$}에 관해 가우스 소거법을 이용하여 행렬식을 푼다. 기본 해인 미지변위와 SPC 값을 이용하여 미지 반력{$\overline{F_a}$}및 요소내 응력{$\sigma$}을 구한다. 이상이 탄성체 문제에서의 개략적인 유한요소법의 절차이다.

모델을 정의하는 것은 해석하고자 하는 대상의 기하학적 자료를 유한요소로 표현하고 모델에 작용하는 하중 및 변위 제한조건을 부여하는 과정을 의미한다.

주어진 문제는 각 부분의 물리적인 거동 특성을 고려하여 Truss, Beam, Solid, Rigid Bar, Spring 등으로 모델링을 해야 한다. 동일한 대상에 대하여 다른 형식의 요소로 모델링 할 수 있는데 이는 부재의 거동, 설계 관심부분, 해석의 정확도 등에 따라 사용자의 지식, 경험 등에 의존하여 결정된다. 일단 해석 대상이 지원되는 요소로 모델링이 되었다 면 요소의 기하학적 정의는 요소를 구성하는 절점 및 요소의 단면계수에 의해 이루어진다.

###1.2. 변분 방정식
모든 자연현상은 열역학 제1법칙을 만족한다. 이는 에너지의 보존 및 변환의 과정을 지배하는 법칙으로서 탄성체의 거동 또한 에너지와 일의 개념으로 지배방정식을 유도할 수 있다. 역학적 평형상태란 에너지가 변동이 없는 고정된(stationary) 상태를 의미한다. 중력작용하에서의 평형상태를 나타내는 아래 그림에서 고정된 포텐셜에너지(potential energy)의 상태에는 불안정한 상태와 안정된 상태의 두 가지가 있음을 보여주고 있다. 이 두 가지 중에서 안정된 상태는 에너지가 최소값을 가질 때를 말하고 불안정한 상태에서는 최소값이 아님을 알 수 있다. 즉 자연 현상에서 안정된 정적평형이라고 하는 것은 반드시 포텐셜 에너지가 최소가 되어야만 하는 것이다.

![Aspect ratio](/media/POST/000056/2.jpg)

변분법(variational method)이란 원래 편미분 방정식을 적분형식으로 바꾸어 푸는 데 있어서 포텐셜에너지와 유사한 일종의 가상 에너지를 - 이것을 수학에서는 범함수 (functional)라고 한다 - 도입하고 이에 대한 정류점(stationary point)을 찾기 위해 편미분 혹은 변분(variation)이 영(zero)라는 조건을 구하는 방법을 말한다. 여기서 수학적 범함수 에는 여러 가지 종류가 있을 수 있으나 지배방정식의 유형과 원하는 해의 종류에 따라 다르게 적용한다. 예를 들어 유체의 유동이나 열전달 문제 등의 장문제(field problem)에 서는 탄성체 문제의 포텐셜에너지와 같은 개념을 사용할 수 없다. 따라서 해당 문제의 지배방정식으로부터 변분법에 의한 유한요소 정식화가 이루어질 것이다. 탄성체 문제에 서는 포텐셜에너지 방법이 열역학적 법칙과 합치되므로 굳이 변분법을 사용할 필요는 없으나, 사실상 변분법을 이용하여 평형방정식으로부터 앞으로 설명될 포텐셜에너지 식이 유도됨이 증명된다.

지금 외력을 받아 변형되어 평형을 이루고 있는 탄성체를 하나의 계(system) 로 생 각하면, 계의 포텐셜에너지 Π 는 탄성체의 변형률에너지 Λ 에서 외력에 의한 포텐셜 W를 뺀 값이다.

$$
\Pi=\Lambda-W
$$

에너지를 어떤 변수들의 함수로 표현한다면 정적평형상태를 구하기 위해서는 위 식을 최소로 하는 변수를 찾으면 된다. 즉 그 변수를 유한요소의 변위(displacement)
{${U}^T$}={${U_1,U_2,U_3,\cdots \cdots}$}로 택한다면 다음 식으로부터 평형상태의 변위를 구할 수 있다.

$$
\frac{\partial\Pi}{\partial U_1}=0; \frac{\partial\Pi}{\partial U_2}=0; \frac{\partial\Pi}{\partial U_3}=0; \cdots\cdots
$$

변형률 에너지는 탄성론의 정의에 의해 다음과 같은 영역적분으로부터 구해진다.

$$
\Lambda=\int\limits_V \frac{1}{2} \begin{Bmatrix}\varepsilon\end{Bmatrix}^T\begin{Bmatrix}\sigma\end{Bmatrix}dV
$$

여기서 적분기호 안의 값은 단위체적당 변형률 에너지이다. 응력값은 변형률 항들을 이용하여 쓸 수 있다. 그리고 변형률은 변위의 미분식으로부터 구할 수 있고 영역변위 {u}는 유한요소변위 혹은 절점변위(nodal displacement) {U}로 부터 보간(interpolation) 법에 의해 다음과 같이 구해진다.

$$
\begin{Bmatrix}u\end{Bmatrix}=[N]\begin{Bmatrix}U\end{Bmatrix}
$$

[N]은 보간행렬(형상함수로 구성)를 의미한다. 즉, 변형률은 절점변위를 이용하여 다음과 같은 행렬식으로 표현된다.

여기서 [B] 행렬은 구배행렬(gradient matrix) 이라고 하고, 이는 보간행렬 [N] 을 변 형률 연산자에 맞게 구함(편미분)을 의미한다. 이러한 행렬들의 계산절차는 일차원, 이차 원, 삼차원에 맞게 다음에 나오는 절에서 상세히 언급될 것이다. 

따라서 변형률에너지가 유한요소변위를 변수로 하는 함수로 표현될 수 있다.

$$
\Lambda=\frac{1}{2}\int\limits_V \begin{Bmatrix}\varepsilon\end{Bmatrix}^T [D]\begin{Bmatrix}\varepsilon\end{Bmatrix}dV=\frac{1}{2}\int\limits_V \begin{Bmatrix}U\end{Bmatrix}T \bigg([B]^T [D] [B]\bigg)\begin{Bmatrix}U\end{Bmatrix}dV
$$

외력에 의해 행해진 일은 체적력 {$b$}의 경우 영역적분에 의해서, 표면력 {$t$}의 경우 경계적분에 의해서 다음과 같이 구해진다.

$$
W=\int\limits_V \begin{Bmatrix}u\end{Bmatrix}^T\begin{Bmatrix}b\end{Bmatrix}dV + \int\limits_S\begin{Bmatrix}u\end{Bmatrix}^T\begin{Bmatrix}\overline{t}\end{Bmatrix}dS
$$

절점변위를 이용하면 하중을 절점 변위의 형태로 나타낼 수 있다.

$$
W=\int\limits_V\begin{Bmatrix}U\end{Bmatrix}^T[N]^T\begin{Bmatrix}b\end{Bmatrix}dV+\int\limits_S\begin{Bmatrix}U\end{Bmatrix}^T[\overline{N}]^T\begin{Bmatrix}\overline{t}\end{Bmatrix}dS
$$

실제 적분을 수행함에 있어서 위의 우변의 두번째 항은 $S_t$ 경계에서의 적분이므로 영역보간에서 쓰인 [$N$]행렬을 경계좌표계로 변환하여([$\overline{N}$]) 수행된다.
이제 유한요소변위의 함수로 된 식들을 대입하고 포텐셜에너지의 최소화 방정식에 적용하면 다음과 같은 방정식을 얻게 된다.

$$
\Pi=\Lambda-W=\frac{1}{2}\int\limits_V\begin{Bmatrix}U\end{Bmatrix}^T\bigg([B]^T[D][B]\bigg)\begin{Bmatrix}U\end{Bmatrix}dV\\
-\Bigg(\int\limits_V\begin{Bmatrix}U\end{Bmatrix}^T[N]^T\begin{Bmatrix}b\end {Bmatrix}+\int\limits_S \begin{Bmatrix}U\end{Bmatrix}^T[\overline{N}]^T\begin{Bmatrix}\overline{t}\end{Bmatrix}dS \Bigg)
$$

$$
\frac{\partial\Pi}{\partial\left\{ U \right\}}=\left\{\frac{\partial\Pi}{\partial U_1}, \frac{\partial\Pi}{\partial U_1}, \cdots\cdots \right\}\\
=\int\limits_V[B]^T[D][B]\left\{U \right\}dV-\int\limits_V[N]^T \left\{ b \right\}dV-\int\limits_S[\overline{N}]^T\left\{\overline{t} \right\}dS=0
$$

따라서 위 식은 정적 평형상태의 변수인 유한요소변위 {$U$}를 풀 수 있는 대수 (algebraic) 연립방정식이 된다. 여기서 $[N] , [B] , [D]$ 행렬들을 포함한 적분을 수행하면 상수값들이 된다.

이 식을 다시 정리하면 다음과 같은 유한요소 평형방정식이 얻어진다.

$$
[K]\left\{ U\right\}=\left\{ F\right\}
$$

여기서 좌변의 행렬 $[K]$를 강성행렬(stiffness matrix), 우변의 배열{$F$}를 외부 외력 벡터(external load vector)라 하고 다음과 같다.

$$
[K]=\int\limits_V[B]^T[D][B]dV
$$

$$
\left\{ F\right\}=\int\limits_V[N]^T\left\{ b\right\}dV+\int\limits_S[\overline{N}]^T\left\{ \overline{t}\right\}dS
$$

실제 적분을 수행함에 있어서 위의 우변의 두번째 항은 $S_t$ 경계에서의 적분이므로 영역보간에서 쓰인 $[N]$행렬을 경계좌표계로 변환하여($[N]$) 수행된다.
이제 유한요소변위의 함수로 된 식들을 대입하고 포텐셜에너지의 최소화 방정식에 적용하면 다음과 같은 방정식을 얻게 된다.

$$
\Pi=\Lambda-W=\frac{1}{2}\int\limits_V\left\{ U\right\}^T\Big( [B]^T[D][B]\Big)\left\{U \right\}dV\\
-\Bigg(\int\limits_V\left\{ U\right\}^T[N]^T\left\{ b\right\}dV+\int\limits_S\left\{ U\right\}^T[\overline{N}]^T\left\{ \overline{t}\right\}dS \Bigg)
$$

$$
\frac{\partial\Pi}{\partial\left\{ U\right\}}=\frac{\partial\Pi}{\partial U_1}, \frac{\partial\Pi}{\partial U_1}, \cdots\cdots\\
=\int\limits_V[B]^T[D][B]\left\{ U\right\}dV-\int\limits_V[N]^T\left\{ b\right\}dV-\int\limits_S[\overline{N}]^T\left\{ \overline{t}\right\}dS=0
$$

따라서 위 식은 정적 평형상태의 변수인 유한요소변위 {$U$}를 풀 수 있는 대수 (algebraic) 연립방정식이 된다. 여기서 $[N] , [B] , [D]$ 행렬들을 포함한 적분을 수행하면 상수값들이 된다.

이 식을 다시 정리하면 다음과 같은 유한요소 평형방정식이 얻어진다.

$$
[K]\left\{ U\right\}=\left\{ F\right\}
$$

여기서 좌변의 행렬 $[K]$를 강성행렬(stiffness matrix), 우변의 배열{$F$}를 외부 외력 벡터(external load vector)라 하고 다음과 같다.

$$
[K]=\int\limits_V[B]^T[D][B]dV
$$

$$
\left\{ F\right\}=\int\limits_V[N]^T\left\{ b\right\}dV+\int\limits[\overline{N}]^T\left\{\overline{t}\right\}dS
$$

만약 절점변위 {$U$}의 미지수가 $n$개 라면 배열 {$F$}의 개수도 $n$개이고 행렬 $[K]$ 의 차원은 $(n \times n)$ 의 정방행렬(square matrix)이 되어 역행렬(inverse matrix)방법이나 가우스 소거법(Gaussian elimination) 등에 의해{$U$}를 구할 수 있다.

실제로 강성행렬과 절점 외력벡터를 구할 때는 적분의 영역을 이산화된 하나의 요소 에 국한하여 쉽게 구하고 모든 요소방정식을 시스템방정식에 조립(assembling)하는 방법을 쓰게 된다. 즉,

$$
[K]=\sum_{e=1}^{nel}\int\limits_{V^e}[B^e][D][B^e]dV^e=\sum_{e=1}^{nel}[K^e]
$$

$$
\left\{ F\right\}=\sum_{e=1}^{nel}\int\limits_{V^e}[N^e]^T\left\{ b^e\right\}dV^e+\int\limits_{S^e}[\overline{N}^e]^T\left\{ \overline{t}^e\right\}dS^e=\sum_{e=1}^{nel}\left\{F^e\right\}
$$

여기서 $e$ 는 각각의 요소를, $nel$ 은 요소의 총 수를 의미하고 행렬들은 요소내부에서 정의되었다.

##2. 빔/트러스요소
###2.1. 트러스 요소
####2.1.1. 트러스 요소의 형상 함수
단순 인장 혹은 압축만을 받도록 설계된 되어 있는 균일단면 부재를 유한요소로 아래 그림에 표시하였다. 부재의 양 끝의 절점을 잇는 선분을 따라 국부좌표계(local
coordinate system)를 잡고 x 방향으로의 절점 변위를 $u_1 , u_2$ 라 하면 영역변위는 다음과 같이 한다.

![Aspect ratio](/media/POST/000056/3.jpg)

여기서 보간함수 혹은 형상함수(shape function)는 다음과 같다.

$$
N_1=1-\frac{x}{L}; \;\;\;\;\;\; N_2=\frac{x}{L}
$$

형상함수와 절점의 변위를 이용하여 $u$ 를 근사적으로 나타낼 수 있다. 형상함수를 이용하면 각 절점에서 보간된 u는 원래 절점의 변위 $u_1,u_2$와 같게 된다. 이는 형상함수 의 특징으로, 형상함수들은 함수 값이 1 이되는 한 절점이 존재하고, 나머지 절점에선 0 을 가지는 함수이다. 그렇기 때문에 보간을 취하여도 원래 절점에서는 주어진 값을 갖게 된다. 또한 요소 내에서는 형상함수의 합이 언제나 1이 되기 때문에 강체 변위를 표현할 수 있다.

####2.1.2. 트러스 요소의 강성행렬
축방향 변형률은 변위식을 미분하면

$$
\varepsilon_{xx}=\frac{du}{dx}=\frac{dN_1}{dx}u_1+\frac{dN_2}{dx}u_2=[\frac{dN_1}{dx} \;\;\; \frac{dN_2}{dx}]
\begin{Bmatrix}
u_1\\
u_2
\end{Bmatrix}=[B]\left\{U\right\}
$$

여기서 구배행렬 $[B]$는 위 식을 미분하여 다음과 같이 상수행렬이 된다.
$$
[B]=[B_1 \;\;\; B_2]=[\frac{-1}{L} \;\;\; \frac{1}{L}]
$$

영이 아닌 응력이 오직 $\sigma_{xx}$ 뿐인 일차원 문제에 대한 구성행렬은

$\left\{ \sigma_{xx}\right\}=[E]\left\{ \varepsilon_{xx}\right\}=[D]\left\{ \varepsilon_{xx}\right\}$ 와 같이 되므로 이를 대입하면 강성행렬은 다음과 같이 구해진다.

$$
[K]=\int\limits_V[B]^T[D][B]dV=\int\limits_0^L 
\begin{Bmatrix}
\frac{-1}{L}\\
\frac{1}{L}
\end{Bmatrix}
[E][\frac{-1}{L} \;\;\; \frac{1}{L}]Adx\\
=\frac{AE}{L^2}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
\int\limits_0^Ldx=\frac{AE}{L}
\begin{bmatrix}
1 & -1\\
-1 & 1
\end{bmatrix}
$$

체적력과 표면력이 요소에 작용하고 있으면 이 힘들의 상당 절점 외력벡터 {F}를 구할 수 있다. 그러나 대부분의 트러스 구조물에 적용되는 하중은 절점에 걸리는 집중하중(concentrated load)이다. 집중하중의 경우에는 조립과정이 필요없고 단지 하중벡터에 더해주면 된다. 주의할 점은 요소 행렬은 국부좌표계에 대해서 구해졌다는 것이다. 이러 한 요소행렬의 조립과정에서는 요소행렬을 조립하기 전에 전체좌표계(global coordinate system)의 값으로 변환(transformation)하는 과정을 거쳐야 한다.

###2.2. 빔 요소
####2.2.1. 빔 요소의 형상 함수
빔 요소는 트러스 요소의 축방향 변위에 굽힘 변형을 나타내는 수직 변위와 회전 변 위를 추가한 요소이다. Euler 와 Bernoulli 에 의해 유도된 미분방정식은 빔의 처짐을 나타 내는데 다음과 같다.

$$
EI\frac{d^4\nu}{d^4x}=0
$$

위 식을 풀기 위해선 처짐을 나타내는 $\nu$가 3차 다항식보다 차수가 낮아야 한다. $\nu$ 를 3차 방정식이라 가정하여 식으로 나타내면 다음과 같다.

$$
\nu(x)=a_1x^3+a_2x^2+a_3x+a_4
$$

여기서 $\nu$를 구하기 위해 필요한 조건은 총 네 개이다. 빔의 양끝 단절점1,2에서 각각 처짐 변위를 알고, 미소 회전량 $\theta_1$ , $\theta_2$ 은 각각
 $\frac{d\nu_1}{dx}, \frac{d\nu_2}{dx}$ 를 의미하므로 네 값을 알게 된다면 빔의 처짐 변위식을 계산할 수 있다. 따라서 이 네 개의 물리량을 절점의 자유도 (degree of freedom, d.o.f)로 설정하면 요소의 형상함수를 구성할 수 있다.

![Aspect ratio](/media/POST/000056/4.jpg)

$$
\nu(0)=\nu_1=a_4\\
\frac{dv}{dx}(0)=\theta_1=a_3\\
\nu(L)=\nu_2=a_1L^3+a_2L^2+a_3L+a_4\\
\frac{d\nu}{dx}(L)=\theta_2=3a_1L^2+2a_2L+a_3
$$

위 식을 풀면 계수 $a_i$들을 구할 수 있고 절점의 자유도와 보간행렬의 곱 형태로 처짐식을 계산하면 빔요소의 형상함수를 구할 수 있다.

$$
\nu=[N]\left\{U\right\}=[N_1 \;\;\;\; N_2 \;\;\;\; N_3 \;\;\;\; N_4]\left\{U\right\}\\
=\Big[\frac{1}{L^3}(2x^3-3x^2L+L^3) \;\frac{1}{L^3}(x^3L-sx^2L^2+xL^3)\;\\
\frac{1}{L^3}(-2x^3+3x^2L)\;\frac{1}{L^3}(x^3L-x^2L^2)\Big]\begin{Bmatrix} \nu_1 \\ 
\theta_1 \\
\nu_2 \\
\theta_2
\end{Bmatrix}
$$

위의 $[N]$을 구성하는 항이 차례대로 $v_1, \theta_1, v_2, \theta_2$의 자유도에 해당하는 형상함수 를 의미한다. 빔 요소의 형상함수는 각각 처짐 변위와 회전 변위를 나타내기 때문에 $N_1, N_3$는 각각 $x=0,x=L$에서 1을 만족하고 다른 절점, 자유도에선 0을 만족하며, $N_2, N_4$는 $x$에 대해 미분한 함수가 $x=0,x=L$에서 1을 만족하고 다른 절점, 자유도에선 0 을 만족한다.

![Aspect ratio](/media/POST/000056/5.jpg)

![Aspect ratio](/media/POST/000056/6.jpg)

####2.2.2. 빔 요소의 강성행렬 (2 차원 기준)
빔 요소의 경우 강성행렬은 요소의 처짐변위, 회전변위, 축방향 변위와 관련된 항이 포함되어야 한다. 각 절점에서 3 개의 자유도를 표현해야 하므로 강성행렬의 크기는 6x6 이 된다. 여기서 빔 요소의 축방향 강성행렬은 트러스 요소의 강성행렬과 동일하다.

먼저 처짐과 회전에 관련된 곳만 고려하자. 이때의 강성행렬은 아래와 같다.

$$
\begin{bmatrix}
K_{11} & k_{12} & k_{13} & k_{14}\\
k_{21} & k_{22} & k_{23} & k_{24}\\
k_{31} & k_{32} & k_{33} & k_{34}\\
k_{41} & k_{42} & k_{43} & k_{44}
\end{bmatrix}
$$

빔요소의 강성행렬은 요소에 가해지는 처짐 변위, 회전 변위를 이론을 이용하여 계산할 수 있다. 강성행렬의 각 열벡터는 해당 자유도를 단위 크기만큼 변형시키기 위해 필요한 물리량을 나타낸다. 따라서 각 절점에 단위 처짐 변위 또는 단위 회전 변위를 발생시키기 위한 힘과 모멘트를 계산하여 요소의 강성행렬을 구성할 수 있다.

![Aspect ratio](/media/POST/000056/7.jpg)

그림(1)은 절점 1번(왼쪽 절점)의 처짐 변위를 1, 회전 변위를 0으로 만드는 경우를 나타내고,이때의 하중과 모멘트는 $k_{11}, k_{21}, k_{31}, k_{41}$로표시할수있다.빔이론에서 처짐과 회전 변위를 나타내는 식은 다음과 같다.

$$
\nu_1=1=\frac{k_{11}L^3}{3EI_z}-\frac{k_{21}L^2}{2EI_z}, \;\;\; \theta_1=0=-\frac{k_{11}L^2}{2EI_z}+\frac{k_{21}L}{EI_z}
$$

여기서 $E$는 영의 계수, $I_z$ 는 빔 단면의 moment of inertia를 의미한다. 위 식을 풀어 $k_11$과 $k_21$을 계산할 수 있다. $k_{11}$,$k_{21}$을 계산하였으므로 빔의 평형을 고려하여 $k_{31}$, $k_{41}$ 을 계산할 수 있다.

$$
\text{force}:K_{11}+k_{31}=0, \;\;\; \text{moment}: k_{21}+k_{41}-k_{11}L=0
$$

위 식들을 정리하면 강성행렬 첫 열벡터는 다음과 같다.

$$
\left\{ k_{11} \;\; k_{21} \;\; k_{31} \;\; k_{41} \right\}^T =\begin{Bmatrix}\frac{12EI_z}{L^3} \frac{6EI_z}{L^2}-\frac{12EI_z}{L^3}\frac{6EI_z}{L^2} \end{Bmatrix}
$$

같은 방법으로 1 번 절점의 회전변위만 존재하는 경우를 구하여 강성행렬의 2 열 벡터를 구할 수 있다.

$$
\nu_1=0=\frac{k_{12}L^3}{EI_z}-\frac{k_{22}L^2}{2E_z}, \theta_1=0=-\frac{k_{12}L^2}{2EI_z}+\frac{k_{22}L}{EI_z}
$$

$$
\text{force}: k_{12}+k_{32}=0, \;\; \text{moment}: k_{22}+k_{42}-k_{12}L=0
$$

2 번 절점에 대해서도 동일한 계산을 수행하면 다음과 같이 빔 요소의 강성행렬을 구할 수 있다.
![Aspect ratio](/media/POST/000056/8.jpg)

이 강성행렬은 빔의 처짐과 회전만 고려한 것이다. 여기에 축 방향 강성행렬을 추가해주면 다음과 같은 강성행렬을 구성할 수 있다.
![Aspect ratio](/media/POST/000056/9.jpg)

##3. 입력문 형식
입력 파일은 Free Field 형식으로 모델 데이터를 정의하는 독자적인 명령어 체계를 가지는 CSD 파일이다. 사용자가 직접 입력문을 수정해야 할 경우의 사용자 편의성을 대폭 향상시켰다. 입력 파일의 입력 형식에 대해서 간략하게 설명하면 다음과 같다.

입력문은 쉼표를 통해서 구분되고 각 자료의 크기가 통일되지 않아도 되는 Free field 방식을 취하고 있다. 따라서 명령문의 각 자료들은 크기나 타입에 관계없이 대부분의 경우 쉼표를 통해서 구별될 수 있다.

유효 자료가 많은 명령문을 위해서 입력 자료의 연속(continuation)성이 보장되도록 설계하였다. 자료를 연속해서 정의하는 경우는 명령문의 마지막에 쉼표와 연속기호 ‘+’를 첨가하게 되면 다음 라인의 자료들도 같은 명령문에 속하게 된다. 즉, 각 줄의 마지막에 ‘+’ 기호를 첨부함으로써 연속기호 ‘+’가 나오지 않을 때까지는 한 줄로 생각할 수 있게 된다. 이 연속 또한 수에 제한 없이 가능하다. 하지만 프로그램 내에서 입력 파일을 읽어 들이는 속도를 향상시키기 위해서 다음과 같은 조건들을 부과하였다. 각 명령문에서 한 자료(쉼표와 쉼표 사이의 자료 값을 의미)의 크기는 문자열 기준으로 30자를 넘을 수 없고, 한 라인은 300 자를 넘을 수 없다. 그리고 연속을 통해서 묶이는 자료의 크기는 1000 자로 제한하였다.

각 명령문의 시작은 ‘*’ 기호를 통해서 알리게 된다. 파일의 끝은 ‘#END’로 규정한다. 각 명령문들의 순서는 특별히 없으며, 중복 선언될 수 있다. 각 명령문에 대한 세부적인 설명은 명령문 설명 부분을 참조하도록 하자. 입력파일의 입력문 특징을 요약하면 아래와 같다.

- 각 자료값들에 제약이 없는 Free Field 형식이다. 
- 쉼표를 통해서 각 자료값들 사이에 구별이 된다. 
- 명령어는 항상 ‘*’ 기호와 같이 온다.
- 명령어들 사이에 특별한 순서는 없다.
- 명령문의 마지막에 ‘+’ 기호를 첨가하여 한 명령문의 연속이 가능하다.
- 입력 파일의 마지막은 ‘#END’로 표시한다.
- 일반적인 명령어들은 문자열 기준 최대 1000 자까지 입력이 가능하다. 
- 한 자료값은 문자열 기준 30자를 넘길 수 없다.
- 입력 파일의 한 줄은 300자를 넘길 수 없다. 연속기호를 활용한다.

![Aspect ratio](/media/POST/000056/10.jpg)

###3.1. CONTROL PARAMETER
해석 종류와 변위, 응력, 반력, 내부힘의 출력과 관계된 제어 변수들을 입력받는다. 하위 명령어들에 원하는 자료를 입력하게 되면 해석의 제어가 가능하게 된다. 이 명
령어에 한해서는 연속이 허용되지 않는다. 그리고 필수적인 값은 아니므로 반드시 입력 문에 쓸 필요는 없다. 출력 파일은 .out, .pch, .op2 파일을 의미한다.

![Aspect ratio](/media/POST/000056/11.jpg)

**STRESS POINT 의 경우는 각 요소에 따라서 지원이 가능한 응력 계산점의 위치가 다르다. 명령어 ELEMENT 의 요소 특성들을 참조하여 적절한 조건을 주어야 한다.

####1. 사용규칙
![Aspect ratio](/media/POST/000056/12.jpg)

####2. 사용 예
선형 정적 해석을 수행하며, 변위값, 응력값, 반력값들을 출력 파일에 써주고, 응력값은 요소의 적분점에서 구하는 경우의 입력문은 다음 그림 2과 같다.
![Aspect ratio](/media/POST/000056/13.jpg)

###3.2. ELEMENT
요소에 필요한 자료 값들을 표시한다. 모든 요소에서 요소 번호, 요소 종류(문자열 상수), 단면 특성 번호, 재료 특성 번호, 절점 리스트의 순서로 자료 값들이 오게 된다. 본 프로그램에서 지원하는 요소의 종류와 요소의 특성은 아래 표와 같다.

![Aspect ratio](/media/POST/000056/14.jpg)

####1. 사용규칙
![Aspect ratio](/media/POST/000056/15.jpg)

![Aspect ratio](/media/POST/000056/16.jpg)

![Aspect ratio](/media/POST/000056/17.jpg)

![Aspect ratio](/media/POST/000056/18.jpg)

####2. 사용 예

2.1) 예시 1

1 번 단면특성, 1 번 재료특성을 참조하고, 요소 orientation 벡터가 z 축인 빔 요소에 대한 입력문은 그림 3.3과 같다.

![Aspect ratio](/media/POST/000056/19.jpg)

2.2) 예시 2

1번 단면특성, 1번 재료특성을 참조하는 트러스 요소에 대한 입력문은 그림 3.4와 같다.

![Aspect ratio](/media/POST/000056/20.jpg)

2.3) 예시 3

1번 절점의 3번 자유도(z축 변위)와 2번 절점의 3번 자유도를 스프링 강성 5000을 갖는 스프링 요소에 대한 입력문은 다음과 같다.

![Aspect ratio](/media/POST/000056/21.jpg)

###3.3. NODE
절점의 위치 정보를 나타낸다. 현재는 local 좌표계를 사용하지 않기 때문에 참조 좌표계의 의미가 없지만, 기본적으로 NODE 명령에서는 절점의 위치 정보뿐만 아니라 참조 좌표계 번호도 나타내준다.

####1. 사용규칙
![Aspect ratio](/media/POST/000056/22.jpg)

####2. 사용 예
![Aspect ratio](/media/POST/000056/23.jpg)

![Aspect ratio](/media/POST/000056/24.jpg)

###3.4. MATERIAL
각 재표 특성들의 자료 값을 나타낸다. 입력 자료들은 재료 특성 번호, 재료 특성 종류, 참조 좌표계, 재료 특성 값들이다. 본 프로그램에서 사용되는 재료 특성의 종류는 선형 등방성 재료(LISO)와 다양한 재료 특성을 제공하고 있지만, 현재 프로그램 안에 선형 등방성 재료 이외의 재료를 사용하는 함수가 없기 때문에 여기서는 선형 등방성 재료만 사용하도록 한다. 그리고 선형 등방성 재료에서도 실제로 사용하는 자료 값 필드가 몇 개 되지 않기 때문에 사용하지 않는 부분에 대해서는 임의의 값이나 공란으로 남겨두어 도 된다. 참조 좌표계 역시 사용하지 않으므로 임의의 값이어도 무방하다.

####1. 사용규칙
![Aspect ratio](/media/POST/000056/25.jpg)

![Aspect ratio](/media/POST/000056/26.jpg)

####2. 사용 예
영계수 E =10000000, 포아송 비 v = 0.3, 밀도 ρ =1000의 특성을 가지는 선형 등방성 재료 특성을 정의하는 경우
![Aspect ratio](/media/POST/000056/27.jpg)

###3.5. PHYSICAL
단면 특성의 자료 값들을 정의한다. 입력 자료들은 단면 특성 번호, 단면 특성 종류, 단면 특성 값들이다. 단면 특성 값들은 각 단면 특성들에 따라서 달라지게 된다.

####1. 사용규칙
![Aspect ratio](/media/POST/000056/28.jpg)

![Aspect ratio](/media/POST/000056/29.jpg)

![Aspect ratio](/media/POST/000056/30.jpg)
※ 2 번 절점방향의 특성값이 1 번 절점방향의 특성값과 같을 경우, PVL(10~18)은 입력하지 않 아도 무방하다.

####2. 사용 예
![Aspect ratio](/media/POST/000056/31.jpg)

###3.6. BOUNDARY
변위 경계 조건 중에서 단절점 경계 조건(SPC)을 정의한다. 입력 자료 값들은 경계 조건 Set 번호, ‘SPC’(단절점 경계 조건을 나타내는 문자열), 참조 좌표계, 적용 절점 번호, 적용 자유도, 변위값이다. SPC 경계 조건은 입력문 초기에 주어진 경계 조건 적용 번호와 경계 조건 Set 번호가 일치하는 것들만 실제 해석에 사용된다. 두 번째 필드의 ‘SPC’는 고 정된 값이다. 참조 좌표계의 경우 사용하지 않으므로 임의의 값이어도 무방하다. 적용 절 점의 경우 경계 조건이 적용되는 절점 번호이고, 적용 자유도의 경우 적용 절점에서 구속되는 자유도를 의미한다. 적용 자유도의 값들은 6 자유도를 ‘1’,’2’,’3’,’4’,’5’,’6’으로 분배 하여 구속되는 절점의 자유도 번호를 연이어 적어주는 방식이다. 예를 들어 x 방향의 병 진 변위와 y 방향과 z 방향의 회전 변위를 구속하게 되면 적용 자유도의 필드 값은 ‘156’ 이 된다. 마지막으로 변위 값은 절점의 변위 크기를 ‘0.’으로 고정할 것인지, 특정한 값을 줄 것인지를 결정하게 된다. ‘0.’이외의 값이 주어지면 Prescribed boundary condition 이 된다.

####1. 사용규칙
![Aspect ratio](/media/POST/000056/32.jpg)

![Aspect ratio](/media/POST/000056/33.jpg)

####2. 사용 예

2.1) 예시 1

1 번 절점의 y 방향으로의 움직임이 제한되었고, 2 번 절점의 모든 방향으로의 움직임이 제한되었다.

![Aspect ratio](/media/POST/000056/34.jpg)

![Aspect ratio](/media/POST/000056/35.jpg)

2.2) 예시 2

1 번 절점의 y 방향으로의 움직임이 제한되고, x,y,z 축을 중심으로 한 회전이 제한되 었다.

![Aspect ratio](/media/POST/000056/36.jpg)

2.3) 예시 3

1번 절점이 z 방향으로 강제 변위가 1.0으로 주어졌다.

![Aspect ratio](/media/POST/000056/37.jpg)

###3.7. LOAD
각종 하중의 자료 값들을 정의한다. 입력 자료 값들은 하중 Set 번호, 하중 종류, 참조 좌표계, 적용 절점(요소), 스케일 값, 적용 영역, 하중벡터 등이다. 하중의 종류에 따라서 입력 자료 값들이 약간씩 달라진다. 경계 조건과 마찬가지로 하중 Set 번호와 입력문 의 초기에 오는 Control Parameter 에서의 값이 일치해야 하중 조건을 적용하게 된다. 프 로그램 안에서 처리할 수 있는 하중 명령어들로는 FORCE, MOMENT, BEAMDLOAD, BEAMPLOAD 등이 있다.

![Aspect ratio](/media/POST/000056/38.jpg)

####1. 사용규칙
![Aspect ratio](/media/POST/000056/39.jpg)

![Aspect ratio](/media/POST/000056/40.jpg)

![Aspect ratio](/media/POST/000056/41.jpg)

![Aspect ratio](/media/POST/000056/42.jpg)

![Aspect ratio](/media/POST/000056/43.jpg)

![Aspect ratio](/media/POST/000056/44.jpg)

####2. 사용 예
2.1) 예시 1

![Aspect ratio](/media/POST/000056/45.jpg)

2.2) 예시 1

![Aspect ratio](/media/POST/000056/46.jpg)

2.3) 예시 3

![Aspect ratio](/media/POST/000056/47.jpg)

###3.8. RELEASE
끝단부 해제(END RELEASE)는 빔 요소를 이용하여 해석 도중 요소 끝단의 일부 자유도를 없애고자 할 때 이용한다. 빔 요소의 끝단의 회전을 풀어 핀 조인트처럼 연결을 하고 싶거나 인접 물체와 슬라이딩하는 경계조건을 갖는 등의 경우 사용하게 된다.

####1. 사용규칙
![Aspect ratio](/media/POST/000056/48.jpg)

![Aspect ratio](/media/POST/000056/49.jpg)

####2. 사용 예
그림 3.16과 같이 1,2,3번 요소가 위치해 있고,n2 축이 모두 지면을 뚫고 들어가는 방향으로 정의한다. 이와 같은 경우 n2 축의 회전 자유도를 풀어주는 입력문 형식은 다음과 같다.

![Aspect ratio](/media/POST/000056/50.jpg)

##4. 종합예제
###4.1. 집중하중을 받는 외팔보의 해석
* 본 예제의 입력문 파일명은 BeamTruss_tutorial.csd 이다.

####4.1.1. 문제의 정의
본 예제는 빔 요소를 이용해 외팔보를 해석해 보고 결과를 확인함으로써 프로그램의 간단한 활용법을 익힌다.

외팔보는 한쪽 끝단이 고정되어 모든 자유도가 구속되고 반대쪽 끝단은 어떠한 자유 도도 구속되지 않는 형태의 부재를 의미한다. 보는 사각형의 단면을 가지고 있고, 자유단 끝에 2kN의 힘이 가해지는 경우를 가정한다.

![Aspect ratio](/media/POST/000056/51.jpg)

3 장에서 설명한대로 입력문을 구성하여 다음 페이지에 첨부하였다.

####4.1.2. 문제 입력문
![Aspect ratio](/media/POST/000056/52.jpg)
![Aspect ratio](/media/POST/000056/53.jpg)

입력문의 형식을 구분기호에 따라 설명하면 다음과 같다.

![Aspect ratio](/media/POST/000056/54.jpg)

![Aspect ratio](/media/POST/000056/55.jpg)

![Aspect ratio](/media/POST/000056/56.jpg)

![Aspect ratio](/media/POST/000056/57.jpg)

![Aspect ratio](/media/POST/000056/58.jpg)

![Aspect ratio](/media/POST/000056/59.jpg)

####4.1.3. 유한요소 해석 결과
요소의 변형 형상과 응력 해석 결과는 다음 그림과 같다

![Aspect ratio](/media/POST/000056/60.jpg)

![Aspect ratio](/media/POST/000056/61.jpg)
