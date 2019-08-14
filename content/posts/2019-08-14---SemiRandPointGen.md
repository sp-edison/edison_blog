---
title: "SemiRandPointGen"
date: "2019-08-14T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/SemiRandPointGen"
category: "Computational Structural Dynamics"
tags: 
 - "semi-random point"
 - "beam domain"
description: "Semi-random Point Generation Manual"
---
##매뉴얼 – Semi Random Point Generation
###서론
본 해석에 사용되는 Rigid-Body-Spring-Network(RBSN)은 무작위 격자 모델을 일종으로써, 도메인 내에 랜덤하게 시드를 뿌려 무작위한 Voronoi mesh를 이용한다. 이러한 무작위하게 시드 를 생성하는 단계에서 바운더리 조건과 하중 조건에 해당되는 노드 포인트의 추가가 필요하며, 도메인의 경계 조건을 고려한 시드 생성을 필요하다. Semi Random Point Generation 프로그램을 이용하여 이러한 변수를 고려하여 시드를 생성할 수 있다.

![Aspect ratio](/media/POST/000043/0.jpg)

###Input 파일
Mesh를 생성하기 위해서는 Semi point generation을 통해서 도메인 내에 노드를 무작위하게 뿌 리는 단계를 거처야한다. 무작위하게 시드를 뿌리는 인풋파일의 작성 방법은 다음과 같다.

####Dimension.in: 
도메인 내의 시드 생성
![Aspect ratio](/media/POST/000043/1.jpg)

####N: 
도메인 내의 하중 작용점과 서포트 포인트 총 개수 (컨트롤 포인트 총 개수)

####NN: 
하중 종류
![Aspect ratio](/media/POST/000043/2.jpg)
![Aspect ratio](/media/POST/000043/3.jpg)

####LM: 
생성할 mesh의 최소 사이지

####DIMENSION:
 X, Y, Z 방향으로의 dimension 값 ($\sqrt{X^2+Y^2+Z^2}>𝐿𝑀$)

####K:
 컨트롤 포인트(하중 재하 점과 서포트 포인트) 넘버링 
 
 (※ 넘버링이 바뀔 때 줄 변환, N 개 만큼 필요)

####ADDPOINT:
 컨트롤 포인트 K의 x,y,z 축 좌표 (※ 글자간 간격 주의)

(※ LM이 작을수록 노드 수가 많아져 해석 소요 시간이 길어지게 된다. 또한 LM이 도메인의 최대 길이보다 길수 없음)

###프로그램 사용 방법
개별 요소를 이용한 철근 콘크리트 해석 프로그램을 사용하기 위해서는 EDISON 웹사이트 https://www.edison.re.kr/를 접속한 후 통합검색>구조동역학>콘크리트구조물의정적/동적해석>사 이언스앱에 있는 프로그램을 이용하여 해석을 진행할 수 있다.

![Aspect ratio](/media/POST/000043/5.jpg)

![Aspect ratio](/media/POST/000043/6.jpg)

![Aspect ratio](/media/POST/000043/7.jpg)

![Aspect ratio](/media/POST/000043/8.jpg)


###결과 처리
SemiRandPointGen 프로그램을 통해 생선된 point 결과 값을 통해 도메인 내의 노드 포인트를 확인 할 수 있다.

VSView 프로그램 (2019년 구조동역학 5년차 등록 예정 프로그램)을 이용하여 쉽게 형상을 시각화 할 수 있다.

![Aspect ratio](/media/POST/000043/4.jpg)
