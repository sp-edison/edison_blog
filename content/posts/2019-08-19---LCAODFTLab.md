---
title: "LCAODFTLab"
date: "2019-08-19T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/LCAODFTLab"
category: "Nano Physics"
tags: 
 - "SIESTA"
 - "Atomic orbital basis sets"
 - "DFT"
description: "Linear Combination of Atomic Orbitals 기반 Density Functional Theory 전자구조계산 SW Manual"
---
##LCAODFTLab 배경 이론 및 소프트웨어 설명서
##LCAODFT – Theoretical background
### 다체계의 해밀토니안 $\rightarrow$ 제1원리 전자구조 계산
이온과 전자로 구성된 다체계 시스템의 슈뢰딩거 방정식(The Schrodinger equation)은 다음과 같이 기술된다:
![Aspect ratio](/media/POST/000050/0.jpg)
![Aspect ratio](/media/POST/000050/1.jpg)
![Aspect ratio](/media/POST/000050/2.jpg)
![Aspect ratio](/media/POST/000050/3.jpg)
![Aspect ratio](/media/POST/000050/4.jpg)
![Aspect ratio](/media/POST/000050/5.jpg)

####본-오펜하이머 근사(Born-Oppenheimer approximation)
한편, 계의 파동함수 = 전자부분+이온부분으로 쪼갤 수 있다:

$$
\Psi(r,R)=\psi(r,R) \times \chi(R)
$$

또한 슈뢰딩거방정식도 전자에 관한 식, 이온에 관한 식으로 나뉘는데, 전자는 이온보다 가벼우므로 이온은 멈춰 있고 전자가 빠르게 움직이는 상태로 근사할 수 있다. 이온은 고정되어 있다고 보고, 전자의 고유값 공식(eigenvalue equation)을 풀면된다.

$$
[T_I + V_{II}(R) + \varepsilon_{n}(R)]\chi(R) = E \chi(R)
$$

$$
[T_e + V_{el}(r,R) + V_{ee}(r)]\Psi(r,R) = \varepsilon(R)\Psi(r,R) 
$$

### 전자밀도 범함수 이론(Density Functional Theory, DFT)
![Aspect ratio](/media/POST/000050/6.jpg)

### 의사 포텐셜(Pseudopotential)
$$
\Bigg[-\frac{\hbar^2}{2m}\nabla^2 - V_{ion}(r)+V_{H}(r)+V_{xc}[\rho(r)]\Bigg]\varphi(r)=\varepsilon\varphi(r)
$$

- 원자시스템은 크게 중심(core)과 원자가(valence)의 두가지 부분으로 나뉨.
-  시스템이 반응에 참여하거나 특성을 나타내는 것은 주로 원자가 부분에서 일어나는 일이므로, 핵의 포텐셜을 단순한 쿨롱 포텐셜(Coulomb potential)로 대체할 수 있음.

$V_{ion}(r)$ $\rightarrow$ 의사포텐셜(pseudopotential)

- SIESTA에서는 아래 링크와 같은 형태의 의사 포텐셜을 사용함:

e.g. OCTOPUS pseudopotential: http://www.tddft.org/programs/octopus/pseudo.php
![Aspect ratio](/media/POST/000050/7.jpg)

### 기저 함수(Basis Sets)
$$
\Bigg[-\frac{\hbar^2}{2m}\nabla^2 - V_{ion}(r)+V_{H}(r)+V_{xc}[\rho(r)]\Bigg]\varphi(r)=\varepsilon\varphi(r)
$$

-  슈뢰딩거 방정식(즉, 콘-샴 방정식)을 수치적으로 풀기 위해서는 기저 함수(Basis set)를 도입하여 이를 행렬문제로 변환하게 된다.

![Aspect ratio](/media/POST/000050/8.jpg)

### 기저 함수(Basis Sets): LCAO
$$
\Bigg[-\frac{\hbar^2}{2m}\nabla^2 - V_{ion}(r)+V_{H}(r)+V_{xc}[\rho(r)]\Bigg]\varphi(r)=\varepsilon\varphi(r)
$$

- 슈뢰딩거 방정식(즉, 콘-샴 방정식)을 수치적으로 풀기 위해서는 기저 함수(Basis set)를 도입하여 이를 행렬문제로 변환하게 된다.

![Aspect ratio](/media/POST/000050/9.jpg)

- LCAO는 서로 멀리 떨어진 두 기저함수간의 상호작용을 무시할 수 있으므로 평면파 기저함수에 비해 계산 비용이 줄어드는 장점이 있다. 반면에 더 정확한 기저함수를 선택하는 것이 어려워지는 단점이 발생한다.

### LCAO DFT 계산 순서도
![Aspect ratio](/media/POST/000050/10.jpg)

##SIESTA 소개
### SIESTA software 소개
####- SIESTA hompage: http://icmab.cat/leem/siesta/
####- 고체 시스템에 대한 제1원리 계산 프로그램.
####- 정확도와 비용의 절충 $\rightarrow$ 비교적 큰 시스템을 기술할 수 있음.
####- 무료 프로그램.
####- 주요 특징:
- 밀도 범함수 이론(DFT) 기반.
- 국소 밀도 근사(LDA), 일반 구배 근사(GGA) 포함.
- 의사 포텐셜(pseudopotential) 사용.
- 원자 궤도 함수(atomic orbital) 기저함수(basis set) 사용.

공간적으로 유한한 기저 함수 사용

사용하는 기저 함수의 개수와 모양 조절 가능.
- 직렬/병렬 실행 가능.

####가능한 계산들:
- 힘 (force)
- 응력 텐서 (stress tensor)
- 전자밀도 (electron density)
- 구조 최적화 (geometry optimization)
- 분자 동역학 계산 (molecular dynamics)
- 스핀 분극 계산 (spin-polarized calculation)
- K 포인트 샘플링 (k-sampling of the Brillouin zone)
- 상태 밀도 (local and orbital-projected density of states)
- 화학 결합 분석 (COOP and COHP curves for chemical bonding analysis) 
- 유전체 분극 (dielectric polarization)
- 음향자 계산 (phonons)
- 밴드 구조 계산 (band structure)

##EDISON INPUT Parameters & Run 설명서
![Aspect ratio](/media/POST/000050/11.jpg)

####“Job type” : 사용자의 목적에 따라 수행할 계산의 종류를 지정
- Singlepoint (default) : 주어진 원자구조의 total energy를 도출함. Density Matrix 파일(siesta.DM)를 바탕으로 여러 가지 후속작업(post-processing)이 가능함.
- Optimization : 주어진 원자구조에 작용하는 원자간 힘 (Force) 을 구하여 시스템의 total energy가 낮아지는 방향으로 원자들을 움직여 원자구조를 도출하는 계산
- MD : 분자동력학 (Molecular Dynamics) [1]. 원자간 힘을 얻어 이를 바탕으로 운동방정식을 이용, 시간에 따른 원자구조의 변화를 관찰하는 계산방법

![Aspect ratio](/media/POST/000050/16.jpg)

####“Cell Parameter” : 격자구조의 cell vector의 길이 단위 (Å) 
####“Cell Vector 1/2/3” : 격자구조의 주기적 경계조건을 기술하는 백터 (Cell Parameter 단위)

![Aspect ratio](/media/POST/000050/19.jpg)

####“Structure file” : xyz형식, 원자의 위치정보가 담긴 파일,
![Aspect ratio](/media/POST/000050/20.jpg)
![Aspect ratio](/media/POST/000050/21.jpg)

####“Use external pseudopotential” : 사용자가 직접 유사퍼텐셜 파일을 업로드 후 사용할지 여부
- True : 유사퍼텐셜 파일 업로드 (psf 형식) 
- False : 기본값 사용 (Appendix III 참조)

####“Pseudopotential file(s)” : “Use external pseudopotential”이 True일 경우 활성화, “Structure file” 에 포함된 원자의 종류가 2가지 이상일 경우 각 psf 파일을 하나의 파일로 합쳐 업로드 (17페이지 참조)

####“k-sampling” : Monkhorst-Pack [2] 방법을 사용한 역공간의 차분화

####“XC functional” : XC 분류-저자 지정
- local density approximation (Ceperley-Aldr) [3] 
- generalized gradients approximation (Perdew-Burke-Ernzerhof) [4]

####“Spin polarization” : 스핀의 자유도를 고려한 전자구조 계산 수행 여부

####“Post processing” : 전자구조 도출 후 추가적으로 수행할 작업 선택, “Job type”이 Singlepoint일 경우 활성화
- None
- Band : 지정된 역공간상 경로에서 전자의 고유에너지값 도출

####“Bandline filename” : Band구조 도출을 위한 역공간상 경로가 지정된 파일 (fdf 형식), “Post processing”이 Band일 경우 활성화

####“Advanced”
- (1) Optimization / MD 수행시 세부 옵션 조절
- (2) SIESTA가 지원하는 다른 옵션들을 사용자가 직접 fdf 형식으로 “Advenced filename” 에 업로드

예) Mixing weight 조절, 사용자 정의 Basis set 적용

![Aspect ratio](/media/POST/000050/12.jpg)

####“Advenced” 옵션 활성화 후 예제와 같이 Optimization, MD를 위한 옵션이 들어있는 파일을 업로드 (19페이지, SIESTA3.1 매뉴얼 78 페이지 참조)
![Aspect ratio](/media/POST/000050/17.jpg)
![Aspect ratio](/media/POST/000050/18.jpg)


![Aspect ratio](/media/POST/000050/13.jpg)

![Aspect ratio](/media/POST/000050/14.jpg)

![Aspect ratio](/media/POST/000050/15.jpg)

##Case Study
### 1. Single point 에너지 계산: 간단한 분자
####1. xyz형식 구조파일 준비 & 업로드 (필수)
![Aspect ratio](/media/POST/000050/22.jpg)
####2. Download results
####3. siesta.OUT: SCF loop $\rightarrow$ Total energy
![Aspect ratio](/media/POST/000050/23.jpg)

![Aspect ratio](/media/POST/000050/24.jpg)

![Aspect ratio](/media/POST/000050/25.jpg)

![Aspect ratio](/media/POST/000050/26.jpg)

![Aspect ratio](/media/POST/000050/27.jpg)

### 2. Single point + 밴드계산: 2차원 그래핀
####1. xyz형식 구조파일 준비 & 업로드 (필수)
![Aspect ratio](/media/POST/000050/28.jpg)

####2. Cell paramter & cell vector 지정
![Aspect ratio](/media/POST/000050/29.jpg)
![Aspect ratio](/media/POST/000050/30.jpg)

####3. Post processing 선택
![Aspect ratio](/media/POST/000050/31.jpg)

####4. Bandlines 파일 업로드 $\rightarrow$ Launch Simulation
![Aspect ratio](/media/POST/000050/32.jpg)
![Aspect ratio](/media/POST/000050/33.jpg)

##EDISON INPUT Parameters & SIESTA 해당 명령어
### Appendix I: SIESTA 해당 명령어
![Aspect ratio](/media/POST/000050/34.jpg)

![Aspect ratio](/media/POST/000050/35.jpg)

##EDISON 구조 파일 형식 (Structure file format)설명서
### Appendix II: 구조 파일 형식 – XYZ 파일 형식 [5]
 XYZ 파일 형식은 화학적 파일 형식이다. 정규 표현이 없고 몇몇의 변수가 존재하지만 전형적인 XYZ 파일 형식은 데카르트(Cartesian) 좌표와 함께 원자의 개수를 제공하는 분자 기하학을 명시한다. XYZ 파일 형식은 기하학적 구조를 입력하고 추출하는 계산 화학 프로그램에 사용된다. 단위는 일반적으로 Ångströms이다. 몇 가지 변수들은 원자 부호 대신에 원자 번호를 사용하고 comment 줄은 생략한다.
![Aspect ratio](/media/POST/000050/36.jpg)
![Aspect ratio](/media/POST/000050/37.jpg)

####※ 현재 EDISON에서는 XYZ 형식만 지원됨

### Appendix II: 구조 파일 형식 – PDB 파일 형식 [6]
PDB는 분자들의 삼차원 구조를 표현하는 파일 형식이다. pdb 형 식 은 atomic coordinates, observed sidechain rotamers, secondary structure assignment, atomic connectivity를 포함 하는 프로틴(protein)과 nucleic acid 구조들을 표현할 수 있다.
![Aspect ratio](/media/POST/000050/38.jpg)
####(1) HEADER, TITLE and AUTHOR records
구조의 이름과 만든이에 대한 정보를 나타냄.
####(2) REMARK records
자유 양식의 주석과 규격화된 정보를 수용. 예제에서 REMARK 350 BIOMT 기록은 실험적으로 관찰된 다합체의 coordinate를 어떻게 계산할지 나타냄.
####(3) SEQRES records
A, B, C의 3가지 펩티드 사슬의 순서를 제공. 예제에서는 매우 짧게 표현했지만 보통 다양한 복합라인으로 표현.
####(4) ATOM records
프로틴을 구성하는 원자의 좌표를 나타냄. 예제에서 첫 번째 ATOM라인은 프롤린 잔여물인 펩티드 체인 A의 첫 번째 잔여 물의 alpha-N 원자를 나타냄. 처음 3가지 숫자는 x, y, z 좌표 (Ångströms 단위). 다음 3가지 정보는 각각 occupancy, 온도 인자, 원자 이름.
####(5) HETATM records
프로틴 분자의 구성원이 아닌 헤테로 원자의 좌표를 나타냄.

### Appendix II: 구조 파일 형식 – XSF 파일 형식 [7]
XSF는 내부의 XCrysDen 구조 형식이다.
- XSF 구성 파일은 다양한 부분으로부터 구성. 
- 각각의 부분은 키워드로 시작.
- 키워드 : 시작 키워드로 시작해서 종료 키워드 없이 끝. 
- 모든 좌표는 ANGSTROMS 단위.
- 모든 힘은 Hartree/ANGSTROM 단위.  
- 주석은 #과 함께 시작.

![Aspect ratio](/media/POST/000050/39.jpg)
![Aspect ratio](/media/POST/000050/40.jpg)

##기본 제공 유사퍼텐셜 목록
### 기본 제공 유사퍼텐셜 목록: LDA [8]
![Aspect ratio](/media/POST/000050/41.jpg)

### 기본 제공 유사퍼텐셜 목록: GGA [8]
![Aspect ratio](/media/POST/000050/42.jpg)
