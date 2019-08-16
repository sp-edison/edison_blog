---
title: "Single_Cell_Electrophysiology"
date: "2019-08-16T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Single_Cell_Electrophysiology"
category: "Computational Medicine"
tags: 
 - "Single cell simulation"
 - "Ten Tusscher model"
description: "부정맥기전 교육용 SW Manual"
---
##Simulation Manual
![Aspect ratio](/media/POST/000046/0.jpg)

![Aspect ratio](/media/POST/000046/1.jpg)

![Aspect ratio](/media/POST/000046/2.jpg)

![Aspect ratio](/media/POST/000046/3.jpg)

![Aspect ratio](/media/POST/000046/4.jpg)

##Input Parameter
![Aspect ratio](/media/POST/000046/5.jpg)

1. ion Model
![Aspect ratio](/media/POST/000046/6.jpg)

2. variant
![Aspect ratio](/media/POST/000046/7.jpg)

![Aspect ratio](/media/POST/000046/9.jpg)

3. celltype
![Aspect ratio](/media/POST/000046/8.jpg)

![Aspect ratio](/media/POST/000046/10.jpg)

4. apdslop
![Aspect ratio](/media/POST/000046/11.jpg)

####Action potential shape
- ibclb (BCL의 초기값) < ibcle (BCL의 마지막 값)으로 설정 
- 전체 시뮬레이션 시간 (tmax) = ibclb X irepeat 

![Aspect ratio](/media/POST/000046/12.jpg)

####Action potential duration restitution (APDR) curve
- ibclb (BCL의 초기값) > ibcle (BCL의 마지막 값)으로 설정
- ibcld: BCL의 감소 정도 (ibcld=20: 20ms씩 감소시킴)
- 전체 시뮬레이션 시간 (tmax)
= ibclb × irepeat + (ibclb-ibcld) × irepeat +
((ibclb-ibcld)-ibcld) × irepeat + ... + ibcle × irepeat

![Aspect ratio](/media/POST/000046/13.jpg)
