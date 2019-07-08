---
title: Craig-Bampton Component Mode Synthesis Method
date: "2019-06-18T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/CB-CMS-Method"
category: "Computational Structural Dynamics"
tags:
  - "CB-CMS"
description: "항공기, 차량 등과 같은 크고 복잡한 유한요소모델의 경우 해석의 많은 어려움이 있기 때문에 많은 연구자들이 유한요소모델의 축소하여 효율적으로 해석할 수 축소모델기법을 활용한다. 본 글에서는 축소모델기법에 대한 이론내용을 살펴보고자 한다."
---

- [Introduction](#Introduction)
- [Static Condensation](#Static-Condensation)
- [Dynamic Condensation](#Dynamic-Condensation)
- [Dynamic Substructring](#Dynamic-Substructring)
- [Craig Bampton - CMS](#Craig-Bampton---CMS)

## Introduction

- Model Order Reduction

항공기, 차량 등과 같은 크고 복잡한 유한요소모델의 경우 해석의 많은 어려움이 있기 때 문에 많은 연구자들이 유한요소모델의 축소하여 효율적으로 해석할 수 있는 방법을 연구 중에 있다. 이러한 축소모델기법은 학계뿐 만아니라 산업분야에도 널리 사용되고 있다.

![CB-CMS1.jpg](/media/POST/CB_CMS_Method/CB-CMS1.jpg)

## Static Condensation

Static Condesation기법은 Static Problem을 해결하기위한 모델축소기법으로 전체 노드 중 대표 노드들만 선택하여 Transformation Matrix, TG를 만들어서 강성행렬 K의 자유도를 줄 이는 방법이다.

![CB-CMS2.png](/media/POST/CB_CMS_Method/CB-CMS2.png)

## Dynamic Condensation

Dynamic Condensation 기법은 Static Condensation 기법을 확장하여 동적인 문제 해결하 기위한 모델축소기법이다. Transformation Matrix, TG 를 이용하여 강성행렬 뿐만아니라 질 량 행렬의 자유도를 축소하여 고유치문제를 좀더 효율적으로 해석할 수 있다.

![CB-CMS3.png](/media/POST/CB_CMS_Method/CB-CMS3.png)

## Dynamic Substructring

Dynamic Substructring 기법은 항공기, 차량 등과 같이 여러가지 부분구조물들을 가질 경 우 적용이 용이한 방법으로 원래의 유한요소모델의 분할방법과 경계조건에 따라 여러가지 방법들이 존재한다.

![CB-CMS4.png](/media/POST/CB_CMS_Method/CB-CMS4.png)

## Craig Bampton - CMS

CB-CMS 기법은 Dynamic Substructuring 기법 중 가장 대표적인 방법으로 부분구조물들의 경계조건이 고정되었다는 가정하여 모델을 축소하는 기법으로 학계 및 산업분야에 널 리 사용되고 있는 기법이다.

![CB-CMS5.jpg](/media/POST/CB_CMS_Method/CB-CMS5.jpg)

1. Partitioned Formulation
   전체구조를 부분구조들로 나누고 경계조건을 정의한다. 그에 따라 질량행렬과 강성행렬을 재조합한다.

![CB-CMS6](/media/POST/CB_CMS_Method/CB-CMS6.png)

1. Transformation Matrix
   재조합된 강성행렬과 부분구조들의 고유벡터를 이용하여 Transformation Matrix를 생성한 다. 자유도를 축소하기 위해 고차모드를 제외한 저차모드만 사용한다.

![CB-CMS7](/media/POST/CB_CMS_Method/CB-CMS7.png)

1. Reduced System
   저차모드만 고려되어 생성된 Transformation Matrix를 이용하여 질량행렬 M과 강성행렬 K의 자유도를 축소한다.

$$
\mathbf{T}_{0}=\left[\begin{array}{ccc}{\mathbf{\Phi}_{d}} & {\mathbf{\Phi}_{r}} & {-\mathbf{K}_{s}^{-1} \mathbf{K}_{c}} \\ {\mathbf{0}} & {\mathbf{0}} & {\mathbf{I}_{b}}\end{array}\right] \longrightarrow \overline{\mathbf{T}}_{0}=\left[\begin{array}{cc}{\mathbf{\Phi}_{d}} & {-\mathbf{K}_{s}^{-1} \mathbf{K}_{c}} \\ {\mathbf{0}} & {\mathbf{I}_{b}}\end{array}\right]
$$

$$
\mathbf{u}_{g}=\left[\begin{array}{c}{\mathbf{u}_{s}} \\ {\mathbf{u}_{b}}\end{array}\right]=\mathbf{T}_{0}\left[\begin{array}{c}{\mathbf{q}_{s}} \\ {\mathbf{u}_{b}}\end{array}\right] \quad \longrightarrow \quad \mathbf{u}_{g} \approx \overline{\mathbf{u}}_{g}=\overline{\mathbf{T}}_{0}\left[\begin{array}{c}{\mathbf{q}_{d}} \\ {\mathbf{u}_{b}}\end{array}\right]
$$

$$
\overline{\mathbf{M}}_{p} \ddot{\mathbf{u}}_{p}+\overline{\mathbf{K}}_{p} \overline{\mathbf{u}}_{p}=\overline{\mathbf{f}}_{p}
$$

$$
\overline{\mathbf{M}}_{p}=\overline{\mathbf{T}}_{0}^{T} \mathbf{M}_{g} \overline{\mathbf{T}}_{0}, \quad \overline{\mathbf{K}}_{p}=\overline{\mathbf{T}}_{0}^{T} \mathbf{K}_{g} \overline{\mathbf{T}}_{0}
$$

$$
\overline{\mathbf{u}}_{p}=\left[\begin{array}{l}{\mathbf{q}_{d}} \\ {\mathbf{u}_{b}}\end{array}\right], \quad \overline{\mathbf{f}}_{p}=\overline{\mathbf{T}}_{0}^{T}\left[\begin{array}{l}{\mathbf{f}_{s}} \\ {\mathbf{f}_{b}}\end{array}\right]
$$

[원본 PDF 다운 받으러 가기](https://www.edison.re.kr/web/csd/contents?p_p_id=edisoncontent_WAR_edisoncontent2016portlet&p_p_lifecycle=0&p_p_state=maximized&p_p_mode=view&p_p_col_id=column-1&p_p_col_count=1&_edisoncontent_WAR_edisoncontent2016portlet_myaction=generalModifyView&_edisoncontent_WAR_edisoncontent2016portlet_contentDiv=2001002&_edisoncontent_WAR_edisoncontent2016portlet_contentSeq=31203)
