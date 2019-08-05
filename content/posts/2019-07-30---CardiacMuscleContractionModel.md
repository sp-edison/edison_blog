---
title: "CardiacMuscleContractionModel"
date: "2019-07-31T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/CardiacMuscleContractionModel"
category: "Computational Medicine"
tags: 
 - "organ model"
 - "physiology"
 - "pharmacologu"
description: "Cardiac Muscle Contraction Model Manual"
---
#매뉴얼
##심근수축모델 Cardiac muscle contraction model
 
##내용
- 소프트웨어 소개 
- 이론적 배경 
- 실행방법 
- 입력변수 설명 
- 결과보기
             
##소프트웨어 소개
###학습목표 
$C a^{2+}$-transient 발생에 의해 수축과 이완이 발생하는 인체 심근의 특성을 확인한다.

###주요기능
1. 근육 수축 시뮬레이션
2. 교차-다리 칼슘동역학 변수 지원 
3. 교차-다리 역학 변수
4. Ca2+-tension 관계

  
##이론적 배경
###심근 vs 골격근
![Aspect ratio](/media/POST/00009/0.jpg)
![Aspect ratio](/media/POST/00009/1.jpg)

###교차-다리 주기
![Aspect ratio](/media/POST/00009/2.jpg)
   
###심근수축의 기본단위
![Aspect ratio](/media/POST/00009/3.jpg)

- $L=L_{m} / N_{0}$
- 𝐿:근절절반의길이
- $L_{\mathrm{m}}$: 근육 전체의 길이
- $N_{0}$:근절의숫자
- Thick filament: 미오신 필라멘트
- Thin filament: 액틴 필라멘트
- h: 교차-다리 두 부착점 사이의 수평거리
- Parallel element: 근절의 병렬 요소
  
###심근의 교차-다리 역학
![Aspect ratio](/media/POST/00009/4.jpg)

1. 교차-다리에서 발생하는 힘
$$
\boldsymbol{F}_{\boldsymbol{b}}=\boldsymbol{A} \cdot \mathbf{h}
$$

$$
𝑨: 비례상수
$$

2. 병렬 탄성 요소에 의해 발생하는 힘
$$
\mathbf{F}_{\mathbf{p}}=\mathbf{K} \cdot\left(\mathbf{L}-\mathbf{L}_{0}\right)^{5}
$$
$$
\text { K: equation parameter }
$$
$$
\mathbf{L}_{0} : \text { unstressed length of } \mathbf{F}_{\mathbf{p}}
$$

3. 교차-다리가 새로운 부착지점으로 이동하는 속도
$$
\frac{\Delta \mathbf{h}}{\Delta \mathbf{t}}=-\text { slinding rate } \cdot\left(\mathbf{h}-\mathbf{h}_{\mathbf{c}}\right)
$$
  
###교차-다리의 칼슘동역학
![Aspect ratio](/media/POST/00009/5.jpg)

- $\mathbf{T4}$: thin filament with $\mathbf{C a}^{2+}$- free troponin C
- $\mathbf{TCa}$: thin flament with $\mathbf{C a}^{2+}$-bound troponin C
- $\mathbf{T} \mathbf{C} \mathbf{a}^{*}$: TCa attached to cross-bridge
- $\mathbf{T}^{*}$: T attached to cross- bridge
   
###교차-다리의 칼슘 동역학 반응식
![Aspect ratio](/media/POST/00009/6.jpg)

1. 칼슘이온이 액틴에 결합하는 알짜 속도
$$
\mathbf{Q}_{\mathbf{b}}=\mathbf{Y}_{\mathbf{1}} \cdot\left[\mathbf{C a}^{2+}\right] \cdot[\mathbf{T}]-\mathbf{Z}_{\mathbf{1}} \cdot[\mathbf{T} \mathbf{C} \mathbf{a}]
$$

2. 교차-다리가 만들어지는 알짜 속도
$$
\mathbf{Q}_{\mathbf{a}}=\mathbf{Y}_{2} \cdot[\mathbf{T} \mathbf{C} \mathbf{a}] \cdot \mathbf{e}^{-\mathbf{R} \cdot\left(\mathbf{L}-\mathbf{L}_{\mathbf{a}}\right)^{2}}-\mathbf{Z}_{\mathbf{2}} \cdot\left[\mathbf{T} \mathbf{C} \mathbf{a}^{*}\right]
$$

3. 교차-다리에서 칼슘이온이 떨어져 나가는 알짜속도
$$
\mathbf{Q}_{\mathbf{r}}=\mathbf{Y}_{3} \cdot\left[\mathbf{T} \mathbf{C} \mathbf{a}^{*}\right]-\mathbf{Z}_{3} \cdot\left[\mathbf{T}^{*}\right] \cdot\left[\mathbf{C a}^{2+}\right]
$$

4. 칼슘이온이 없는 교차-다리에서 액틴이 떨어지는 알짜속도
$$
\mathbf{Q}_{\mathbf{d} \mathbf{1}}=\mathbf{Y}_{\mathbf{d}} \cdot(\mathbf{d} \mathbf{X} / \mathbf{d} \mathbf{t})^{2} \cdot\left[\mathbf{T}^{*}\right]
$$

5. 칼슘이온이 결합된 교차-다리에서 액틴이 떨어지는 알짜속도
$$
\mathbf{Q}_{\mathrm{d} 2}=\mathbf{Y}_{\mathbf{d}} \cdot(\mathbf{d} \mathbf{X} / \mathbf{d} \mathbf{t})^{2} \cdot\left[\mathbf{T} \mathbf{C} \mathbf{a}^{*}\right]
$$
    
###근육의 힘과 길이변화를 구하는 순서도
![Aspect ratio](/media/POST/00009/7.jpg)
   
1. 교차-다리에서 발생하는 힘 
$$
\mathbf{F}_{\mathbf{b}}=\mathbf{A}_{\mathbf{b}} \cdot\left(\left[\mathbf{T} \mathbf{C} \mathbf{a}^{*}\right]+\left[\mathbf{T}^{*}\right]\right) \cdot \mathbf{h}
$$

2. 병렬 탄성 요소에 의해서 발생하는 힘 
$$
\mathbf{F}_{\mathbf{p}}=\mathbf{K} \cdot\left(\mathbf{L}-\mathbf{L}_{0}\right)^{5}
$$

3. 교차-다리가 새로운 부착 지점으로 이동하는 속도
$$
\frac{\Delta \mathbf{h}}{\Delta \mathbf{t}}=-\text { slinding rate } \cdot\left(\mathbf{h}-\mathbf{h}_{\mathbf{c}}\right)
$$


##입력변수 설명
![Aspect ratio](/media/POST/00009/8.jpg)
![Aspect ratio](/media/POST/00009/9.jpg)
![Aspect ratio](/media/POST/00009/10.jpg)
![Aspect ratio](/media/POST/00009/11.jpg)

- Ca_amplitude : $\mathbf{C} \mathbf{a}^{2+}$- transient의 amplitude factor
- Ca_rise_factor : $\mathbf{C} \mathbf{a}^{2+}$- transient의 rise factor
- Ca_decay_factor : $\mathbf{C a}^{2+}$- transient의 decay factor
- time_length : Total time length
- time_step : Time step of iteration

##결과 보기
![Aspect ratio](/media/POST/00009/12.jpg)

![Aspect ratio](/media/POST/00009/13.jpg)

![Aspect ratio](/media/POST/00009/14.jpg)

![Aspect ratio](/media/POST/00009/15.jpg)
