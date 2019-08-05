---
title: "Cerebrovascular Arterial Model integrated Yang V8"
date: "2019-07-31T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Cerebrovascular_Arterial_Model_integrated_Yang_V8"
category: "Computational Medicine"
tags: 
 - "Ca2+-tension"
 - "uChem"
description: "Cerebrovascular Arterial Model integrated Yang V8 Manual"
---
##뇌혈관동맥 모델 매뉴얼 Cerebrovascular arterial model

##소프트웨어 소개
- 이론적 배경
- 실행방법
- 입력변수 설명
- 결과보기
- 실습내용
           
##소프트웨어 소개
###학습목표 및 주요기능
####학습목표
뇌혈관 동맥 평활근 세포를 전기적 으로 자극하였을 때 발생하는 수축 이완의 특성을 확인한다.

####주요기능
1. 이온전류 시뮬레이션
2. 교차-다리 칼슘동역학 변수 지원 
3. 교차-다리 역학 변수
4. $C a^{2+}$-tension 관계

  
##이론적 배경
###평활근 세포모델의 개념도
![Aspect ratio](/media/POST/000010/0.jpg)

###Multi-state kinetic model of Ca-Calmodulin dependent myosin phospohrylation
![Aspect ratio](/media/POST/000010/1.jpg)
![Aspect ratio](/media/POST/000010/2.jpg)

---------------------------------------------------------------------------------------------

![Aspect ratio](/media/POST/000010/3.jpg)

###Calculation of CaCM
![Aspect ratio](/media/POST/000010/4.jpg)

###Multi-state kinetic model of Ca- Calmodulin dependent myosin phospohrylation
![Aspect ratio](/media/POST/000010/5.jpg)
![Aspect ratio](/media/POST/000010/6.jpg)

###Mechanical model for the smooth muscle cell
####1. ![Aspect ratio](/media/POST/000010/7.jpg)

- $\mathbf{k}_{\mathbf{x}}$ : elasticity of attached cross bridge
- $\mathbf{k}_{\mathbf{s}}$ : elasticity of series elastic element
- Viscous contribution to cell stiffness ($\boldsymbol{\mu}_{\mathbf{s}}$) causes phase shift between a applied cell length change and the observed change in force.
- $\mathbf{l}_{\mathbf{x}}$ : extension of cross-bridge
- $\mathbf{l}_{\mathbf{a}}$ : length of active element
   

####2. ![Aspect ratio](/media/POST/000010/8.jpg)

- Passive Force

$$
F_{\mathrm{p}}=k_{\mathrm{p}} \cdot\left(e^{\alpha_{\mathrm{p}} \frac{l_{c}-l_{\mathrm{e}}}{l_{\mathrm{o}}}}\right)
$$

![Aspect ratio](/media/POST/000010/9.jpg)


####3. ![Aspect ratio](/media/POST/000010/10.jpg)

- Cross-bridge elasticity

$$
F_{\mathrm{x}}=\left(k_{\mathrm{x} 1} \cdot \mathrm{AMp}+k_{\mathrm{x} 2} \cdot \mathrm{AM}\right) \cdot l_{\mathrm{x}} \cdot e^{-\beta\left(\frac{l_{a}-L_{\mathrm{opt}}}{l_{\mathrm{opt}}}\right)^{2}}
$$

![Aspect ratio](/media/POST/000010/11.jpg)


####4.![Aspect ratio](/media/POST/000010/12.jpg)

- Active force generation
$$
F_{a}=\left[\begin{array}{c}{f_{\text { AMp }} \cdot \operatorname{AMp} \cdot\left(v_{x}-i_{a}\right)} \\ {+f_{\text { AM }} \cdot A M \cdot i_{a}}\end{array}\right] \cdot e^{-\beta\left(\frac{l_{a}-l_{\text { opt }}}{l_{\text { opt }}}\right)^{2}}
$$

![Aspect ratio](/media/POST/000010/13.jpg)

####5. ![Aspect ratio](/media/POST/000010/14.jpg)

- Series viscoelastic force
$$
F_{S}=\mu_{s} \cdot \frac{\mathrm{d} l_{\mathrm{s}}}{\mathrm{d} t}+k_{s} \cdot\left(e^{\alpha_{s} \cdot\left(l_{s}-l_{s 0}\right) / l_{s 0}}-1\right)
$$

![Aspect ratio](/media/POST/000010/15.jpg)


###Actin-myosin state
![Aspect ratio](/media/POST/000010/16.jpg)
![Aspect ratio](/media/POST/000010/17.jpg)
![Aspect ratio](/media/POST/000010/18.jpg)
![Aspect ratio](/media/POST/000010/19.jpg)
![Aspect ratio](/media/POST/000010/20.jpg)



###Active and passive force
![Aspect ratio](/media/POST/000010/21.jpg)
![Aspect ratio](/media/POST/000010/22.jpg)
![Aspect ratio](/media/POST/000010/23.jpg)
![Aspect ratio](/media/POST/000010/24.jpg)
![Aspect ratio](/media/POST/000010/25.jpg)



###Length of elements
![Aspect ratio](/media/POST/000010/26.jpg)
![Aspect ratio](/media/POST/000010/27.jpg)
![Aspect ratio](/media/POST/000010/28.jpg)
![Aspect ratio](/media/POST/000010/29.jpg)
![Aspect ratio](/media/POST/000010/30.jpg)

##Simulation Manual


##실습내용
###간격이 긴 pulse 반복의 효과
'interval': 50000, 'cycle length': 200000, 'No of Pulse':1 로 설정하고 'calculate(reset initial values)' 버튼을 실행하여 $\mathbf{C a}^{2+}$ transient와 근육 의 단축을 확인한다.

'No of Pulse'를 2로 증가시키고 'calculate(reset initial values)' 을 실행한다.

- $\mathbf{C a}^{2+}$ transient의 크기가 두 번째 pulse에서 달라지는가?

- 근육의 단축 정도가 두 번째 pulse에서 달라지는가?

- 'Display option'에서 force, actin-myosin의 변화도 확인

- 'No of Pulse'를 3으로 증가시키고 결과를 확인.

![Aspect ratio](/media/POST/000010/31.jpg)


###간격이 짧은 pulse 반복의 효과
'Display option'은 force로, 'interval': 20000, 'No of Pulse':6으로 바꾸고 'Reset all and calculate'를 실행한다. $\mathbf{C a}^{2+}$ transient의 크기와 force의 변화가 각 pulse 마다 어떻게 달라지는지 기술하시오.

- 'interval': 10000, 'No of Pulse':12로 바꾸고 'calculate(reset initial values)'을 실행하여 이전 결과와 비교한다.
- 'interval': 5000, 'No of Pulse':24로 바꾸고 마찬가지로 'calculate(reset initial values)'을 실행하여 이전 결과들과 비교한다.

계속해서 'interval'은 이전의 절반, 'No of Pulse'는 두 배로 바꿔 가며 그 결 과를 확인한다.
  
###Ca-Calmodulin 관계
'Display option'은 Actin-myosin fraction으로, 'Amplitude':80, 'Duration':100, 'interval':50000, 'No of Pulse':1, 'Cycle length':100000으로 바꾸고 'Reset all and calculate'를 실행한다. Ca2+ transient의 크기와 CaCM의 크기를 확인하시오.

- 'total CM'을 50% 증가시키고 'calculate(reset initial values)'을 실행한 다음, 'Calculate(keep values)'를 실행한다. total CM의 증가로 인한 Ca2+, CaCM transient의 증가, 감소가 같은 방향인가 아니면 다른 방향인가? 그러한 결과가 나 온 이유는?
- AMp, AM, M, Mp의 변화는?
- 'Amplitude'를 60으로 바꾸고 'Reset all and calculate'를 실행한 다음 'Amplitude' 를 80으로 증가시켜 'Calculate(reset initial values)'를 실행하였을 때 Ca2+, CaCM transient의 변화는 같은 방향인가 아니면 다른 방향인가? 그 이유는?
  

###Myosin light chain kinase/phosphatase
'Display option'을 Actin-myosin fraction에 두고, 'Reset pulse'와 'Reset all
and calculate'를 차례로 실행한다.
- K1 value를 증가시키기 위한 방법은 무엇인가? 아래 두 수식을 참조하시오.
$$
\text { (1) } K_{1}=K_{6}=\frac{1}{1+\left(K_{\mathrm{CaCM}} / \mathrm{CaCM}\right)^{2}}
$$

$$
(2)[\mathrm{CaCM}]=\frac{[\mathrm{CM}]_{\mathrm{total}}}{\left(1+K_{\mathrm{d}} /\left[C a^{2+}\right]_{\mathrm{i}}\right)}
$$


- K1 value를 증가시켰을 때 M, Mp, AM, Amp의 변화를 기술하시오.
- 'Display option'을 Cell length 혹은 force에 두고 위의 과정을 반복하시오.
- 'K2'를 10배 증가시켰을 때의 변화는? 실제 몸에서 K2가 증가되는 경우는?
        

###Latch state
'Display option'을 Cell length에 두고, 'Reset pulse'와 'Reset all and calculate'를 실행.

'Display option'을 Force에 두고 'K7'을 0으로 바꾼 다음 'Calculate(reset initial values)'를 실행한다.
- Force 의 변화를 확인한다. Peak에서의 변화는? Steady-state에 도달했을 때의 force는 어떻게 변화되었는가?
- 'Reset all and calculate'를 실행한 후, 'display option'의 Force를 선택한다 (선택이 되어 있어도 클릭한다).
- 'K7'을 10배로 수정하고 'Calculate(reset initial values)'를 실행하여 그 결과를 확인한다.
  