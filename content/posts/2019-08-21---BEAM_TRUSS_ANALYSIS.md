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
-\Bigg(\int\limits_V\begin{Bmatrix}U\end{Bmatrix}^T[N]^T\begin{Bmatrix}b\end{Bmatrix}+\int\limits_S \begin{Bmatrix}U\end{Bmatrix}^T[\overline{N}]^T\begin{Bmatrix}\overline{t}\end{Bmatrix}dS \Bigg)
$$