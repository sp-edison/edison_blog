---
title: "2D_CONTINUUM_ANALYSIS"
date: "2019-08-06T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/2D_CONTINUUM_ANALYSIS"
category: "Computational Structural Dynamics"
tags: 
 - "field equation"
 - "equilibrium equation"
description: "2D Continuum linear analysis program Manual"
---
##연속체 2D 요소 구조 해석 프로그램 매뉴얼

##목차
###1. 유한요소법
1.1 유한요소법의 개념

1.2 탄성체 평형 방정식

1.3 변분 방정식

###2. 2차원 연속체 요소 설명
####2.1 형상 함수
2.1.1 등매개 근사 

2.1.2 삼각형 요소 

2.1.3 사각형 요소

####2.2강성행렬
2.2.1 재료 물성 행렬 [D]

2.2.2 형상 함수의 미분 행렬 [B]

2.2.3 강성행렬 [K]

2.2.4 가우스 적분법

###3. 입력문 형식 세부 설명
####3.1 입력문 예제
3.1.1 CONTROL PARAMETER 

3.1.2 ELEMENT

3.1.3 NODE

3.1.4 BOUNDARY

3.1.5 LOAD

3.1.6 MATERIAL

3.1.7 PHYSICAL

###4. 튜토리얼
####4.1 2개의 구멍이 있는 2차원 브라켓

------------------------------------------------------------------------------------------------------------------

##1. 유한요소법
###1.1 유한요소법의 개념
유한요소법은 탄성체문제 또는 장(field equation)문제를 풀기위한 기법이다. 외팔보에 힘이 가해졌을 경우 이론적인 변위와 응력은 수계산을 통해 직접 구할 수 있다. 그러나 복잡한 형상을 가지는 모델에 대한 해석이 필요할 경우, 수계산으로 해를 구하기 어렵다. 이를 해결하기 위하여 복잡한 형상의 모델을 삼각형, 사각형, 사면체 또는 육면체로 구성된 작은 요소(element)들이 모여 이루고 있음을 가정하면, 모델의 응력, 변위를 구할 수 있는 행렬 방정식을 구성할 수 있다. 이 때, 유한요소법을 이용하여 구성한 행렬 방정식은 실제 모델을 유한요소로 이산화한 것이기 때문에 정해가 아닌 근사해 (approximate solution)를 얻게 된다. 하지만 요소의 크기가 작을수록, 요소의 성능이 좋을수록 유한요소법을 이용한 해석결과는 정해에 매우 가까워진다. 다음 그림과 같은 문제가 있을 경우, 문제의 영역은 2 개의 유한 요소로 이산화되었고 이산화된 요소들은 요소를 구성하는 절점(node)으로 구성되어있다. 모든 요소에서 변위와 하중의 관계를 나타내주는 강성행렬을 구성한 후, 이를 하나의 행렬로 조립하면 해석할 모델의 이산화된 행렬 방정식을 만들 수 있다.

![Aspect ratio](/media/POST/000021/0.jpg)


이산화된 요소는 매우 간단하므로 요소의 평형방정식과 요소의 강성행렬등은 표준 화된 요소 함수에 의해 쉽게 계산된다. 행렬의 조립과 비슷한 방법으로 외력힘 벡터$\left\{\overline{\mathrm{F}}_{b}\right\}$도 조립된다.조립된 시스템 방정식에서 SPC(단일 절점) 변위 경계조건$\left\{\overline{\mathrm{U}}_{a}\right\}$을 부여한다. 그리고 미지의 변위$\left\{U_{b}\right\}$에 관해 가우스 소거법을 이용하여 행렬식을 푼다. 기본 해인 미지 변위와 SPC 값을 이용하여 미지 반력$\left\{\overline{\mathrm{F}}_{a}\right\}$및 요소내 응력$\{\sigma\}$을 구한다. 이상이 탄성체 문제에서의 개략적인 유한요소법의 절차이다.

모델을 정의하는 것은 해석하고자 하는 대상의 기하학적 자료를 유한요소로 표현하 고 모델에 작용하는 하중 및 변위 제한조건을 부여하는 과정을 의미한다.

주어진 문제는 각 부분의 물리적인 거동 특성을 고려하여 Truss, Beam, Solid, Rigid Bar, Spring 등으로 모델링을 해야 한다. 동일한 대상에 대하여 다른 형식의 요소로 모델 링 할 수 있는데 이는 부재의 거동, 설계 관심부분, 해석의 정확도 등에 따라 사용자의 지식, 경험 등에 의존하여 결정된다. 일단 해석 대상이 지원되는 요소로 모델링이 되었 다면 요소의 기하학적 정의는 요소를 구성하는 절점 및 요소의 단면계수에 의해 이루어 진다.

본 장에서는 구조해석의 기본이 되는 평형방정식과 현실의 해석 대상을 가상의 유한 요소 모델로 수식화 하는 방법을 소개한다.
 
###1.2 탄성체의 평형방정식
아래 그림에서 탄성체는 영역(domain: $V$ )과 경계(boundary: $S$ )로 구분하여 나타내었 다. 경계는 변형을 구속하고 있는 변위경계($S_{u}$)와 표면력(surface force, $\overline{\mathrm{t}}$ )이 작용하는 하중경계($S_{t}$)로 나누어 지고 이 둘은 서로 중복되어 정의될 수 없다. 영역의 전체 혹은 일부에서는 중력에 의한 자중이나 관성력, 전자기력 등과 같은 체적력(body force, $\mathbf{b}$)이 작용할 수 있고 이러한 하중들에 의해 응력이 분포되므로 탄성변형을 일으킨다. 흔히 표 면력과 체적력을 하중조건(loading condition)이라 칭하고 변위경계조건상의 값을 간단히 경계조건(boundary condition)이라 한다.

![Aspect ratio](/media/POST/000021/1.jpg)


탄성체의 정적 평형상태는 영역내의 응력의 분포로 표시 할 수 있고 임의의 위치에 서의 미소체적에 대한 뉴턴의 힘의 평형방정식(equilibrium equation)을 유도한 것은 소변 형, 소변위 영역에서 다음과 같이 표현된다.

$$
\frac{\partial \sigma_{x x}}{\partial x}+\frac{\partial \tau_{x y}}{\partial y}+\frac{\partial \tau_{x z}}{\partial z}+b_{x}=0
$$

$$
\frac{\partial \tau_{y x}}{\partial x}+\frac{\partial \sigma_{y y}}{\partial y}+\frac{\partial \tau_{y z}}{\partial z}+b_{y}=0 \quad \text { in } \quad V
$$

$$
\frac{\partial \tau_{z x}}{\partial x}+\frac{\partial \tau_{z y}}{\partial y}+\frac{\partial \sigma_{z z}}{\partial z}+b_{z}=0
$$

여기서 x, y, z는 3 차원 직교좌표계의 좌표 값을 나타내고, $b_{x}, b_{y}, b_{z}$ 는 단위영역 당 작용하는 체적력 벡터$\{\mathrm{b}\}^{T}=\left\{b_{x}, b_{y}, b_{z}\right\}$의 직교 좌표 성분 값이다. 그리고 미지수인 응력은 9 개의 텐서 성분으로 표시되며 2 차텐서의 정의로부터 다음과 같다.

$$
[\sigma]=\left[\begin{array}{ccc}{\sigma_{x x}} & {\tau_{x y}} & {\tau_{x z}} \\ {\tau_{y x}} & {\sigma_{y y}} & {\tau_{y z}} \\ {\tau_{z x}} & {\tau_{z y}} & {\sigma_{z z}}\end{array}\right]
$$

$$
\tau_{y x}=\tau_{x y} ; \quad \tau_{z x}=\tau_{x z} ; \quad \tau_{z y}=\tau_{y z}
$$

따라서 대칭성을 고려하면 응력성분 표시는 6 개로 가능하며 프로그램 작성시 일차원 배열을 사용하기 위해 흔히 $\{\sigma\}^{T}=\left\{\sigma_{x x}, \sigma_{y y}, \sigma_{z z}, \tau_{x y}, \tau_{y z}, \tau_{x z}\right\}$와 같이 표기한다. 여기서 $\sigma$는 수직응력(normal stress)을 표시하는데 쓰였고 $\tau$는 전단응력(shear stress)을 나타낸다.

한편 경계에서는 영역의 응력값의 좌표변환에 의한 경계면의 벡터성분과 표면력의 성분이 동일해야 한다는 연속조건으로부터 다음과 같이 표현된다.

$$
\sigma_{x x} \cdot n_{x}+\tau_{x y} \cdot n_{y}+\tau_{x z} \cdot n_{z}=\overline{t}_{x}
$$

$$
\tau_{x y} \cdot n_{x}+\sigma_{y y} \cdot n_{y}+\tau_{y z} \cdot n_{z}=\overline{t}_{y} \quad \text { on } \quad S_{t}
$$

$$
\tau_{x z} \cdot n_{x}+\tau_{y z} \cdot n_{y}+\sigma_{z z} \cdot n_{z}=\overline{t}_{z}
$$


여기서 $\{\mathrm{n}\}^{T}=\left\{n_{x}, n_{y}, n_{z}\right\}$는 경계면의 법선 단위벡터이며 $\{\overline{t}\}^{T}=\left\{\overline{t}_{x}, \overline{t}_{y}, \overline{t}_{z}\right\}$는 표면 트랙션(traction)이라고 하는 단위표면적당 표면력을 의미한다. 위 식의 좌변들은 응력 텐서를 표면에 투영시킨 것이고 우변은 단위면적당 표면에 작용하는 힘으로써 주어진 성분값이다. 위 식들은 6개의 응력성분의 미지수를 가진 6개의 식이고 이를 풀이할 수 있 다면 응력의 분포를 얻게 된다.

만약 문제가 변위경계($S_{u}$)를 포함하고 있다면 다음의 경계조건을 만족해야 한다.

$$
u=\overline{u}
$$

$$
v=\overline{v} \quad \text { on } \quad S_{u}
$$

$$
w=\overline{w}
$$

여기서 $\{\mathrm{u}\}^{T}=\{u, v, w\}$는 탄성체의 임의의 점에서 x, y, z 축방향 성분으로 구성된 변위벡터로서 위 식은 $S_{u}$에서 변위가 주어졌음을 의미한다. 따라서 변위와 응력간의 관계식을 추가해야 한다.

변형률 정의식을 적용함으로써 응력-변위의 관계를 얻을 수 있다. 탄성론에 의하면 변형률은 변위로부터 구할 수 있다. 변형률도 응력과 마찬가지로 2 차텐서로서 다음과 같이 표시한다.

$$
[\varepsilon]=\left[\begin{array}{lll}{\varepsilon_{x x}} & {\varepsilon_{x y}} & {\varepsilon_{x z}} \\ {\varepsilon_{x y}} & {\varepsilon_{y y}} & {\varepsilon_{y z}} \\ {\varepsilon_{x z}} & {\varepsilon_{y z}} & {\varepsilon_{z z}}\end{array}\right]
$$

$$
\varepsilon_{x y}=\frac{1}{2} \gamma_{x y} ; \quad \varepsilon_{y z}=\frac{1}{2} \gamma_{y z} ; \quad \varepsilon_{x z}=\frac{1}{2} \gamma_{x z}
$$


$\{\varepsilon\}^{T}=\left\{\varepsilon_{x x}, \varepsilon_{y y}, \varepsilon_{z z}, \gamma_{x y}, \gamma_{j s}, \gamma_{x z}\right\}$와 같이 표기한다. 그러면 변형률과 응력의 관계식은 후크의 법칙(Hooke’s law)을 삼차원으로 확장하는 방식으로 다음과 같이 배열(array)-행렬 (matrix)로 된 식을 얻을 수 있다.

$$
\{\sigma\}=[\mathbf{D}]\{\varepsilon\}
$$

여기서 [D]행렬은 탄성체 문제의 차원(dimension)에 따라 다르게 구해지는 구성행렬(constitutive matrix)로 다음에 나오는 절에서 상세히 다룰 것이다. 변형률은 변위로부터 계산 하므로 결국은 응력은 변위로써 표현 된다. 따라서 평형방정식은 3 개의 변위성분을 미지수로 갖는 고차 미분방정식으로 치환이 가능하고 이를 풀어 변위를 구할 수 있다. 그러나 여기에서는 변위에 대한 고차미분항이 들어 있으므로 응력에 관해 푸는 것보다 한층 더 높은 난이도를 요구할 것이다.

###1.3 변분 방정식
모든 자연현상은 열역학 제1법칙을 만족한다. 이는 에너지의 보존 및 변환의 과정을 지배하는 법칙으로서 탄성체의 거동 또한 에너지와 일의 개념으로 지배방정식을 유도할 수 있다. 역학적 평형상태란 에너지가 변동이 없는 고정된(stationary) 상태를 의미한다. 중력작용하에서의 평형상태를 나타내는 아래 그림에서 고정된 포텐셜에너지(potential energy)의 상태에는 불안정한 상태와 안정된 상태의 두 가지가 있음을 보여주고 있다. 이 두 가지 중에서 안정된 상태는 에너지가 최소값을 가질 때를 말하고 불안정한 상태에서 는 최소값이 아님을 알 수 있다. 즉 자연현상에서 안정된 정적평형이라고 하는 것은 반 드시 에너지가 최소가 되어야만 하는 것이다.

![Aspect ratio](/media/POST/000021/2.jpg)

변분법(variational method)이란 원래 편미분 방정식을 적분형식으로 바꾸어 푸는 데 있어서 포텐셜에너지와 유사한 일종의 가상 에너지를 - 이것을 수학에서는 범함수 (functional)라고 한다 - 도입하고 이에 대한 정류점(stationary point)을 찾기 위해 편미분 혹은 변분(variation)이 영(zero)라는 조건을 구하는 방법을 말한다. 여기서 수학적 범함수 에는 여러 가지 종류가 있을 수 있으나 지배방정식의 유형과 원하는 해의 종류에 따라 다르게 적용한다. 예를 들어 유체의 유동이나 열전달 문제 등의 장문제(field problem)에 서는 탄성체 문제의 포텐셜에너지와 같은 개념을 사용할 수 없다. 따라서 해당 문제의 지배방정식으로부터 변분법에 의한 유한요소 정식화가 이루어질 것이다. 탄성체 문제에 서는 포텐셜에너지 방법이 열역학적 법칙과 합치되므로 굳이 변분법을 사용할 필요는 없으나, 사실상 변분법을 이용하여 평형방정식으로부터 앞으로 설명될 포텐셜에너지 식이 유도됨이 증명된다.

지금 외력을 받아 변형되어 평형을 이루고 있는 탄성체를 하나의 계(system) 로 생 각하면, 계의 포텐셜에너지 Π 는 탄성체의 변형률에너지 Λ 에서 외력에 의한 포텐셜 W를 뺀 값이다.

$$
\Pi=\Lambda-W
$$

에너지를 어떤 변수들의 함수로 표현한다면 정적평형상태를 구하기 위해서는 위 식을 최소로 하는 변수를 찾으면 된다. 즉 그 변수를 유한요소의 변위(displacement)
$\{\mathrm{U}\}^{T}=\left\{U_{1}, U_{2}, U_{3}, \Lambda \Lambda\right\}$로 택한다면 다음 식으로부터 평형상태의 변위를 구할 수 있다.

$$
\frac{\partial \Pi}{\partial U_{1}}=0 ; \quad \frac{\partial \Pi}{\partial U_{2}}=0 ; \quad \frac{\partial \Pi}{\partial U_{3}}=0 ; \Lambda \Lambda
$$

변형률 에너지는 탄성론의 정의에 의해 다음과 같은 영역적분으로부터 구해진다.

$$
A=\int_{V} \frac{1}{2}\{\varepsilon\}^{T}\{\sigma\} d V
$$
    
여기서 적분기호 안의 값은 단위체적당 변형률 에너지이다. 응력값은 변형률 항들을 이용하여 쓸 수 있다. 그리고 변형률은 변위의 미분식으로부터 구할 수 있고 영역변위 {u}는 유한요소변위 혹은 절점변위(nodal displacement) {U}로 부터 보간(interpolation) 법에 의해 다음과 같이 구해진다.

$$
\{\mathrm{u}\}=[\mathbf{N}]\{\mathrm{U}\}
$$

[N]은 보간행렬(형상함수로 구성)를 의미한다. 즉, 변형률은 절점변위를 이용하여 다음과 같은 행렬식으로 표현된다.

$$
\{\varepsilon\}=[\mathbf{B}]\{\mathbf{U}\}
$$


여기서 [B] 행렬은 구배행렬(gradient matrix) 이라고 하고, 이는 보간행렬 [N] 을 변 형률 연산자에 맞게 구함(편미분)을 의미한다. 이러한 행렬들의 계산절차는 일차원, 이차 원, 삼차원에 맞게 다음에 나오는 절에서 상세히 언급될 것이다.

따라서 변형률에너지가 유한요소변위를 변수로 하는 형태로 표현될 수 있다.

$$
\Lambda=\frac{1}{2} \int_{V}\{\varepsilon\}^{T}[\mathbf{D}]\{\varepsilon\} d V=\frac{1}{2} \int_{V}\{\mathrm{U}\}^{T}\left([\mathbf{B}]^{T}[\mathbf{D}][\mathbf{B}]\right)\{\mathrm{U}\} d V
$$

외력에 의해 행해진 일은 체적력 {b}의 경우 영역적분에 의해서, 표면력 $\overline{\mathrm{t}}$의 경우 경계적분에 의해서 다음과 같이 구해진다.

$$
W=\int_{V}\{\mathrm{u}\}^{T}\{\mathbf{b}\} d V+\int_{S}\{\mathrm{u}\}^{T}\{\overline{\mathrm{t}}\} d S
$$

절점변위를 이용하면 하중을 절점 변위의 형태로 나타낼 수 있다.

$$
W=\int_{V}\{\mathrm{U}\}^{T}[\mathrm{N}]^{T}\{\mathrm{b}\} d V+\int_{S}\{\mathrm{U}\}^{T}[\overline{\mathrm{N}}]^{T}\{\mathrm{t}\} d S
$$

실제 적분을 수행함에 있어서 위 의 우변의 두번째 항은 St 경계에서의 적분이므로 영역보간에서 쓰인 [$\mathbf{N}$]행렬을 경계좌표계로 변환하여([$\overline{\mathbf{N}}$]) 수행된다.

이제 유한요소변위의 함수로 된 식들을 대입하고 포텐셜에너지의 최소화 방정식에 적용하면 다음과 같은 방정식을 얻게 된다.
![Aspect ratio](/media/POST/000021/3.jpg)

따라서 위 식은 정적 평형상태의 변수인 유한요소변위 {U}를 풀 수 있는 대수 (algebraic) 연립방정식이 된다. 여기서 [N] , [B] , [D] 행렬들을 포함한 적분을 수행하면 상수값들이 된다.

이 식을 다시 정리하면 다음과 같은 유한요소 평형방정식이 얻어진다.

$$
[\mathbf{K}]\{\mathrm{U}\}=\{\mathrm{F}\}
$$

여기서 좌변의 행렬 $\mathbf{K}$ 를 강성행렬(stiffness matrix), 우변의 배열 F 를 외부 외력벡터 (external load vector)라 하고 다음과 같다.

$$
[\mathbf{K}]=\int_{V}[\mathbf{B}]^{T}[\mathbf{D}][\mathbf{B}] d V
$$

$$
\{\mathrm{F}\}=\int_{V}[\mathbf{N}]^{T}\{\mathbf{b}\} d V+\int_{S}[\overline{\mathbf{N}}]^{T}\{\overline{t}\} d S
$$

만약 절점변위 {U}의 미지수가 n개 라면 벡터 {F}의 차원도 n개이고 행렬 [$\mathbf{K}$] 의 차원은 ($n \times n$) 의 정방행렬(square matrix)이 되어 역행렬(inverse matrix)방법이나 가 우스 소거법(Gaussian elimination) 등에 의해{U}를 구할 수 있다.

실제로 강성행렬과 절점 외력벡터를 구할 때는 적분의 영역을 이산화된 하나의 요소 에 국한하여 쉽게 구하고 모든 요소방정식을 시스템방정식에 조립(assembling)하는 방법 을 쓰게 된다. 즉,
$$
[\mathbf{K}]=\sum_{e=1}^{n d} \int_{V^{*}}\left[\mathbf{B}^{e}\right][\mathbf{D}]\left[\mathbf{B}^{e}\right] d V^{e}=\sum_{e=1}^{n e l}\left[\mathbf{K}^{e}\right]
$$

$$
\{\mathrm{F}\}=\sum_{e=1}^{n e l} \int_{V^{e}}\left[\mathbf{N}^{e}\right]^{T}\left\{\mathbf{b}^{e}\right\} d V^{e}+\int_{S^{e}}\left[\overline{\mathbf{N}}^{e}\right]^{T}\left\{\overline{\mathbf{t}}^{e}\right\} d S^{e}=\sum_{e=1}^{n e l}\left\{\mathrm{F}^{e}\right\}
$$

여기서 e 는 각각의 요소를, nel 은 요소의 총 수를 의미하고 행렬들은 요소내부에서 정의되었다.

##2. 2차원연속체요소
###2.1 형상 함수
####2.1.1 등매개 근사 요소 (isoparametric element)
2 차원 연속체 유한요소법은 임의의 형상을 갖는 2 차원 연속체를 작은 요소단위로 나누어 유한요소해석을 진행하게 된다. 이 과정에서 임의의 형상을 갖는 해석 모델을 정
사각형 또는 정삼각형 꼴의 정규화 된 형상만으로 구성하는 것은 무리가 있다. 그러므로 정규적이지 않은 삼각형 또는 사각형을 이용하여 유한요소 해석을 진행한다. 이 때 임의
의 형상을 갖는 요소에 대해 형상함수를 구성할 경우, 모든 요소에 대한 형상함수의 형태가 다르게 구성되며 수렴에 필수적인 연속성을 만족시키기 어렵게 된다. 유한요소해석
을 진행할 때 각기 다른 형상함수들을 이용할 경우 강성행렬을 계산하는 것이 어려워진다. 강성행렬을 계산할 때 형상함수의 미분값을 적분하기 때문이다. 따라서 2차원 연속체 유한요소 해석을 진행할 때 사용하는 요소는 natural coordinate 을 이용하여 임의의 삼각형, 사각형을 직각삼각형과 정사각형으로 매핑하게 된다. 이를 통하여 절점의 변위근사 뿐만 아니라 해당 요소의 좌표도 근사하게 된다.

![Aspect ratio](/media/POST/000021/4.jpg)


####2.1.2 삼각형 요소
삼각형 요소의 형상함수는 요소의 면적을 함수로 이용한다. 삼각형 요소의 형상함수 를 각각 $N_{1}, N_{2}, N_{3}$라 하자. 형상함수는 보간함수이므로 요소 내 어느 점에서나 형상 함수의 합이 1이 되어야 한다.

$$
N_{1}+N_{2}+N_{3}=1
$$

또한 등매개 근사를 위해 좌표 x와 y는 형상함수에 의해 근사된다.

$$
x=N_{1} x_{1}+N_{2} x_{2}+N_{3} x_{3}
$$

$$
y=N_{1} y_{1}+N_{2} y_{2}+N_{3} y_{3}
$$

임의의 형상에 대하여 위 식을 풀면 $N_{1}, N_{2}, N_{3}$를구할수있다.

$$
N_{i}=\frac{a_{i}+b_{i} x+c_{i} y}{2 \times \text {areal} 23}, i=1,2,3
$$

$$
\text { where, } a_{i}=x_{j} y_{k}-x_{k} y_{j}, \quad b_{i}=b_{j}-b_{k}, \quad c_{i}=c_{k}-c_{j}
$$

$$
(i, j, k)=(1,2,3),(2,3,1),(3,1,2)
$$

area123은 해당 요소의 면적을 의미한다. 이것을 정리하면 형상함수를 보다 더 간단하게 나타낼 수 있다.

$$
N_{i}=\frac{\operatorname{area} P 23}{\operatorname{area} 123}, i=1,2,3
$$

P는 요소 내 임의의 위치에 존재하는 점이다.

![Aspect ratio](/media/POST/000021/5.jpg)

임의의 요소에서 형상함수는 전체 면적대비 해당 절점을 마주보는 변을 포함하는 삼 각형의 비율을 나타낸다. 형상함수를 정규화 하기 위하여 natural coordinate 의 직각 삼 각형으로 위의 요소를 매핑할 수 있다.

![Aspect ratio](/media/POST/000021/6.jpg)

위의 요소로 매핑할 경우, 형상함수는 다음과 같아진다.

$$
N_{1}(\xi, \eta)=1-\xi-\eta, \quad N_{2}(\xi, \eta)=\xi, N_{3}(\xi, \eta)=\eta
$$

이 형상함수를 이용하면 변위와 절점을 같은 보간함수를 이용하여 근사할 수 있게 된다.

####2.1.3 사각형 요소
Reference coordinate 에 있는 한 변의 길이가 2 이고 중심점이 원점에 위치하는 정 사각형을 이용하여 형상함수를 구성할 수 있다.

![Aspect ratio](/media/POST/000021/7.jpg)

이 때 형상함수의 꼴은 다음과 같이 규칙적으로 나타난다.

$$
N_{i}=\frac{1}{4}\left(1+\xi_{i} \xi\right)\left(1+\eta_{i} \eta\right), \quad i=1,2,3,4
$$

($\xi_{i}, \eta_{i}$) 는 해당 절점이 위치하는 Reference coordinate 상의 좌표로 1 또는 -1 의 값 을 갖게 된다. 여기서 만든 형상함수는 다 합하였을 때 1, 각 절점에서 해당하는 형상함 수만 1 의 값을 갖게 된다.
  
###2.2 강성 행렬
####2.2.1 재료 물성 행렬 [D]
이차원 문제에는 평면응력 문제, 평면변형률 문제가 있다. 이들은 모두 유사한 요소 보간함수로 이산화 과정을 거치고 변위의 자유도가 평면상에서만 정의되기 때문에 이차 원문제가 되지만 각각 응력 혹은 변형률의 정의가 서로 다르다. 먼저 응력-변형률의 관계 식을 구하기 위해 탄성론의 삼차원 변형률을 정의하면

$$
\varepsilon_{x x}=\frac{1}{E}\left[\sigma_{x}-v\left(\sigma_{y y}+\sigma_{z z}\right)\right] ; \quad \gamma_{y y}=\frac{\tau_{x y}}{G}
$$

$$
\varepsilon_{y y}=\frac{1}{E}\left[\sigma_{y y}-v\left(\sigma_{z z}+\sigma_{x x}\right)\right] ; \quad \gamma_{y z}=\frac{\tau_{y z}}{G}
$$

$$
\varepsilon_{z z}=\frac{1}{E}\left[\sigma_{z z}-v\left(\sigma_{x x}+\sigma_{y y}\right)\right] ; \quad \gamma_{x z}=\frac{\tau_{x}}{G}
$$

와 같다. 여기서 $v$ 는 프와송비(Poisson's ratio)이고 G 는 전단탄성계수(shear elastic
modulus)로서 탄성계수 E 와는 다음과 같은 관계가 성립한다.

$$
G=\frac{E}{2(1+v)}
$$

![Aspect ratio](/media/POST/000021/8.jpg)

평면응력 문제는 탄성론의 정의에 의해 다음과 같은 역학적(static) 구속조건을 부여 하면

$$
\sigma_{z z}=\tau_{j \varepsilon}=\tau_{x z}=0
$$

변형률은 다음과 같이 간단해진다.

$$
\varepsilon_{x x}=\frac{1}{E}\left[\sigma_{x x}-v \sigma_{y y}\right] ; \quad \varepsilon_{y y}=\frac{1}{E}\left[\sigma_{y y}-v \sigma_{x x}\right] ; \quad \gamma_{x y}=\frac{\tau_{x y}}{G}
$$

$$
\varepsilon_{=z}=-\frac{v}{E}\left(\sigma_{x x}+\sigma_{y y}\right) ; \quad \gamma_{y z}=\gamma_{x z}=0
$$

위 식을 종속변수를 제외하고 응력에 관해 정리하면 다음과 같은 형태의 평면응력 문제의 구성행렬을 얻을 수 있다.

![Aspect ratio](/media/POST/000021/9.jpg)

평면변형률 문제는 정의에 의해 다음과 같은 운동학적(kinematic) 구속조건을 부여함 으로써 얻어진다.

$$
\varepsilon_{z z}=\gamma_{y z}=\gamma_{x z}=0
$$

$$
\sigma_{z z}=\nu\left(\sigma_{x x}+\sigma_{y y}\right) ; \quad \tau_{y z}=\tau_{x z}=0
$$

$$
\varepsilon_{x x}=\frac{1}{E}\left[\left(1-v^{2}\right) \sigma_{x x}-v(1+v) \sigma_{y y}\right]
$$

$$
\varepsilon_{y y}=\frac{1}{E}\left[\left(1-v^{2}\right) \sigma_{y y}-v(1+v) \sigma_{x x}\right]
$$

$$
\gamma_{x y}=\frac{\tau_{y}}{G}
$$

마지막으로 식들을 응력에 관해 정리하면 다음과 같은 평면변형률문제의 응력-변형률 관계식 및 구성행렬을 얻을 수 있다.

![Aspect ratio](/media/POST/000021/47.jpg)


####2.2.2 형상 함수의 미분 행렬 [$\mathbf{B}$]
평면응력과 평면변형률 문제의 구배행렬은 동일한 식으로써 다음과 같은 영역 변위의 절점변위에 의한 보간으로부터 출발한다.

$$
u(x, y)=N_{1} \cdot u_{1}+N_{2} \cdot u_{2}+\Lambda \Lambda+N_{n e n} \cdot u_{n \sigma n}
$$

$$
v(x, y)=N_{1} \cdot v_{1}+N_{2} \cdot v_{2}+\Lambda \Lambda+N_{n e n} \cdot v_{n e n}
$$

여기서 $\{\mathrm{u}\}^{T}=\{u, v\}$은 요소의 영역변위를, $u_{1}, v_{1}, u_{2}, v_{2}, \mathrm{K}$는 각각 절점 1,2,... 에서의 절점변위를 가리키고 nen 은 요소의 절점 수로써 삼각형의 경우는 3, 사각형의 경 우는4이다.또한보간함수 $N_{1}, N_{2}, \mathrm{K}$ 는각각절점1,2,...에 대한 보간함수이다. 위 식은 행렬의 형태로 나타내면 다음과 같다.

![Aspect ratio](/media/POST/000021/10.jpg)

여기서 [$\mathbf{N}$] 행렬의 크기는 $2 \times 2 n e n$ 이므로 삼각형요소는 $2 \times 6$ , 사각형요소는 $2 \times 8$이 된다.

![Aspect ratio](/media/POST/000021/11.jpg)

다음 식에서 구배행렬 [$\mathbf{B}$]를 얻을 수 있다. 평면응력, 평면변형률 문제에 있어서 는 변형률-변위의 정의가 동일하게 다음과 같이 쓰여 진다.

![Aspect ratio](/media/POST/000021/12.jpg)

즉, [B]행렬의 크기는 32nen 이다. 따라서 주어진 보간함수를 위 식과 같이 미분 함으로써 [B] 행렬을 구할 수 있는 것이다. [B] 행렬의 각각의 항들의 값은 보간함수의 차수에 따라 그 차수가 결정된다. 예를들어 삼각형 3절점 요소의 경우 보간함수는 x 및
y 좌표값의 일차함수이므로 미분하면 상수값들이 된다. 그러나 사각형요소 또는 고차요 소(high order element)라 불리는 요소들은 보간함수가 고차이므로 한번 미분해도 상수값 이 되지 않는다.

2.2.3 강성행렬 [$\mathbf{K}$]
구배행렬 [$\mathbf{B}$] 와 구성행렬 [$\mathbf{D}$] 가 구해지면 요소강성행렬 [$\mathbf{K}$] 를 구하는 것은 행렬 의 곱과 적분에 의해 구해진다. 적분 항을 보면 대부분 [$\mathbf{D}$] 행렬은 재료 상수 값으로 채워진 상수행렬이고 [$\mathbf{B}$] 행렬은 보간 함수의 차수에 따라 상수 혹은 함수가 된다. 예를 들어 일차원 트러스요소의 식이나 삼각형요소의 [$\mathbf{B}$]행렬은 상수행렬이다. 그러므로 평면 응력과 평면변형률 삼각형요소의 경우 적분은 다음과 같이 간단해 진다.

$$
[\mathbf{K}]=[\mathbf{B}]^{T}[\mathbf{D}][\mathbf{B}] \int_{V} d V=[\mathbf{B}]^{T}[\mathbf{D}][\mathbf{B}](A \cdot t)
$$

[$\mathbf{B}$]행렬이 상수가 아닌 경우 요소강성행렬의 계산은 엄밀적분이 매우 번거롭고 때로 는 불가능한 경우도 있으므로 가우스 적분법(Gauss integration)을 이용해 수행한다.

####2.2.4 가우스 적분법
강성행렬은 앞에서와 같이 구배행렬과 구성행렬의 곱을 곱하여 적분한 형태로 나타나게 된다. 아래 식에서 nsd는 요소의 절점당 갖는 자유도 수를 의미하여 2를 의미한다. nst는 독립이 되는 변형률의 개수를 의미한다. 2 차원 연속체에서 다룰 평면 응력, 평면 변형률 문제는 nst3이 된다. nen 은 요소의 절점 수를 나타내는 것으로 선형 삼각형
요소의 경우 nen = 3, 선형 사각형 요소의 경우 nen = 4가 된다.

![Aspect ratio](/media/POST/000021/13.jpg)

여기서 강성행렬의 적분을 계산하려면 형상함수의 미분값인 [$\mathbf{B}$]를 구해야 한다. 이 때 [$\mathbf{B}$]는 형상함수를 x,y에 대해 미분한 행렬이다. 등매개 근사에서 x,y는 $\xi, \eta$로 된 형상함수로 보간되어있는 형태이기 때문에 바로 미분이 불가능하다. 따라서 chain rule 을 이용하여 다음과 같이 계산해야 한다.

$$
\frac{\partial N_{i}}{\partial x}=\frac{\partial N_{i}}{\partial \xi} \frac{\partial \xi}{\partial x}+\frac{\partial N_{i}}{\partial \eta} \frac{\partial \eta}{\partial x}+\frac{\partial N_{i}}{\partial \zeta} \frac{\partial \zeta}{\partial x}
$$

따라서 다음과 같이 변형되어야 한다.

![Aspect ratio](/media/POST/000021/14.jpg)

여기서 $|\underline{J}|$는 자코비언(Jacobian)의 행렬식인데 자코비언 $\underline{J}$는 원 좌표와 natural coordinate 간의 관계를 나타낸다.

$$
\underline{J}=\left[\begin{array}{ll}{\frac{\partial x}{\partial \xi}} & {\frac{\partial y}{\partial \xi}} \\ {\frac{\partial x}{\partial \eta}} & {\frac{\partial y}{\partial \eta}}\end{array}\right]
$$

x,y는 절점에서의 좌표 값과 ,로 된 형상함수로 구성되기 때문에 자코비언 행렬을 구하는 것은 쉽다.

$\xi, \eta$로 구성된 강성행렬 적분 식을 만들었어도 정적분을 직접 계산하는 것은 매우 어렵고 계산량이 많이 든다. 따라서 수치 적분법인 가우스 적분법을 이용하여 강성행렬 을 빠르게 계산하도록 해야 한다.

가우스 적분법은 적분을 계산하기 위해 적분점과 가중치를 이용하는 방법이다. 다음 과 같은 적분 식을 가우스 적분법으로 나타내면 다음과 같다.

$$
\int_{-1}^{1} \int_{-1}^{1} f(\xi, \eta) d \xi d \eta=\sum_{i=1}^{n} w_{i} f\left(\xi_{i}, \eta_{i}\right)
$$

가우스 적분법을 이용해 적분을 정확하게 계산하기 위해선 적분해야 할 식의 차수에 따라 적분점의 수를 설정해주어야 한다. $f(\xi, \eta)$가 등매개 근사를 이용한 것이므로 정 확한 적분을 수행하기 위해선 각 방향으로 최소 2점 적분이 필요하다.

이때 사용하는 적분점의 좌표와 가중치는 다음과 같다.

![Aspect ratio](/media/POST/000021/15.jpg)

##3. 입력문 형식
입력 파일은 Free Field 형식으로 모델 데이터를 정의하는 독자적인 명령어 체계를 가지는 CSD 파일이다. 사용자가 직접 입력문을 수정해야 할 경우의 사용자 편의성을 대폭 향상시켰다. 입력 파일의 입력 형식에 대해서 간략하게 설명하면 다음과 같다.

입력문은 쉼표를 통해서 구분되고 각 자료의 크기가 통일되지 않아도 되는 Free field 방식을 취하고 있다. 따라서 명령문의 각 자료들은 크기나 타입에 관계없이 대부분의 경 우쉼표를 통해서 구별될 수 있다.

유효 자료가 많은 명령문을 위해서 입력 자료의 연속(continuation)성이 보장되도록 설계하였다. 자료를 연속해서 정의하는 경우는 명령문의 마지막에 쉼표와 연속기호 ‘+’를 첨가하게 되면 다음 라인의 자료들도 같은 명령문에 속하게 된다. 즉, 각 줄의 마지막에 ‘+’ 기호를 첨부함으로써 연속기호 ‘+’가 나오지 않을 때까지는 한 줄로 생각할 수 있게 된다. 이 연속 또한 수에 제한 없이 가능하다. 하지만 프로그램 내에서 입력 파일을 읽어 들이는 속도를 향상시키기 위해서 다음과 같은 조건들을 부과하였다. 각 명령문에서 한 자료(쉼표와 쉼표 사이의 자료 값을 의미)의 크기는 문자열 기준으로 30자를 넘을 수 없 고, 한 라인은 300 자를 넘을 수 없다. 그리고 연속을 통해서 묶이는 자료의 크기는 1000 자로 제한하였다.

각 명령문의 시작은 ‘*’ 기호를 통해서 알리게 된다. 입력문을 닫을 때는 마지막에 ‘#END’를 통해서 입력문의 마지막을 알리게 된다. 각 명령문들의 순서는 특별히 없으며, 중복 선언될 수 있다. 각 명령문에 대한 세부적인 설명은 명령문 설명 부분을 참조하도 록 하자. 입력파일의 입력문 특징을 요약하면 아래와 같다.

- 각 자료값들에 제약이 없는 Free Field 형식이다. 
- 쉼표를 통해서 각 자료값들 사이에 구별이 된다. 
- 명령어는 항상 ‘*’ 기호와 같이 온다.
- 명령어들 사이에 특별한 순서는 없다.
- 명령문의 마지막에 ‘+’ 기호를 첨가하여 한 명령문의 연속이 가능하다.
- 입력 파일의 마지막은 ‘#END’로 표시한다.
- 일반적인 명령어들은 문자열 기준 최대 1000 자까지 입력이 가능하다. 
- 한 자료값은 문자열 기준 30자를 넘길 수 없다.
- 입력 파일의 한 줄은 300자를 넘길 수 없다. 연속기호를 활용한다.

![Aspect ratio](/media/POST/000021/16.jpg)

 
###3.1. CONTROL PARAMETER
해석 종류와 변위, 응력, 반력, 내부힘의 출력과 관계된 제어 변수들을 입력받는다. 하위 명령어들에 원하는 자료를 입력하게 되면 해석의 제어가 가능하게 된다. 이 명
령어에 한해서는 연속이 허용되지 않는다. 그리고 필수적인 값은 아니므로 반드시 입력 문에 쓸 필요는 없다. 출력 파일은 .out, .pch, .op2 파일을 의미한다.

![Aspect ratio](/media/POST/000021/17.jpg)

** STRESS POINT 의 경우는 각 요소에 따라서 지원이 가능한 응력 계산점의 위치가 다르다. 명령어 ELEMENT 의 요소 특성들을 참조하여 적절한 조건을 주어야 한다.

####1) 사용규칙
![Aspect ratio](/media/POST/000021/18.jpg)

####2) 사용 예
선형 정적 해석을 수행하며, 변위값, 응력값, 반력값들을 출력 파일에 써주고, 응력값은 요소의 적분점에서 구하는 경우의 입력문은 다음 그림 2과 같다.
![Aspect ratio](/media/POST/000021/19.jpg)
 
###3.2. ELEMENT
요소에 필요한 자료 값들을 표시한다. 모든 요소에서 요소 번호, 요소 종류(문자열 상수), 단면 특성 번호, 재료 특성 번호, 절점 리스트의 순서로 자료 값들이 오게 된다. 본 프로그램에서 지원하는 요소의 종류와 요소의 특성은 아래 표와 같다.
![Aspect ratio](/media/POST/000021/20.jpg)

####1) 사용규칙
![Aspect ratio](/media/POST/000021/21.jpg)

####2) 사용 예

아래 그림에 (1) 4절점 평면 응력 요소(STR41)가 1번 단면 특성과 2번 재료 특성을 참조하는 경우 (2) 3절점 요소(STR31)가 1번 단면 특성과 2번 재료 특성을 참조하는 경 우의 입력문을 적어놓았다.

![Aspect ratio](/media/POST/000021/22.jpg)


###3.3. NODE
절점의 위치 정보를 나타낸다. 현재는 local 좌표계를 사용하지 않기 때문에 참조 좌표계의 의미가 없지만, 기본적으로 NODE 명령에서는 절점의 위치 정보뿐만 아니라 참조 좌표계 번호도 나타내준다.

####1) 사용규칙
![Aspect ratio](/media/POST/000021/23.jpg)
※참조좌표계 1, 참조좌표계 2 는 기본값으로 0 으로 설정되어있다. 

####2) 사용 예
![Aspect ratio](/media/POST/000021/24.jpg)

###3.4. MATERIAL
각 재표 특성들의 자료 값을 나타낸다. 입력 자료들은 재료 특성 번호, 재료 특성 종류, 참조 좌표계, 재료 특성 값들이다. 본 프로그램에서 사용되는 재료 특성의 종류는 선 형 등방성 재료(LISO)와 다양한 재료 특성을 제공하고 있지만, 현재 프로그램 안에 선형 등방성 재료 이외의 재료를 사용하는 함수가 없기 때문에 여기서는 선형 등방성 재료만 사용하도록 한다. 그리고 선형 등방성 재료에서도 실제로 사용하는 자료 값 필드가 몇 개 되지 않기 때문에 사용하지 않는 부분에 대해서는 임의의 값이나 공란으로 남겨두어 도 된다. 참조 좌표계 역시 사용하지 않으므로 임의의 값이어도 무방하다.

####1) 사용규칙
![Aspect ratio](/media/POST/000021/25.jpg)

####2) 사용 예

영계수 E =10000000, 포아송 비 v = 0.3, 밀도 ρ =1000의 특성을 가지는 선형 등방성 재료 특성을 정의하는 경우

![Aspect ratio](/media/POST/000021/26.jpg)

###3.5. PHYSICAL
단면 특성의 자료 값들을 정의한다. 입력 자료들은 단면 특성 번호, 단면 특성 종류, 단면 특성 값들이다. 단면 특성 값들은 각 단면 특성들에 따라서 달라지게 된다. 

####1) 사용규칙
![Aspect ratio](/media/POST/000021/27.jpg)

####2) 사용 예
![Aspect ratio](/media/POST/000021/28.jpg)

###3.6. BOUNDARY
변위 경계 조건 중에서 단절점 경계 조건(SPC)을 정의한다. 입력 자료 값들은 경계 조건 Set 번호, ‘SPC’(단절점 경계 조건을 나타내는 문자열), 참조 좌표계, 적용 절점 번호, 적용 자유도, 변위 값 이다. SPC 경계 조건은 입력문 초기에 주어진 경계 조건 적용 번호와 경 계 조건 Set 번호가 일치하는 것들만 실제 해석에 사용된다. 두 번째 필드의 ‘SPC’는 고 정된 값이다. 참조 좌표계의 경우 사용하지 않으므로 임의의 값이어도 무방하다. 적용 절 점의 경우 경계 조건이 적용되는 절점 번호이고, 적용 자유도의 경우 적용 절점에서 구 속되는 자유도를 의미한다. 적용 자유도의 값들은 6 자유도를 ‘1’,’2’,’3’,’4’,’5’,’6’으로 분배 하여 구속되는 절점의 자유도 번호를 연이어 적어주는 방식이다. 예를 들어 x 방향의 병 진 변위와 y 방향과 z 방향의 회전 변위를 구속하게 되면 적용 자유도의 필드 값은 ‘156’ 이 된다. 마지막으로 변위 값은 절점의 변위 크기를 ‘0.’으로 고정할 것인지, 특정한 값을 줄 것인지를 결정하게 된다. ‘0.’이외의 값이 주어지면 Prescribed boundary condition 이 된다.

####1) 사용규칙
![Aspect ratio](/media/POST/000021/29.jpg)

![Aspect ratio](/media/POST/000021/30.jpg)


####2) 사용 예 

2.1) 예시 1

1 번 절점의 y 방향으로의 움직임이 제한되었고, 2 번 절점의 모든 방향으로의 움직임이 제한되었다.
![Aspect ratio](/media/POST/000021/31.jpg)

  
2.2) 예시 2

1 번 절점의 y 방향으로의 움직임이 제한되고, x,y,z 축을 중심으로한 회전이 제한되었다.

![Aspect ratio](/media/POST/000021/32.jpg)


2.3) 예시 3

1번 절점이 z 방향으로 강제 변위가 1.0으로 주어졌다.

![Aspect ratio](/media/POST/000021/33.jpg)


###3.7. LOAD
각종 하중의 자료 값들을 정의한다. 입력 자료 값들은 하중 Set 번호, 하중 종류, 참조 좌표계, 적용 절점(요소), 스케일 값, 적용 영역, 하중벡터 등이다. 하중의 종류에 따라 서 입력 자료 값들이 약간씩 달라진다. 경계 조건과 마찬가지로 하중 Set 번호와 입력문 의 초기에 오는 Control Parameter 에서의 값이 일치해야 하중 조건을 적용하게 된다. 프 로그램 안에서 처리할 수 있는 하중 명령어들로는 FORCE, MOMENT 가 있다.

![Aspect ratio](/media/POST/000021/34.jpg)

####1) 사용규칙
![Aspect ratio](/media/POST/000021/35.jpg)
![Aspect ratio](/media/POST/000021/36.jpg)

    
####2) 사용 예 

2.1) 예시 1
![Aspect ratio](/media/POST/000021/37.jpg)

2.2) 예시 2
![Aspect ratio](/media/POST/000021/38.jpg)


##4. 종합예제
###4.1 2개의 구멍이 있는 2차원 브라켓
* 본 예제의 입력문 파일명은 TwoD_tutorial.csd 이다.

평면 응력 상태는 두께가 다른 치수에 비해 얇을 때 두께방향으로의 응력이 0 임을 가정한다. 이러한 가정 하에 3차원 고체를 2차원 평면에 표현하여 비교적 간단하게 해석 할 수 있다. 본 예제에서는 평면 응력 상태의 부재를 2 차원 요소로 모델링 하고 해석해 본다. 아래 그림은 2 차원 브라켓을 나타낸다. 이 브라켓은 두께방향으로 얇기 때문에 평면 응력 상태를 가정하고 해석을 진행할 수 있다.

![Aspect ratio](/media/POST/000021/39.jpg)


####4.2 문제 입력문
입력문이 길기 때문에 일부만 이용하여 설명하였다. 전체 입력문은 <파일링크>를 통해 받을 수 있다.

![Aspect ratio](/media/POST/000021/40.jpg)

![Aspect ratio](/media/POST/000021/41.jpg)

![Aspect ratio](/media/POST/000021/42.jpg)

![Aspect ratio](/media/POST/000021/43.jpg)

![Aspect ratio](/media/POST/000021/44.jpg)

![Aspect ratio](/media/POST/000021/45.jpg)

해석을 진행한 결과는 다음과 같다. 응력과 변위가 어떻게 구성되는지 알 수 있다.

![Aspect ratio](/media/POST/000021/46.jpg)

    