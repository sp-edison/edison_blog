---
title: "SNUFOAM_ShipRes_freesurf_ADV"
date: "2019-08-08T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/SNUFOAM_ShipRes_freesurf_ADV"
category: "Computational Fluid Dynamics"
tags: 
 - "PRAGMA"
 - "Ship Resistance"
description: "SNUFOAM Ship Resistance with Free-surface for EDISON ADVANCED Manual"
---
##OpenFoam tutorial1 : SNUFOAM_Ship_Resistane_with_Free-surface_for_EDISON_ADVANCED

###해석자 소개 및 사용방법
####RANS(Reynolds Averaged Navier-Stokes) 방정식을 지배방정식으로 하며 유한체적법(Finite Volume Method, FVM)을 바탕으로 유동해석을 수행하는 것을 목표로 함
####선박에서 자유수면 상의 유동특성 및 저항성능을 추정할 수 있음
####시뮬레이션에 사용된 해석자 : LTSInterFoam
####본 Tutorial을 통해 삼동선 후류의 자유 수면파계 간섭 및 저항 성능 추정을 목표로 함
- 삼동선에 걸리는 저항과 삼동순 선미의 후류에서 생기는 파들의 간섭효과를 볼 수 있음
- 더 나아가 주선체와 보조 선체의 간격 변화 및 선속 변화에 따른 조파 저항 감소 현상 연구에 사용될 수 있음

![Aspect ratio](/media/POST/000027/0.jpg)

####edShipFS 폴더 구조 
- 하위폴더구성

![Aspect ratio](/media/POST/000027/1.jpg)

- 0 : 변수들의초기조건
- Constant : 해석하는 공간의 격자 정보가 polyMesh라는 폴더에 들어가 있고 해석자에 사용되는 물리량들([ex] 𝜈, 𝑔, 𝜌)을 정의하는 파일들로 구성
- System : 시뮬레이션 설정들을 정의하는 파일들로 구성되어있고 최소한 controlDict, fvSolution, fvSchemes 파일들을 무조건 포함하고 있음

![Aspect ratio](/media/POST/000027/2.jpg)

polyMesh : 해석하는 controlvolume의 mesh정보가 위치함
- 격자 정보에 대한 Mesh 파일은 개인적으로 준비
- OpenFOAM에 맞도록 변환해야함 ( Fluent mesh파일의 경우 fluent3DMeshToFoam xxx.msh 이런식으로 변환, 변환 명령어는 openfoamwiki에서 검색 가능)

![Aspect ratio](/media/POST/000027/3.jpg)

controlDict : 시뮬레이션 시작시간, 끝 시간, 시간간격, 데이터 입력 시간 등을 조정할 수 있음 
- startFrom : 시작 시간 설정
- endTime : 끝 시간 설정
- deltaT : 시간 간격 조정
- writeinterval : 데이터 입력 간격 (만약 100이라면 100초 간격이 아니라 100 스텝 마다 입력)

![Aspect ratio](/media/POST/000027/4.jpg)

setFieldsDict : 해석 공간 상의 특정 영역의 물리적 상수 값을 지정
- 일반적으로 초기 상수 값 같은 경우 0폴더에 지정할 수 있음
- 하지만 본 예제처럼 선박의 형상이 복잡하여 초기에 그 값을 설정하지 못할 경우 setFieldsDict를 통해 일관적으 로 특정 영역의 값을 지정할 수 있음
- 본 연구에서는 alpha (VOF 기법에서 volume fraction)을 setFieldsDict에서 설정
- 본 예제에서는 자유수면의 z값이 0이 되도록 setFieldsDict에 설정이 되어있으므로 격자를 구성할 때 이를 유의해 야함

####Tutorial 실행 방법
- 먼저 준비된 격자 파일을 OpenFOAM 형식으로 변환 →fluent3DMeshToFoam xxxxxxx.msh
- 변환 된 격자 관련 파일들을 constant/polyMesh 폴더에 넣음
- Alpha값을 수선면 이하는 1 그 위는 0으로 설정하기 위해 setFieldsDict 파일을 돌려야함 → setFields
- 마지막으로 시뮬레이션 해석자를 돌리기 위해서 다음과 같은 명령어를 입력한다 →LTSInterFoam

####결과
- postProcessing 폴더에 있는 파일을 통해 저항값 확인 가능
- 후처리 프로그램 (ex paraview)를 이용하여 선체 주위 유동 확인 가능
- Postprocessing 폴더 내부의 force폴더에서 선체에 걸리는 힘 및 모멘트를 확인할 수 잇음

![Aspect ratio](/media/POST/000027/5.jpg)

## EDISON 사용법: SNUFOAM_ShipRes
![Aspect ratio](/media/POST/000027/6.jpg)

![Aspect ratio](/media/POST/000027/7.jpg)

![Aspect ratio](/media/POST/000027/8.jpg)

![Aspect ratio](/media/POST/000027/9.jpg)
