---
title: 설계 예제 - 송전탑 구조의 안전성 평가
date: "2019-06-18T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Design-Example-Safety-Evaluation-of-Transmission-Line-Tower"
category: "Computational Structural Dynamics"
tags: 
 - "Design Example"
 - "Safety Evaluation"
 - "Transmission Line Tower"
 - "EdisonPrePost"
 - "CASADSolver integrated"

description: "송전탑은 발전소에서 만든 전력을 멀리 있는 공장에나 일반 가정 등으로 수송하는 송전선로를 지지하 기 위한 탑이다. 수송 전력에 따라 무게가 10~45t인 대형구조물로, 고압의 전류가 흐르는 전선을 지지 하고 있기 때문에 송전탑 구조의 안전성이 매우 중요하다. 본 예제에서는 송전탑의 설계에 요구되는 안 전성을 평가하는 과정을 제시한다. 또한 학생들이 직접 설계를 변경하면서 안전성을 평가해보도록 하여 안전성 평가의 과정을 이해할 수 있도록 한다."
---

- [1장 개요](#1장-개요)
- [2장 구조 설명](#2장-구조-설명)
- [3장 바람에 의한 하중](#3장-바람에-의한-하중)
- [4장 Electric wire load](#4장-electric-wire-load)
- [5장 설계 규격](#5장-설계-규격)
- [6장 모델링 및 경계조건](#6장-모델링-및-경계조건)
- [7장 해석 결과 및 설계 평가](#7장-해석-결과-및-설계-평가)
- [8장 설계 변경](#8장-설계-변경)
- [9장 고찰](#9장-고찰)
- [10장 연습 문제](#10장-연습-문제)
- [참고 자료](#참고-자료)
- [참고 문헌](#참고-문헌)

## 1장 개요 

– 예제 배경 및 목적

구조 설계 시 고려해야 할 부분 중 가장 중요한 것은 구조의 안전성이다. 특히 대형 구조물의 경우 파 손이 일어난다면 재산 손실뿐만 아니라 대형 참사로 이어질 수 있기 때문에 구조적 안전성을 가지는 설 계는 필수적이다. 이를 위해 구조의 안전성 평가가 이루어져야 하며, 일반적으로 평가에 필요한 변수 산출을 위해 유한요소해석을 활용한 구조 해석이 동반된다.

송전탑은 발전소에서 만든 전력을 멀리 있는 공장에나 일반 가정 등으로 수송하는 송전선로를 지지하 기 위한 탑이다. 수송 전력에 따라 무게가 10~45t인 대형구조물로, 고압의 전류가 흐르는 전선을 지지 하고 있기 때문에 송전탑 구조의 안전성이 매우 중요하다. 본 예제에서는 송전탑의 설계에 요구되는 안 전성을 평가하는 과정을 제시한다. 또한 학생들이 직접 설계를 변경하면서 안전성을 평가해보도록 하여 안전성 평가의 과정을 이해할 수 있도록 한다.

2장에서는 예제로 사용될 송전탑에 대한 스펙을 설명하고, 3장과 4장에서는 설계 하중 도출 과정, 안 전성 평가에 사용되는 설계 규격에 대한 설명을 한다. 5장에서 유한요소해석 모델과 해석 방법을 제시 하고, 6장에서 해석 결과를 바탕으로 설계 평가한다. 7장에서는 설계 변경을 하여 안전성이 어떻게 바뀌 는지 관찰한다. 8장에서는 모멘트 비교 등을 고찰한다.

## 2장 구조 설명

본 예제에서 사용한 송전탑의 도면은 관련 논문으로부터 발췌하여 단순화하였다[1]. 기둥은 항복 응력 이 450MPa인 고장력 강으로 되어 있고, 나머지는 연강으로 이루어 졌다. 높이는 59m, 최대 너비는 26m 이고, 허리 부분은 4.1m이다. 송전탑의 형상과 치수는 *그림 1*에 도시하였다. 기재된 치수는 𝑚𝑚 단위 이다.

![그림 1](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/0.png)
*그림 1 송전탑의 도면*

본 예제에서는 송전탑 부재의 단면에 따른 안전성을 평가한다. 굽힘 강성이 크도록 하기 위해 I형 단면 을 갖는 보 요소로 모델링 한다 [2].
 
## 3장 바람에 의한 하중

송전탑을 설계할 때에는 자중, 바람에 의한 하중, 전선에 의한 하중, 지진 하중 등 다양한 하중을 고려 하여 설계해야 한다. 본 예제에서는 바람에 의한 하중(이하 풍하중)과 전선에 의한 하중을 고려한다. 본 장에서는 설계 코드를 통한 풍하중 계산 과정에 대해 소개한다.

설계 코드는 American Society of Civil Engineers(ASCE)에서 제작한 [“Minimum Design Loads for Buildings and Other Structures”](https://law.resource.org/pub/us/cfr/ibr/003/asce.7.2002.pdf)를 사용하였다 [3]. 이 코드는 바람, 비, 눈, 얼음, 지진 등 다양한 하 중이 건물 및 구조에 작용할 때 하중을 계산하는 방법을 나타낸다. 

본 예제에서는 Main-Wind Force Resisting System(MWFRS)의 풍하중 계산 방법을 명시하고 있는 “Chapter 6.0 Wind Loads”를 참고하 였다. 하중 계산 방법으로 Simplified Procedure, Analytical Procedure, Wind Tunnel Procedure의 세 가지 방법을 제안하고 있다. “6.2 DEFINITIONS”의 BUILDING OF OTHER STRUCTURE, REGULAR- SHAPED의 정의에 따라 “6.5 METHOD2-ANALYTICAL PROCEDURE”로 하중을 계산한다.

### 1. Basic wind speed(𝑽), Wind directionality factor 𝑲𝒅

1971년부터 2014년까지 한국에 기록된 최대 풍속이 43𝑚/𝑠이므로 이 속도를 basic wind speed 𝑉로 설정한다 [4]. 송전탑은 Lattice Framework이므로 Wind directionality factor 𝐾𝑑는 코드의 “Table 6-4, Wind Directionality Factor, 𝐾𝑑”로부터 0.85가 된다.

### 2. Building and structure category, Surface roughness Category, Exposure Category

“Table 1-1, CLASSIFICATION OF BUILDINGS AND OTHER STRUCTURES FOR FLOOD, WIND, SNOW, EARTHQUAKE, AND ICE LOADS”에서 Ancillary structures(tower)에 속하므로 Building and structure category는 IV가 된다. 송전탑은 교외 지역에 주로 설치가 되므로 suburban areas, wooded areas에 속하고 “6.5.6.2 Surface Roughness Categories”에 의해 Surface Roughness는 B가 된다. 이어서 Surface Roughness가 B이므로 “6.5.6.3 Exposure Categories”에 의해 Exposure Category는 B가 된다.

### 3. Importance factor 𝑰, Topographic factor 𝑲𝒛𝒕

Building and structure category이 IV이므로 “Table 6-1, Importance Factor, 𝐼 (Wind Loads)”로 부터 Importance factor 𝐼 는1.15가 된다. 송전탑이 평지에 설치된다고 가정하면 “6.5.7.2 Topographic Factor”에 따라 Topographic factor 𝐾𝑧𝑡는 1이 된다.

### 4. Gust effect factor 𝑮𝒇

“6.2 DEFINITIONS”의 BULIDING AND OTHER STRUCTURE, FLEXIBLE에서 Slender structure를 flexible하다고 규정하고 있다. 이에 따라 Gust effect factor 𝐺𝑓는 식 (6-8)에 의해 결정된다.


|    |            |
|:----------:|:-------------:|
| Gust effect factor<br>(6-8) | $ G_{f}=0.925\left(\frac{1+1.7 I_{\overline{z}} \sqrt{g_{Q}^{2} Q^{2}+g_{R}^{2} R^{2}}}{1+1.7 g_{v} I_{\overline{z}}}\right) $ |
| Intensity of turbulence at height(𝑧̅)<br>(6-5)| $I_{\overline{z}}=c\left(\frac{10}{\overline{z}}\right)^{1/6}$|
|Background response<br>(6-6)|$ \mathrm{Q}=\sqrt{\frac{1}{1+0.63\left(\frac{B+h}{L_{\overline{z}}}\right)^{0.63}}} $|
|Integral length scale of turbulence<br>(6-7)|$ L_{\overline{z}}=l\left(\frac{\overline{Z}}{33}\right)^{\overline{\epsilon}} $|
|(6-19)| $ \begin{array}{c}{g_{Q}=3.4, \quad g_{v}=3.4} \\\\ {g_{R}=\sqrt{2 \ln \left(3600 n_{1}\right)}+\frac{0.577}{\sqrt{2 \ln \left(3600 n_{1}\right)}}}\end{array} $|(6-9)|
|The resonant response factor |$ R=\sqrt{\frac{1}{\beta} R_{n} R_{h} R_{B}\left(0.54+0.47 R_{L}\right)}$|
|(6-10)|$R_{n}=\frac{7.47 N_{1}}{\left(1+10.3 N_{1}\right)^{\frac{5}{3}}}$|
|Reduced frequency<br>(6-12)|$ \begin{array}{c}{N_{1}=\frac{n_{1} L_{\overline{z}}}{\overline{V}_{\overline{z}}}} \\\\ {R_{h}=\frac{\overline{V}_{\overline{z}}}{4.6 n_{1} h}-\frac{1}{2}\left(\frac{\overline{V}_{\overline{z}}}{4.6 n_{1} h}\right)^{2}\left(1-e^{-2\left(\frac{4.6 n_{1} h}{V_{\overline{z}}}\right)}\right)}\end{array} \\\\ \begin{array}{c}{R_{B}=\frac{\overline{V}_{\overline{z}}}{4.6 n_{1} B}-\frac{1}{2}\left(\frac{\overline{V}_{\overline{z}}}{4.6 n_{1} B}\right)^{2}\left(1-e^{-2\left(\frac{4.6 n_{1} B}{\overline{V}_{\overline{Z}}}\right)}\right)} \\\\ {R_{L}=\frac{\overline{V}_{\overline{z}}}{15.4 n_{1} L}-\frac{1}{2}\left(\frac{\overline{V}_{\overline{Z}}}{15.4 n_{1} L}\right)^{2}\left(1-e^{-2\left(\frac{15.4 n_{1} L}{\overline{V} \overline{z}}\right)}\right)}\end{array} $|
|Mean hourly wind speed at height(𝑧̅)<br>(6-14)| $\overline{V}_{\overline{z}}=\overline{b}\left(\frac{\overline{Z}}{10}\right)^{\overline{\alpha}} V$|

Exposure Category가 B이므로 “Table 6-2, Terrain Exposure Constants”로부터

$$
\overline{\alpha}=0.25, \overline{b}=0.45, c=0.3, l=97.54 \mathrm{m}, \overline{\epsilon}=\frac{1}{3}
$$


계산에 이용된 변수 B는 바람에 수직한 구조의 수평 길이를 의미하며 송전탑의 도면에서 가장 큰 수평 너비는 26,961𝑚𝑚이므로 이를 이용한다. 유사하게, L은 바람의 방향과 평행한 구조의 길이를 의미하므로 아래, 중간, 위 구조의 평균 길이인 7,230𝑚𝑚를 이용한다. 높이 𝑧̅는 각 요소의 무게 중심이 위치한 높이로 계산한다. 한편 구조의 natural frequency $n_{1}$와 damping ratio 𝛽는 특정하 기 어려우므로 계산의 편의를 위해 1로 가정한다.

### 5. Velocity pressure 𝒒𝒛


|    |            |
|:----------:|:-------------:|
| Velocity pressure 𝑞𝑧 (6-15)  |    $ q_{z}=0.613 K_{Z} K_{Z t} K_{d} V^{2} I $ |

위 식에서 풍속(𝑉)의 단위는 𝑚/𝑠, velocity pressure 𝑞𝑧의 단위는 𝑃𝑎가 된다. 𝐾𝑑는 [1. Basic wind speed(𝑽), Wind directionality factor 𝑲𝒅](#1-basic-wind-speed𝑽-wind-directionality-factor-𝑲𝒅)에서 구한 wind directionality factor이고, 𝐼와 𝐾𝑧𝑡는 각각 [3. Importance factor 𝑰, Topographic factor 𝑲𝒛𝒕](#3-importance-factor-𝑰-topographic-factor-𝑲𝒛𝒕)에서 구한 importance factor와 topographic factor이다. 추가되는 변수 𝐾𝑧 (velocity pressure exposure coefficient)는 “Table 6-3, Velocity Pressure Exposure Coefficients, 𝐾h and 𝐾𝑧”에 따라 다음과 같이 구할 수 있다.


|    |            |
|:----------:|:-------------:|
| Velocity pressure<br> 𝑞𝑧 (6-15)  | $ K_{z}=\left\{\begin{array}{ll}{2.01\left(\frac{z}{z_{g}}\right)^{\frac{2}{\alpha}}} & {\text { for } 4.572 m \leq z} \\ {2.01\left(\frac{15}{z_{g}}\right)^{\frac{2}{\alpha}}} & {\text { for } z \leq 4.572 m}\end{array}\right. $ |

따라서 무게중심이 4.572𝑚보다 낮은 부재와 높은 부재를 구분하여 적용한다. 3-s gust speed power law exponent α는 “Table 6-3, Velocity Pressure Exposure Coefficients, 𝐾h and 𝐾𝑧”에서 Exposure category가 B이기 때문에 7이 된다.

### 6. Force coefficients(𝑪𝒇)

“Figure 6-22, Other Structures – Method 2, Force Coefficients, 𝐶 ”에서 송전탑의 경우 ε (ratio
of solid area to gross area)가 0.1보다 작고 Flat-Sided Member에 속하므로 Force coefficients 𝐶는 2가 된다.

### 7. Design wind load(𝒑)

최종 설계 풍하중은 다음과 같이 계산한다.

|    |            |
|:----------:|:-------------:|
| Design wind load (6-28)  &nbsp; &nbsp; &nbsp;| $ p=q_{z} G_{f} C_{f} t $ |

Design wind load 𝑝의 단위는 𝑁/𝑚가 된다. 𝐺𝑓⁡ 는 [4. Gust effect factor 𝑮𝒇](#4-gust-effect-factor-𝑮𝒇)에서 구한 Gust effect factor, 𝑞𝑧는 [5. Velocity pressure 𝒒𝒛](#5-velocity-pressure-𝒒𝒛)에서 구한 Velocity pressure, 𝐶 는 [6. Force coefficients(𝑪𝒇)](##6-force-coefficients𝑪𝒇)에서 구한 Force coefficients이며 𝑡는 부재의 두께를 의미 한다. 이 때 𝑡는 구조를 평면으로 사영시켰을 때 만들어 지는 보의 두께를 대입해야 하나 주어진 송 전탑의 경우 사영으로 인한 두께의 감소가 0.1% 수준이기 때문에 사영으로 인한 감소량을 무시하 고 보의 두께를 그대로 대입한다.

## 4장 Electric wire load

본 장에서는 설계 코드를 통한 전선에 의한 하중 계산 과정을 소개한다. 설계 코드는 [“IS 802: Indian Standard, Use of Structural Steel in Overhead Transmission Line Towers – Code of Practice, Part 1 Materials, Loads and Permissible stresses, Section 1 Materials and Loads, 3rd revision, 1995.”](http://questin.org/sites/default/files/standards/is.802.1.1.1995.pdf)를 따른다 [[5]](http://questin.org/sites/default/files/standards/is.802.1.1.1995.pdf).

### 1. Terrain category

설계 코드의 8.3.2.1에 따라 노출된 공간을 고려하여 Category I을 고려한다.

### 2. Gust response factor 𝑮𝒄

Gustresponsefactor는 바람의 난류 및 동적 거동을 반영하기 위해 사용하는 변수이다. 설계 코드 의 Table 7에서는 Gust response factor를 전선의 span 길이와 전선의 높이, terrain category의 함수로 나타내고 있다. 참조 문헌 [6]에 따르면 본 예제에서 다루는 모델과 같이 400kV의 전압을 수송하는 송전탑의 평균 전선 span은 450m이다. 따라서 450m를 span 길이로 선택한다. 한편, 모델에서 전선이 연결되는 높이는 각각 36.5m, 45.7m, 54.9m, 59.1m 이다. 이에 따라 Gust response factor를 구하면 각각 1.95, 2.07, 2.07, 2.07의 값을 갖는다.

### 3. Drag coefficient 𝑪𝒅𝒄

설계 코드 [[5]](http://questin.org/sites/default/files/standards/is.802.1.1.1995.pdf)에 따르면 전선의 경우 1을 선택한다.

### 4. Diameter of conductor 𝒅

참조 문헌 [7]에 따르면 400kV를 송전하기 위한 전선으로 ASCR (Aluminum Conductors, Steel Reinforced)계열의 ‘Moose’를 사용하도록 추천하고 있다. Moose는 재료의 코드명이며 재료의 직 경은 참조 문헌 [8]에 따라 31.77mm를 사용한다.

### 5. Wind pressure 𝑷_𝒅

설계 코드 [[5]](http://questin.org/sites/default/files/standards/is.802.1.1.1995.pdf)의 8.4에는 다음과 같이 wind pressure를 계산하도록 추천하고 있다.

$$
P_{d}=0.6 V^{2}
$$

여기서 𝑉는 설계 풍속으로 3장 1절에서와 같이 43𝑚/𝑠로 설정한다.

### 6. 전선에 의한 하중

전선에 의한 하중은 아래 방향으로의 자중과 바람에 의한 측방향 하중으로 나뉜다. 바람에 의한 측 방향 하중은 설계 코드 [[5]](http://questin.org/sites/default/files/standards/is.802.1.1.1995.pdf)의 9.2에 따라 다음과 같이 계산한다.

$$
F_{c}=P_{d} \times C_{d c} \times L \times d \times G_{c}
$$

Gust response factor가 달라지고, 나머지 변수는 모두 같은 값을 갖게 되며, 두 경우에 대해 계산 되는 전선에 의한 측방향 하중은 30.9kN과 32.8kN이 된다.
자중에 의한 아래 방향 하중은 재료의 data sheet [8]로부터 얻은 단위 길이당 무게와 span 길 이를 곱해 계산한다.

$$
F_{w}=1999 k g / k m \times 450 m \times 9.81 m / s^{2}=8.8246 k N
$$

## 5장 설계 규격

본 예제에서는 AISC(American Institute of Steel Construction)에서 제작한 [‘AISC 360-05; Specification for Structural Steel Buildings’](https://www.aisc.org/globalassets/aisc/publications/standards/a360-16-spec-and-commentary.pdf)를 이용해 설계 평가를 한다. 이 규격은 철 구조물(steelframed buildings and other structures)의 설계에 대한 기준이 제시되어 있다[9].

설계 기준에는 크게 항복, 좌굴, 피로, 파괴 등이 있고 각각의 항목에 대한 구조 해석 방법, 평가기준 이 존재한다. 본 예제에서는 가장 주요한 설계 기준인 항복과 좌굴에 대한 평가를 수행하도록 한다.

### 1. 설계 방법

널리 쓰이는 설계 방법으로는 크게 ASD와 LRFD 방법이 있다. ASD(Allowable Strength Design) 는 각 부재의 허용 강도(Allowable strength)가 작용 강도(Required strength) 보다 크거나 같아 야 하는 조건을 만족하도록 설계하는 방법이다. 허용 강도는 부재의 형상과 항복 응력 등에 의해 분석적(analytic)으로 계산되는, 안전율이 포함된 강도이다. 식으로 나타내면 다음과 같다.

$$
R_{a} \leq \frac{R_{n}}{\Omega}
$$

where,

$$
\begin{array}{l}{R_{a} : \text {required strength }(A S D)} \\\\ {R_{n} : \text { nominal strength }} \\\\ {\Omega : \text { safty factor }} \\\\ {\frac{R_{n}}{\Omega} : \text { allowable strength }}\end{array}
$$

LRFD(Load and Resistance Factor Design) 는 각 부재의 설계 강도(Design strength)가 적용 강 도(Required strength)보다 크거나 같아야 하는 조건을 만족하도록 설계하는 방법이다. 설계 강도 는 허용 강도와 마찬가지로 부재의 형상과 항복 응력 등에 의해 분석적으로 계산된다. 식으로 나타 내면 다음과 같다.

$$
R_{u} \leq \phi R_{n}
$$

where,
$$
\begin{array}{l}{R_{u} : \text {required strength }(L R F D)} \\\\ {R_{n} : \text { nominal strength}} \\\\ {\phi : \text { resistance factor }} \\\\ {\phi R_{n} : \text { design strength }}\end{array}
$$


본 예제에서는 전통적인 방법인 ASD를 채택하여 설계 평가를 하도록 한다.

### 2. 설계 조건 설명

#### 1) Design of members for tension

축 방향 정적 인장력을 받는 부재의 허용 강도를 구한다. 각 파라미터의 식은 다음과 같다. 𝐹 와
𝐴𝑔는 각각 𝑦𝑖𝑒𝑙𝑑⁡ 𝑠𝑡𝑟𝑒𝑠𝑠와 𝑔𝑟𝑜𝑠𝑠⁡ 𝑎𝑟𝑒𝑎를 의미한다.

|파라미터 종류| 식 |
|--------|---------|
|Nominal strength 𝑃𝑛 |𝑃𝑛 =𝐹𝑦𝐴𝑔 |
|Safety factor|Ω𝑡 = 1.67|
|Allowable tensile strength|𝑃𝑛/Ω𝑡|

#### 2) Design of members for compression

축 방향 정적 압축력을 받는 부재의 허용 강도를 구한다. 각 파라미터의 식은 다음과 같다. 𝐸는 𝑌𝑜𝑢𝑛𝑔′𝑠⁡ 𝑚𝑜𝑑𝑢𝑙𝑢𝑠를 의미한다.

|파라미터 종류| 식 |
|--------|---------|
|Nominal strength 𝑃𝑛| 𝑃𝑛=𝐹𝑐𝑟𝐴𝑔 |
|Flexural buckling stress| $ \begin{array}{c}{F_{c r}=\left[0.658^{F_{y} / F_{e}}\right] F_{y} \quad \text { or }} \\ {F_{c r}=0.877 F_{e}}\end{array} $ |
|Elastic critical buckling stress| $ F_{e}=\frac{\pi^{2} E}{(K L / r)^{2}} $ |
|Safety factor|Ω𝑡 = 1.67|
|Allowable compressive strength| 𝑃𝑛/Ω𝑡 |

부재의 얇은 정도를 나타내는 것을 Slenderness ratio라 하고, 다음과 같이 정의한다.

$$
\text {Slenerness ratio}=K L / r
$$

where,
$$
\begin{array}{l}{\text {K: the effective length factor}} \\\\ {\text {L: length of the member }} \\\\ {\text { r: governing radius of gyration }}\end{array}
$$

이 𝑆𝑙𝑒𝑛𝑒𝑟𝑛𝑒𝑠𝑠⁡ 𝑟𝑎𝑡𝑖𝑜 값에 따라 𝐹𝑐𝑟 을 다르게 구한다. 

#### 3) Design of members for flexure

Principal axis에 대해 굽힘(bending)을 받는 부재의 허용 강도를 구한다. 각 파라미터의 식은 다음 과 같다.

|파라미터 종류| 식 |
|--------|---------|
|nominal strength 𝑀𝑛| $ M_{n}=M_{p}=F_{y} Z_{x} \text { or } \\\\ M_{n}=C_{b}\left[M_{p}-\left(M_{p}-0.7 F_{y} S_{x}\right)\left(\frac{L_{b}-L_{p}}{L_{r}-L_{p}}\right)\right] \quad  \\\\ M_{n}=F_{c r} S_{x} $ |
|Safety factor|$\Omega_{b}=1.67$|
|Allowable flexural strength|$ M_{n} / \Omega_{b}$|


$ L_{b}, L_{p}, L_{r} $ 는 𝑙𝑖𝑚𝑖𝑡𝑖𝑛𝑔⁡ 𝑙𝑒𝑛𝑔𝑡h𝑠로, 각각 부재 형상, 𝐸, 𝐹𝑦 , 𝑡𝑜𝑟𝑠𝑖𝑜𝑛𝑎𝑙⁡ 𝑐𝑜𝑛𝑠𝑡𝑎𝑛𝑡, 𝑟, 𝑆𝑥 등에 의해 결정된다. 이 세 길이의 관계에 따라 𝑀𝑛을 구하는 식을 선택하여 사용하게 된다.

#### 4) Design of members for combined forces and torsion

위에서 구한 허용강도들을 이용하여 설계 조건을 제시한다. 구조 해석 결과(required strength) 는 다음 식들을 만족해야 한다.

$$
\begin{array}{ll}{\frac{P_{r}}{P_{c}}+\frac{8}{9}\left(\frac{M_{r x}}{M_{c x}}+\frac{M_{r y}}{M_{c y}}\right) \leq 1.0} & {\text { for } \frac{P_{r}}{P_{c}} \geq 0.2} \\\\ {\frac{P_{r}}{2 P_{c}}+\left(\frac{M_{r x}}{M_{c x}}+\frac{M_{r y}}{M_{c y}}\right) \leq 1.0} & {\text { for } \frac{P_{r}}{P_{c}}<0.2}\end{array}
$$

where,
$$
\begin{array}{l}{P_{r} : \text {required axial compressive (or tensile) strength}} \\\\ {M_{r} : \text {required flexural strength}} \\\\ {P_{c} : \text { allowable axial compressive (or tensile)strength }} \\\\ {M_{c} : \text { allowable flexural strength }}\end{array}
$$

### 3. 설계 평가 방법

안전 여유(Safety Margin)를 다음과 같이 정의한다.

$$
\eta \equiv\left(\frac{R_{n}}{\Omega}\right) / R_{a}
$$

ASD 설계 조건 식으로부터, 위 식은 다음과 같은 형태로 나타낼 수 있다.

$$
\frac{1}{\eta}-1 \leq 0
$$

안전 여유는 설계 조건에 대한 여유를 나타낸다. 𝜂>1인 경우는 설계 조건 범위 안에 있는 것, 𝜂<1인 경우는 설계 조건 위배, 𝜂=1인 경우는 설계 조건에 닿아 있는 상태를 뜻한다.
  
## 6장 모델링 및 경계조건

그림 2는 전처리기에 나타나는 유한요소모델의 모습이다.

![그림 2](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/1.jpg)
*그림 2. 송전탑 유한요소 모델*

### 1. Elements

절점과 절점 사이를 한 개의 요소로 모델링한다. 이 때, 모든 요소는 축력과 굽힘력을 모두 전달할 수 있는 요소인 보 요소를 이용한다.

### 2. Boundary conditions

송전탑과 지면이 맞닿는 4개의 절점의 모든 자유도를 구속한다.

### 3. Loads

그림 2에 나타난 하중은 도면을 발췌한 논문[1]에서 시뮬레이션에 이용한 전선에 의한 하중을 표 시한 것이다. 하중이 송전탑의 측방향으로 가해지고 있다. 따라서 풍하중도 측방향으로 가해지는 경우가 주어진 하중 조건에서 최대 하중이 가해지는 경우라고 할 수 있다. 따라서 송전탑의 측면에 풍하중이 작용하는 것을 가정한다.

설계 규격에 따라 계산한 하중이 균일 분포하중으로 송전탑에 가해짐을 가정하고 송전탑의 측면 부재에 풍하중을 부여한다.

### 4. Physical properties

그림 3은 송전탑의 초기 모델링에 이용한 부재가 어떤 단면을 가지고 있는지 나타낸 것이다.

![그림 3](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/2.png)
*그림 3. 유한요소모델에 사용된 Beam 단면*

단면 형상은 Gere의 [“Mechanics of Materials, 7ed”](https://www.academia.edu/34277150/Mechanics_of_Materials_7th_edition_beer.pdf) [2]의 Appendix E 에서 소개하고 있는 규격 단면 을 사용하였고, 각 부재의 세부적 치수는 표 1과 같다.

*표 1. 초기 모델의 단면 형상 치수*
![표 1](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/table1.png)

### 5. Material properties

송전탑의 재질은 steel이다 [1]. 영 계수는 210𝐺𝑃𝑎, 푸아송 비는 0.3으로 설정하였다.

## 7장 해석 결과 및 설계 평가

본 예제에서는 송전탑을 지지하고 있는 다리 부재에 대해 설계 평가를 수행한다. 첨부된 모델 파일의 11번 요소가 이에 해당한다. 그림 4에서 녹색으로 표현된 요소가 11번 요소 이다.

![그림 4](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/3.jpg)
*그림 4. 안전성을 평가한 요소(11번 요소)*

유한요소해석 결과로 나온 11번 요소의 절점 힘을 5장에서 제시한 설계 조건식에 대입하여 안전 여유 값을 도출한다. 학생들은 안전 여유가 계산되도록 작성된 MATLAB 파일을 이용하도록 한다. 초기 단면 에 대한 안전 여유 값은 다음과 같다.

$$
\eta=0.675
$$

도출된 안전 여유가 1보다 작기 때문에 설계조건을 위반하게 되어 안전하지 못한 상태이다.

## 8장 설계 변경

안전한 설계를 위하여 최하단의 부재 “HE180B”를 “HE220B”로 변경한다. 표 2는 변경된 부재의 치수 이다.

*표 2. 변경된 모델의 단면 형상 치수*
![표 2](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/table2.png)

변경된 설계에 대한 안전 여유 값은 다음과 같다.

$$
\eta=1.966
$$

도출된 안전 여유가 1보다 크기 때문에 설계 조건을 만족하고, 안전성 평가를 통과하였음을 확인할 수 있다.

## 9장 고찰

본 예제에서는 모든 요소를 보 요소로 모델링 하였다. 실제 송전탑의 설계에 있어서 기둥과 기둥을 잇 는 영역의 부재는 보 요소 보다 트러스 요소에 더 가깝다. 따라서 개선된 설계 안과 같은 단면 특성을 갖도록 하여 bracing 부재를 트러스 요소로 모델링 하여 해석해 본다. 그림 5는 트러스 요소와 보 요 소가 어떻게 배치되는지를 나타내는 그림이다.

![그림 5](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/4.png)
*그림 5. 보 요소와 트러스 요소의 배치*

모델링을 할 때 풍하중은 분포하중으로 적용된다. 분포하중은 절점에 부여되는 하중과 등가 일의 원 리를 만족하도록 하여 절점력으로 변환된다. 보 요소에 대한 분포하중은 집중하중과 집중 모멘트로 변 환된다. 한편 트러스 요소로 모델링 되는 부재가 받는 풍하중을 적용하기 위해 해당 부재가 지지하는 전체 하중을 구한 뒤 절점에 집중하중의 형태로 부여하도록 한다. 이렇게 모델링 하여 해석한 결과를 그림 6에 도시하였다.

![그림 6-1](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/5.png)
*그림 6-1. 해석 결과의 변위 해를 후처리기에서 나타낸 모습 (보 요소만으로 모델링 한 경우)*

![그림 6.2](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/6.png)
*그림 6-2. 해석 결과의 변위 해를 후처리기에서 나타낸 모습 (트러스 요소와 보 요소를 혼합한 경우)*

분포하중을 보 요소 또는 트러스 요소로 모델링 함에 따라 모멘트의 적용 여부가 결정된다. 트러스 요소와 혼합한 경우 트러스 요소가 받는 모멘트는 모델링 되지 않아 전체 하중이 한 일은 보 요소만으 로 모델링 한 경우보다 적을 것임을 예측할 수 있다. 따라서 그림 6의 해석 결과에서도 더 작은 변위 해가 도출된 것을 알 수 있다.

두 경우에 대해 기둥에 작용하는 모멘트에 대한 선도를 그려본다. 그림 7은 모멘트 선도를 그린 요소 를 녹색으로 표현한 것이고, 그림 8은 두 경우에 대한 모멘트 선도를 나타낸 것이다. 이 때 모멘트 선 도에 나타난 요소 절점의 순서는 아래에서 위 방향이다. 즉, 그림 8에서 1이라고 표현된 절점이 가장 아래의 절점이다.

![그림 7](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/7.jpg)
*그림 7. 모멘트 선도를 그린 기둥*

![그림 8-1](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/8.png)
*그림 8-1. 모멘트의 비교 (왼쪽: 그림 7에서 highlight 된 요소에 나타나는 모멘트*

![그림 8-2](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/9.png)
*그림 8-2. 모멘트의 비교 (전선에 의한 하중과 좌측과 우측을 잇는 보 요소를 트러스 요소로 모델링 한 경우에 나타나는 모멘트)*

그림 8의 좌측 그림을 통해 모든 요소를 보 요소로 한 경우와 bracing을 트러스 요소로 한 경우의 모멘트 차이가 크지 않은 것을 알 수 있다. 이는 분포하중이 그림 7의 우측 기둥에 작용하더라도 전선 에 의한 하중이 영향을 미치고, 그림 5의 하단에 가로로 놓여진 보 요소에 의해 모멘트가 충분히 전달 되기 때문이다. 전선에 의한 하중을 제거하고 가로로 놓인 보 요소를 트러스 요소로 둔 경우 나타나는 모멘트를 그림 8의 우측에 나타내었다. Bracing을 트러스 요소로 한 경우의 모멘트가 상대적으로 작게 발생함을 알 수 있다. 이는 보 요소로 이루어진 기둥을 트러스 요소가 연결하는 모델의 경우 하중을 받 지 않는 면으로 모멘트 전달이 원활하지 못해 하중을 받는 영역이 더 큰 하중을 지지하게 되기 때문이다.
  
설계자는 설계하고자 하는 대상을 명확히 이해하고 보 요소와 트러스 요소 중 어떤 요소에 더 가까운 지 판단할 필요가 있다. 실제 설계에서는 일부의 모멘트를 전달하는 것을 모사하기 위해 연결 부위에 비틀림 용수철(Torsional spring)을 모델링하여 부재의 거동을 보다 현실적으로 표현할 수 있다.

## 10장 연습 문제

그림 9~15는 송전탑의 도면이다. 도면 중 하나를 택하고 다음 물음에 답하도록 한다.

1) 3장과 4장에 제시된 방법을 이용하여 바람에 의한 하중과 전선에 의한 하중을 계산한다.
2) Edison 프로그램을 사용하여 송전탑을 모델링하고, 문제 1)에서 계산한 하중을 적용하여 해석 한다. 이 때, 보, 트러스의 단면 형상과 재료의 종류는 직접 선택해 본다.

3) 5장에 제시된 방법을 이용하여 현재 설계된 구조가 안전한지 평가해 본다.

4) 구조의 일부 요소를 트러스 요소로 바꿔보고, 모멘트선도, 전단력선도를 그려 평가해 본다.

5) 일부의 bracing을 제거 및 추가, 단면 형상의 변화 등을 적용해 설계를 개선해 본다.

![그림 9](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/10.png)
*그림 9. 연습문제 1*

![그림 10](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/11.png)
*그림 10. 연습문제 2*

![그림 11](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/12.png)
*그림 11. 연습문제 3*

![그림 12](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/13.png)
*그림 12. 연습문제 4*

![그림 13](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/14.png)
*그림 13. 연습문제 5*

![그림 14](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/14.png)
*그림 13. 연습문제 6*

## 참고 자료

### 송전탑 구조 안정성 평가 해석 입력 파일 

[TLT_Initial_Beam.csd](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/TLT_Initial_Beam.csd)

[TLT_Modified_Beam.csd](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/TLT_Modified_Beam.csd)

[TLT_Modified_BeamTruss.csd](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/TLT_Modified_BeamTruss.csd)

### 관련 Science App

전후처리기 프로그램 : [EdisonPrePost](https://www.edison.re.kr/web/csd/scienceappstore/-/scienceapp/EdisonPrePost/2-0-0/view)

해석 프로그램 : [CASADSolver integrated](https://www.edison.re.kr/web/csd/scienceappstore/-/scienceapp/CASADSolver_integrated/1-0-0/view)

### 하중 계산 엑셀 파일

[요소별_하중.xlsx](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/요소별_하중.xlsx)

[하중_계산_과정.xlsx](/media/POST/Safety-Evaluation-of-Transmission-Line-Tower/하중_계산_과정.xlsx)

[원본 컨텐츠 파일 다운 받으러 가기](https://www.edison.re.kr/web/csd/contents?p_p_id=edisoncontent_WAR_edisoncontent2016portlet&p_p_lifecycle=0&p_p_state=maximized&p_p_mode=view&p_p_col_id=column-1&p_p_col_count=1&_edisoncontent_WAR_edisoncontent2016portlet_myaction=generalModifyView&_edisoncontent_WAR_edisoncontent2016portlet_contentDiv=2001004&_edisoncontent_WAR_edisoncontent2016portlet_contentSeq=20006)

## 참고 문헌

[1] N. P. Rao, G. M. S. Knight, N. Lakshmanan, N. R. Iyer, “Investigation of transmission line tower failures”, Engineering Failure Analysis 17, p. 1127-1141, 2010

[2] J. M. Gere, B. J. Goodno, “Mechanics of Materials, 7-th ed.”, Cengage Learning, 2009 

[3] ASCE, “Minimum Design Loads for Buildings and Other Structures 2-nd ed.”, 2003

[4] 한국 기상청 홈페이지, <http://www.kma.go.kr>

[5] IS 802: Indian Standard, Use of Structural Steel in Overhead Transmission Line Towers – Code of Practice, Part 1 Materials, Loads and Permissible stresses, Section 1 Materials and Loads, 3rd revision, 1995.

[6] C. Reynolds, Review of 400kV & 220kV trans-power proposal, Electricity commission PB power, ISO 9001, 2000.

[7] 송전선 관련 업체 홈페이지 (Uttar Pradesh power transmission corporation limited)
<http://upptcl.org/tech_info/conductors_for_trans.htm>
<http://upptcl.org/tech_info/current_carrying.htm>

[8] Conductor data sheet, Aluminum conductors steel reinforced (ACSR)

[9] AISC, “360-05; Specification for Structural Steel Buildings”, 2005
