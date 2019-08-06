---
title: "humansanode"
date: "2019-08-01T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/humansanode"
category: "Computatinal Medicine"
tags: 
 - "ionic current"
 - "pacemaker voltage"
description: "Human S. A. Node Manual"
---
##manual
##인체 동방 결절 세포 모델
##Human sinoatrial node cell model

##내용
- 소프트웨어 소개
- 이론적 배경 
- 실행방법 
- 입력변수 설명 
- 결과보기
             
##소프트웨어 소개
###학습목표 및 주요기능
####학습목표 
심박동의 근원인 동방결절세포의 페이스메이커 리듬 생성원리를 설명한다.

####주요기능
1. 이온전류 시뮬레이션
2. 페이스메이커 전압 시뮬레이션 
3. 세포 내외 변수에 따른 변화를 시뮬레이션
  
##이론적 배경
###동방결절 (sinoatrial node)
![Aspect ratio](/media/POST/000017/0.jpg)


1. The sino-atrial node (SA node) is located at the junction of the superior vena cava with the right atrium along the sulcus terminalis, an external ridge corresponding to the crista terminalis internally.

2. It is roughly elliptical in shape and is located cephalad to the crista terminalis (hatched area).

3. The myocytes of the sinus node are thin and irregular in shape and measure 5 to 6 micrometers in diameter.

![Aspect ratio](/media/POST/000017/1.jpg)

  
###동방결절 세포의 막전압 오실레이션
![Aspect ratio](/media/POST/000017/2.jpg)

##동방결절세포 모델의 개요
![Aspect ratio](/media/POST/000017/3.jpg)


####3개의구획
1. subsarcolemma
2. cytosol
3. sarcoplasmic reticulum (SR)

####Ca2+ handling
1. $\mathbf{J}_{\mathrm{diff}}$ : from the subsarcolemmal space to the cytosol and
2. $\mathbf{J}_{\mathrm{tr}}$ : from the network SR to the junctional SR
3. $\mathbf{J}_{\mathrm{up}}$ : the Ca2+ uptake from the cytosol into the network SR by the SERCA pump
4. $\mathbf{J}_{\mathbf{r e l}}$ : from the junctional SR into the subsarcolemmal space by the RyRs.

####Ion channels
11 개의 이온통로와 교환기, 펌프

##실행방법
![Aspect ratio](/media/POST/000017/8.jpg)

![Aspect ratio](/media/POST/000017/9.jpg)

![Aspect ratio](/media/POST/000017/10.jpg)

![Aspect ratio](/media/POST/000017/11.jpg)


##입력변수 설명
- $Na$ : Voltage-gated $\mathbf{N a}^{+}$ channel
- $CaL$ : L-type $\mathbf{C a}^{2+}$ channel
- $K1$ : Inward rectifier $\mathbf{K}^{+}$ channel
- $Kto$ : Transient outward $\mathbf{K}^{+}$ channel
- $Kr$ : Rapidly activating delayed rectifier $\mathbf{K}^{+}$ channel
- $KACh$ : Acetylcholine-activated $\mathbf{K}^{+}$ channel
- $funny$ : Hyperpolarization-activated cation channel
- $Kur$ : Ultra-rapid delayed rectifier $\mathbf{K}^{+}$ channel
- $NaCa$ : Na-Ca exchanger
- $NaK$ : Na-K pump
- $bNa$ : Background Na-channel
- $CaT$ : T-type $\mathbf{C} \mathbf{a}^{2+}$ channel
- $RyR$ : Ryanodine receptor channel
- $SERCA$ : Sarcoendoplasmic reticulum $\mathrm{Ca}^{2+}$ pump
- $pCa$ : Sarcolemmal $\mathrm{Ca}^{2+}$ pump

    
##결과 보기
![Aspect ratio](/media/POST/000017/4.jpg)

#####수치를 조절할 때 마우스로 더블 클릭하면 전체 선택이 되므로 수정이 편리함

####'Channel kinetics'
- Shift: Gate의 막전압 의존성을 우측 (+) 혹은 좌측(-)으로 이동시킨다.
- Scale factor: Gate의 τ(시정수)를 조절

####'Reset constants and Calculate'
: 모든 변수를 초기화 하고 모델을 실행함

####'Display option'
: 특정 변수를 플롯

####'Scale factor'
: 특정 변수의 scale factor를 조절

####Radio button 'Length' or 'Force'
: 결과 변수를 Length 혹은 Force에서 선택

####Check button 'Voltage'
: 막전압 플롯 여부

####Check button 'Output'
: Display option에서 선택한 결과 변수 플롯 여부
  
###데이타 보기, 자극크기/간격
![Aspect ratio](/media/POST/000017/5.jpg)

####'Set X range or use mouse zoom'
- 'Apply' 버튼: 선택한 X-축 범위로 결과를 플롯
- 'Autoscale': 변수의 최대/최소값을 기준으로 플롯 범위를 정함
- 그래프 위의 데이타에 마우스를 가져가면 데이타의 값을 보여줌.
- 그래프에서 마우스 끌기로 데이터 범위를 선택하면 선택된 범위의 데이타를 확대하여 보여줌. 원래대로 되돌아가고자 하면 'Autoscale' 버튼을 누름 

####'Stimulus'
- 'Current (pA)': 자극의 크기
- 'Interval (msec)': 자극이 들어가는 간격
   
###pulse protocol
![Aspect ratio](/media/POST/000017/6.jpg)

####'Run protocol'
- 'AP clamp': 활동전압 모드
- 'Voltage clamp': 막전압 고정 모드
- 'Prerun length': 데이타를 얻기 전 미리 시뮬레이션 하는 길이를 결정
- 'Run length': 시뮬레이션 한번 실행의 길이를 결정
- 'Run repeat': 시뮬레이션을 몇 번 반복 할 것인지를 결정
- 'Time step': 시뮬레이션 계산의 timestep. 계산 에러가 나는 경우 time step 을더욱줄여야함.
  
###데이타 저장, 겹쳐서 그리기 결정
![Aspect ratio](/media/POST/000017/7.jpg)

####'Save option'
- 'Save data': 데이타 저장 여부. 실행시간이 길어짐
- 'Skip points': 데이타를 몇 개마다 저장 하는가를 결정. 숫자가 클수록 저장하는 데이타의 크기를 줄일 수 있음

####'Calculate(refresh)'
: 이전 데이터에 겹치지 않고 새로 플롯.

####'Calculate(superimpose)'
: 이전 데이터에 겹쳐서 플롯.
  