---
title: " Self-propulsion test를 통한 선체-추진기 상호작용 해석"
date: "2019-07-07T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Analysis-of-hull-propeller-interaction-through-self-propulsion-test"
category: "Computational fluid Dynamics"
tags: 
 - "Self-propulsion condition"
 - "Propulsion power transmission process"
description: "EDISON 사이언스 앱을 활용해 Self-propulsion test를 통한 선체-추진기 상호작용 해석"
---

## 1. 자항시험의 개요

### 자항 상태: 선박의 추진기의 추력을 이용해 일정 속도를 유지

- 실선의 자항 성능을 설계 단계에서 추정하게 됨
- 자항 성능을 세 요소로 나누어 실험을 수행하고, 그 결과를 실선 스케일에 맞게 확장하여 실선의 전달 동력, 프로펠러 회전수 등을 정함
- 선체: 저항(저항시험)
- 추진기: 추력/토크 (프로펠러 단독 시험)
- **선체-추진기 상호작용: 반류비, 추력감소비, 상대회전효율 (자항 시험)**

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/01.jpg)

### 추진기의 동력 전달과정

- 엔진에서 생성된 동력이 축계를 통하여 추진기에 토크를 전달
- 축계에 전달된 토크로 프로펠러를 회전시켜 추력을 발생

### 자항 시험의 목적

- 1.실선 스케일의 2.선체 반류장안에서 작동하는 프로펠러가 3.선체를 일정 한 속력으로 추진하기 위해 4.축에 가해줘야 하는 일률을 추정하기 위함

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/1.jpg)

### 자항 상태의 특성

- 프로펠러 전방은 낮은 압력이 형성 --> 선체 입장에서는 압력 저항의 증가
  - 추력 감소비(w)와 관련
- 발달된 반류장이 프로펠러에 유입 --> 선박 속도보다 낮은 속도의 유입류
  - 반류비(t)와 관련
- 프로펠러 유입류가 시공간적으로 불균일 --> 토크-추력 특성의 수정 필요
  - 상대회전효율( $\eta_{\mathrm{R}}$ )과 관련

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/02.jpg)

### 자항점 (Self propulsion point)

- 특정 전진 속도에서 추력과 저항의 평형을 이루는 프로펠러 회전수
- 모형선, 실선의 자항점은 서로 Froude 수나 전진비 조건으로 상사되지 않음
- 모형 시험에서는 실선 스케일과의 점성 저항 차이를 고려한 예인력을 추가로 제공해야 함

$$
C_{T S} \times \frac{1}{2} \rho U_{M}^{2} S_{M}=\left(C_{T M}-C_{F M}+C_{F S}+C_{A}\right) \times \frac{1}{2} \rho U_{M}^{2} S_{M}=R_{T M}-F_{D}
$$

- 저항시험 결과로부터 Ctm, Ca를 구해서 활용할 것
  - 이번 실험에서 Cfm은 저항시험 조건에서 활용한 값을 적용하여 구함
  - Cfs는 해수, 수온 15.0°C 조건으로 구함

### 추력 감소비 (Thrust deduction factor)

- 프로펠러 전방의 저압 구간이 선체의 압력 저항으로 작용
- 실제로는 추력 감소가 아닌 선체의 저항이 증가하는 것으로 이해되어야 함

$$
T=(1+a) R_{T}
$$

- 전통적으로 추력이 감소한 것으로 여겨져 현재도 같은 방법을 적용 

$$
t=\frac{T-R_{T}}{T}
$$

- $R_{T}$: resistance of bare hull (자항시험에서 구했음)
  - 본 수업에서는 저항-자항시험 간의 수온 변화에 따른 보정은 적용하지 않음

### 반류비

- 선박의 반류장에서 일하는 프로펠러는 단독 조건보다 느린 유입류에서 작동
- 전진비가 낮아지는 효과를 보임
- 자항 상태의 추력 계수를 확인하여, POW 성능 곡선에서 같은 추력 계수를 가지는 전진비를 구함
- 자항 시험 조건의 전진비와 비교

$$
w_{T}=\frac{V-V_{A}}{V}=\frac{J_{V}-J_{A}}{J_{V}}
$$

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/03.jpg)

- 실선 스케일의 높은 Reynolds 수 조건에서는 길이 차원에 대해 무차원화된 경 계층의 두께가 낮은 Reynolds 수 조건에 비해 작아짐
  - 마찰저항계수의 비교로 확인할 수 있음

$$
w_{T S}=(t+0.04)+\left(w_{T M}-t-0.04\right)\left(\frac{C_{V S}}{C_{V M}}\right)
$$

![image](/media/POST/Comparison-of-2D-and-3D-Wing-Flow-Characteristics/003.jpg)

## 2. 자항시험 수행

### 계측 내용

- 수온
- 모형선 전진 속도
- 프로펠러 회전수
  - 대응되는 추력, 토크
- 예인력 (저항 동력계로 계측)
- 모형선 자세

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/4.jpg)

### 실험 시스템의 배치

- 저항 시험에 더하여 추진기 시스템과 자항 동력계 설치
- 예인전차
  - 트림가이드
  - 저항동력계 
- 모형선
  - 자항동력계

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/05.jpg)

### 실험 순서

- 예인 속도 및 프로펠러 회전 수입력
- 프로펠러 회전
- 전차 예인 --> 클램프 해방
- 저항,자세,추력,토크신호계측
- 클램프 구속 --> 전차 정지
- 저항동력계의 값이 계산된 예인력보다 작다면 추력이 과도한 상태인 것이므로 프로펠러 회전수를 낮춤
- 저항동력계의 값이 계산된 예인력보다 크다면 프로펠러 회전수를 올림

### 실험 조건 (프로펠러 회전수)

- 프로펠러 회전수를 변경하면 모형선에 걸리는 저항의 크기가 달라짐
- 저항 동력계에서 얻은 힘의 크기가 실험 전 추정한 예인력과 같도록 프로펠러 회전수 조정
- 정확한 프로펠러 회전수를 얻는 것은 불가능하므로, 예인력 내외의 3 회전수 조건에 대해서 실험을 수행
- 예인력이 얻어지는 프로펠러 회전수를 내삽하여 구함
- 이 때의 회전수에 대응되는 추력, 토크값을 내삽하여 계산
       
## 3. 자항시험 결과 해석

### 모형 스케일의 추력 특성

- 예인력이 일치되는 회전수를 내삽
- 회전수에 대응되는 추력, 토크값을 내삽

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/06.jpg)

### 각종 계수의 계산

- 계측 결과에서 얻어내는 계수
  - $K_{T M}=\frac{T_{M}}{\rho_{M} n_{M}^{2} D_{M}^{4}}$
  - $K_{Q M}=\frac{Q_{M}}{\rho_{M} n_{M}^{2} D_{M}^{5}}$
  - $t_{M}=\frac{T_{M}-R_{T M}+F_{D}}{T_{M}}$
- POW 결과와의 비교로 얻는 계수 
  - $w_{T M}=\frac{J_{T M} D_{M} n_{M}}{V_{M}}$
  - $\eta_{R M}=\frac{K_{Q T M}}{K_{Q M}}>1.0$

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/007.jpg)

       
### 해석 결과

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/07.jpg)

### 실선 자항 요소의 추정

- $t_{S}=t_{M}$ (스케일 차이 없음)
- $\eta_{R S}=\eta_{R M}$ (스케일 차이 없음)
- $w_{T S}=(t+0.04)+\left(w_{T M}-t-0.04\right) \frac{(1+k) C_{F S}+\Delta C_{F}}{(1+k) C_{F M}}$ (스케일 차이 있음) 1+𝑘 𝐶𝐹𝑀

### 실선 스케일의 자항점 추정

- 실선 스케일의 POW 추정 결과에서 Kt/J^2곡선을 구하고, 이를 실선 자항 요소와 비교하여 전진비를 구함
- 자항 시험 결과의 회전수가 실선에 직접 스케일링되지 않음
- $\left(\frac{K_{T}}{J^{2}}\right)_{S}=\frac{1}{2} \times \frac{S_{S}}{D_{S}^{2}} \times \frac{C_{T S}}{\left(1-t_{S}\right)\left(1-w_{S}\right)^{2}}$

### 실선 자항 성능의 추정

- 상세내용은교과서참고
- 실선 추진기 회전수: $n_{S}=\frac{\left(1-w_{T S}\right) V_{S}}{J_{T S} D_{S}}$
- 선각 효율 (Hull efficiency): $\eta_{H}=\frac{R_{T} V_{S}}{T V_{A}}=\frac{1-t}{1-w}$
  - 선체 유효 동력/반류 조건에서의 프로펠러 추력의 일률 
- 준추진 효율 (Quasi-propulsive efficiency):
  - $\eta_{D}=\frac{R_{T} V_{S}}{2 \pi Q n}=\frac{R_{T} V_{S}}{T V_{A}} \times \frac{T V_{A}}{2 \pi Q_{0} n} \times \frac{Q_{0}}{Q}=\eta_{H} \eta_{0} \eta_{R}$
  - 선각 효율(유효 동력/추력), 프로펠러 단독 효율(추력/토크), 상대회전효율(토크/토크 )의 곱
- 전달 동력 (Delivered power): $P_{D S}=P_{E} / \eta_{D}$

### 실선성능추정

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/08.jpg)

### 자항 시험은 설계 선속 (15.5 knots)에 대해서만 수행

모형선속은 0.797 m/s로 고정

### 저항 시험 결과를 바탕으로 목표 예인력을 구해올것

자항 시험조건의 설정은 저항 동력계에 사전에 계산된 예인력이 걸리는 회전수를 얻는 것이 핵심

### 결과 해석

각 조에서 수행한 실험 결과를 활용하여 실선의 전달동력,자세등을 추정

이전의 저항 시험, POW 시험 결과 및 KRISO자료 등을 참고하여 결과 보고서에 해석 결과 차이의 원인에 대한 고찰을 서술할 것

### 참고 내용

- KVLCC2 자항시험 결과 (KRISO 제공)
- 5.512m 모형선 사용 (축척비 1/58)

![image](/media/POST/Analysis-of-hull-propeller-interaction-through-self-propulsion-test/011.jpg)