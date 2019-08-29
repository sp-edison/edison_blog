---
title: "CNTModeler"
date: "2019-08-29T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/CNTModeler"
category: "Nano Physics"
tags: 
 - "Carbon Nano Tube"
 - "Modeler"
description: "Carbon Nano Tube Modeler Manual"
---
##그래핀나노리본, 탄소나노튜브 모델러 설명서
##Theoretical background
### 탄소(Carbon)와 탄소동소체
####탄소
![Aspect ratio](/media/POST/000069/0.jpg)
원자번호 6번의 원소로 원소기호는 C이다. 주기율표에서 14족(4A족)에 속하는 비금속 원소로, 공유결합을 할 수 있는 4개의 원자가 전자를 갖고 있다.

탄소는 실온에서는 매우 안정하며, 높은 온도에서 산소와 반응한다. 탄소는 구조와 성질이 아주 다른 여러 동소체로 존재한다.

####탄소 동소체[1]
탄소는 원자가 1개씩 상태로 떨어져있는 상태로 존재하기는 아주 어렵고 여러 원자들이 모여 안정된 구조로 있게 되는데, 구조의 원자 배열이 크게 다른 여러 가지 동소체로 존재한다. 동소체로는 무정형 탄소, 다이아몬드, 흑연, C60, 탄소나튜브, 그래핀, 그래핀 나노리본 등이 있다. 우리 그룹은 차세대 재료로 각광받고 있는 탄소나노튜브, 그래핀, 그래핀 나노리본을 빠르고 쉽게 모델링 할 수 있는 프로그램을 만들었다.
[1] http://en.wikipedia.org/wiki/Carbon 

### 탄소 기반 재료[2]
![Aspect ratio](/media/POST/000069/1.jpg)

[2] A. K. Geim & K. S. Novoselov, Nat. Mater. (2007)

### 그래핀(Graphene)[3]
그래핀은 2차원의 허니콤보 격자로 단단하게 묶여있는 탄소 원자로 이루어져있는 평평한 단일층이다. 그래핀은 다른 모든 차원수의 흑연 재료에서 기본이 되는 단위체이다. 그래핀은 흑연과 비슷한 규칙적인 육방정계 안에 배열된 탄소원자로 구성된다. 탄소와 탄소의 결합 길이는 0.1415nm이다.

![Aspect ratio](/media/POST/000069/2.jpg)

[3] http://en.wikipedia.org/wiki/Graphene

![Aspect ratio](/media/POST/000069/3.jpg)

### 그래핀 나노리본(Graphene nano-ribbon)[4]
그래핀 나노리본은 그래핀을 매우 얇은 폭 ( < 50 nm )로 벗겨낸 것이다. 그래핀과 같이 2차원의 허니콤보 격자로 단단하게 묶여있는 탄소 원자로 이루어져있는 평평한 단일 층이지만 유한한 폭을 가지며 그 끝은 수소(H)로 안정화 되어 있다. 뒤에서 언급될 내용이지만, 그래핀 나노리본은 모서리의 모양에 따라서 armchir와 zigzag 형으로 나뉜다.

![Aspect ratio](/media/POST/000069/4.jpg)

 [4] http://en.wikipedia.org/wiki/Graphene_nanoribbons

####[1] Armchair type
모델러를 이용하여 모델링 한 결과이다. 기본 모델은 n=1, m=1이 입력된 첫 번째 모델이다. ‘n’을 증가시키면 x축 방향으로 늘어나고, ‘m’을 증가시키면 y축 방향으로 늘어난다.

![Aspect ratio](/media/POST/000069/5.jpg)

####[2] Zigzag type
모델러를 이용하여 모델링 한 결과이다. 기본 모델은 n=1, m=1이 입력된 첫 번째 모델이다. ‘n’을 증가시키면 x축 방향으로 늘어나고, ‘m’을 증가시키면 y축 방향으로 늘어난다.

![Aspect ratio](/media/POST/000069/6.jpg)

### 탄소나노튜브(Carbon Nano Tube)[5]
CNT는 종횡비가 높은 1차원 나노구조이다. CNT는 소위 말하는 roll-up 벡터 개념($C=na+mb$, $a$ & $b$ = 그래핀에서의 초기 셀 단위 벡터. 그림1 참조)을 사용하여 그래핀(graphene)을 감아서 만들 수 있다. 그러므로 CNT의 모든 성질은 지수쌍 (n,m)에 의해서 독특하게 결정된다. 다음 장의 그림3에 나와있듯이 $(n, 0)$튜브는 zigzag튜브라 하고 $(n, n)$튜브는 armchair튜브라 한다. 또한 일반적인 $(n,m)$튜브는 chiral 튜브라 한다.

![Aspect ratio](/media/POST/000069/7.jpg)

그림 1: 그래핀으로부터 말려진 탄소나노튜브. roll-up 벡터 $C=na+mb$는 그래핀이 말렸을 때 연결되는 점을 명시(빨간색 점)한다. 나노튜브의 축은 T를 따라가고 C는 축에 수직하다. C와 zigzag방향 사이의 각은 chiral 각이라고 부른다.

![Aspect ratio](/media/POST/000069/8.jpg)

그라핀 시트가 말리는 방법은 지수쌍(n,m)으로부터 결정된다. 정수 n과 m은 래핀의 허니컴 결정 격자에서 두 개의 방향을 따르는 단위 벡터의 숫자이다.

[5] http://en.wikipedia.org/wiki/Carbon_nanotube

####Graphene Generator를 사용하여 모델링 한 결과이다.

![Aspect ratio](/media/POST/000069/9.jpg)

##EDISON INPUT Run 설명서
![Aspect ratio](/media/POST/000069/10.jpg)

![Aspect ratio](/media/POST/000069/11.jpg)

![Aspect ratio](/media/POST/000069/12.jpg)

![Aspect ratio](/media/POST/000069/13.jpg)

##소스 코드 (python 언어)
### 그래핀(Graphene)
![Aspect ratio](/media/POST/000069/14.jpg)

Structure에서 환경이 똑같은 점을 표시한 후 연결하면 Unit Cell을 얻을 수 있다. 그리고 Unit Cell은 4개의 모서리 Point와 Basis로 나타낼 수 있다. 즉, basis와 cell정보만 있다면 전체 structrue을 그려낼 수 있는 것이다.

![Aspect ratio](/media/POST/000069/15.jpg)

![Aspect ratio](/media/POST/000069/16.jpg)

atoms package에 있는 AtomSystem class를 이용해서 앞에서 만든 basis의 atom정보를 가져와 n만큼 x축으로 복사한 후 grp list에 저장한다. 그 다음, grp list에 있는 atom정보를 가져와 m만큼 y축으로 복사한 후 grp2 list에 저장한다. 마지막으로 grp2 list에 있는 atom정보를 AtomsSystem class로 종합한다.

![Aspect ratio](/media/POST/000069/17.jpg)

![Aspect ratio](/media/POST/000069/18.jpg)

### 탄소나노튜브(Carbon Nano Tube)
![Aspect ratio](/media/POST/000069/19.jpg)

C-C 결합길이 : 1.415mm
'$a_1$', '$a_2$' 벡터길이 : $a=\sqrt{3}\times 1.415nm=0.246nm$

$C_h$ vector $r$ = $a\times \sqrt{(n^2+nm+m^2)}\div(2\times\pi)$

CNT 둘레 : $c=a\sqrt{n^2+nm+m^2}$

![Aspect ratio](/media/POST/000069/20.jpg)

그래핀나노리본을 만든 후, 이것을 둥글게 말아서 탄소나노튜브를 얻는다. 자세한 내용은 생략한다.

##EDISON 구조 파일 형식 (Structure file format)설명서
###구조 파일 형식 – XYZ 파일 형식
XYZ 파일 형식은 화학적 파일 형식이다. 정규 표현이 없고 몇몇의 변수가 존재하지만 전형적인 XYZ 파일 형식은 데카르트(Cartesian) 좌표와 함께 원자의 개수를 제공하는 분자 기하학을 명시한다. XYZ 파일 형식은 기하학적 구조를 입력하고 추출하는 계산 화학 프로그램에 사용된다. 단위는 일반적으로 Angstroms이다. 몇 가지 변수들은 원자 부호 대신에 원자 번호를 사용하고 comment 줄은 생략한다.

![Aspect ratio](/media/POST/000069/21.jpg)

[1] http://openbabel.org/wiki/XYZ_%28format%29

###구조 파일 형식 – Pdb 파일 형식[2]
![Aspect ratio](/media/POST/000069/22.jpg)

Pdb는 분자들의 삼차원 구조를 표현하는 파일 형식이다. pdb 형 식 은 atomic coordinates, observed sidechain rotamers, secondary structure assignment, atomic connectivity를 포함하는 프로틴(protein)과 nucleic acid 구조들을 표현할 수 있다.

1. HEADER, TITLE and AUTHOR records
구조의 이름과 만든이의 정보를 나타냄.
2. REMARK records
자유 양식의 주석과 규격화된 정보를 수용. 예제에서 REMARK 350 BIOMT 기록은 실험적으로 관찰된 다합체의 coordinate를 어떻게 계산할지 나타냄.
3. SEQRES records
A, B, C의 3가지 펩티트 사슬의 순서를 제공. 예제에서는 매우 짧게 표현했지만 보통 다양한 복합 라인으로 표현.
4. ATOM records
프로틴을 구성하는 원자의 좌표를 나타냄. 예제에서 첫 번째 ATOM라인은 프롤린 잔여물인 펩티드 체인 A의 첫 번째 잔여물의 alpha-N 원자를 나타냄. 처음 3가지 숫자는 x,y,z 좌표 (Angstroms 단위). 다음 3가지 정보는 각각 occupancy, 온도인자, 원자 이름.
5. HEATATM records
프로틴 분자의 구성원이 아닌 헤테로 원자의 좌표를 나타냄.

[2] http://www.wwpdb.org/documentation/format33/sect9.html

###구조 파일 형식 – XSF 파일 형식
![Aspect ratio](/media/POST/000069/23.jpg)

XSF는 내부의 XCrysDen 구조 형식이다. 
- XSF 구성 파일은 다양한 부분으로부터 구성. 
- 각각의 부분은 키워드로 시작.
- 키워드 : 시작 키워드로 시작해서 종료 키워드 없이 끝. 
- 모든 좌표는 ANGSTROMS 단위.
- 모든 힘은 Hartree/ANGSTROM 단위. 
- 주석은 #과 함께 시작.

![Aspect ratio](/media/POST/000069/24.jpg)
