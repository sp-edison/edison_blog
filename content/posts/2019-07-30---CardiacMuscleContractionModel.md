---
title: "CardiacMuscleContractionModel"
date: "2019-07-30T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/CardiacMuscleContractionModel"
category: "Computational Chemistry"
tags: 
 - "quantum chamistry"
 - "uChem"
description: "Gamess Simulation Manual"
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

𝑨: 비례상수
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

• T: thin filament with Ca2+- free troponin C
• TCa: thin flament with Ca2+-bound troponin C
• TCa*: TCa attached to cross-bridge
• T*: T attached to cross- bridge
   
이론적 배경
 교차-다리의 칼슘 동역학 반응식 1 칼슘이온이 액틴에 결합하는 알짜 속도
   1
452
3
𝐐𝐛=𝐘𝟏+𝐂𝐚𝟐G +𝐓−𝐙𝟏+𝐓𝐂𝐚
2 교차-다리가 만들어지는 알짜 속도
𝐐𝐚 = 𝐘𝟐 + 𝐓𝐂𝐚 + 𝐞J𝐑+ 𝐋J𝐋𝐚 𝟐 − 𝐙𝟐 + 𝐓𝐂𝐚∗
3 교차-다리에서 칼슘이온이 떨어져 나가는 알짜속도 𝐐𝐫=𝐘𝟑+𝐓𝐂𝐚∗ −𝐙𝟑+𝐓∗ +𝐂𝐚𝟐G
4 칼슘이온이 없는 교차-다리에서 액틴이 떨어지는 알짜속도
𝐐𝐝𝟏 = 𝐘𝐝 + 𝐝𝐗/𝐝𝐭 𝟐 + 𝐓∗
5 칼슘이온이 결합된 교차-다리에서 액틴이 떨어지는 알짜속도
              𝐐𝐝𝟐 = 𝐘𝐝 + 𝐝𝐗/𝐝𝐭 𝟐 + 𝐓𝐂𝐚∗
    
이론적 배경
 근육의 힘과 길이변화를 구하는 순서도
   | ! ∙..
1 교차-다리에서발생하는힘 𝐅𝐛=𝐀𝐛+𝐓𝐂𝐚∗+𝐓∗ +𝐡
2 병렬탄성요소에의해서발생하는힘 F𝐩 = 𝐊 + 𝐋 − 𝐋𝟎 𝟓
3 교차-다리가새로운부착지점으로이동하는 속도
∆𝐡=−𝐬𝐥𝐢𝐧𝐝𝐢𝐧𝐠𝐫𝐚𝐭𝐞+ 𝐡−𝐡𝐜 ∆𝐭
     } $ ∙..
      | = } $ '()* h+ L! ∙..
[Tca*]+[T*]
GL MN
     ~, -./ ∂1 -34Х 678 9:;<7 =>)? 678 h
AB (=D LF G)H IJ )
     
     1. 로그인 클릭
2. ID, Password입력

   1. 앱스토어 클릭

     1. 전산의학 클릭 2. 생리 클릭
3. Run 클릭
  
   1. New Simulation 클릭

      1. Title 입력 2. Create 클릭

            1. cmc_inp 클릭
2. Sample 클릭
3. cmc_inp 입력이 되면 녹색으로 바뀜 4. Submit 클릭

                1. 제출 성공 메시지
2. 화살표 클릭
3. 작업 상태
4. 결과 다운로드 아이콘 클릭 5. 전체파일 다운로드 클릭

입력변수 설명
          
입력변수 설명
        TCa 9:: = TCa %𝑒<=% ><>? @
𝐹=𝐴% TCa∗+T∗ %h "
𝐹=𝐾%𝐿−𝐿2+𝐾%𝐿−𝐿 ,.131
dh⁄dt=−B% h−hc
         
입력변수 설명
       Ca_amplitude
Ca_rise_factor Ca_decay_factor time_length time_step
Ca2+-transient의 amplitude factor
Ca2+-transient의 rise factor Ca2+-transient의 decay factor Total time length
Time step of iteration
Ca2+-transient
  Rise phase
Decay phase
       amplitude

결과 보기
    ― [Ca2+]i
 ― Tension
  
결과 보기
    둘 중의 하나를 클릭하면 그래프를 On/Off 토글할 수 있음
       ― [Ca2+]i
 ― Tension
  
결과 보기
  1 Menu에서 Open server를 선택
              3 OK 선택
2 result2.oneD선택
Ca2+-tension 곡선
   