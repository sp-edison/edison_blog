---
title: "OPT_PROPELLER"
date: "2019-08-07T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/OPT_PROPELLER"
category: "Computational Design"
tags: 
 - "Latin Hypercube Design"
 - "Reynolds"
description: "Latin Hypercube Design을 활용한 프로펠러 성능 추정식 기반의 프로펠러 형상 최적화 프로그램 Manual"
---
##Input 파일 입력
![Aspect ratio](/media/POST/000024/0.jpg)

총 4개의 파라미터를 입력받고. output 인자는 pd, aeao, j로 총 3개입니다.

####[Parameter 1 : num_of_points]
- [타입 : integer]
- [범위 : 0 ~ $2^{32}$]

이는 Latin hypercube상에서 찍힐 점의 개수로, 만약 값이 50이라면, 아웃풋 인자가 3개이므로, 3차원 cubic상에서
50개의 점이 찍힙니다. 이 때 각 점들은 같은 축상에서 서로 같은 값을 가지지 않으며, 점들 사이의 평균 거리가 최소 가 되도록 분포합니다.

####[Parameter 2 : downscaling_rate_of_boundary] 
- [타입 : float]
- [범위 : 0.0~1.0]

boundary가 축소되는 비율로, 프로그램 내부적으로 pd, aeao, j 3개의 아웃풋 값에 대해 boundary가 설정되어 있 는데, 최적화를 수행하는 과정에서 boundary가 점점 좁혀집니다. 이 좁혀지는 비율을 나타내는 값입니다.

ex) 값이 0.8이면 boundary가 축소될 경우가 생길 때 기존의 80%로 축소됩니다. 즉, 1에 가까울수록 결과값이 정확해지고 0에 가까울수록 수행시간이 짧아집니다.

####[Parameter 3 : downscaling_rate_of_#_of_point] 
- [타입 : float]
- [범위 : 0.0~1.0]

Parameter 2와 같이, boundary가 축소될 경우가 생길 때 찍을 점의 개수를 얼마나 줄일 것인지에 대한 파라미터입 니다.

ex) 값이 0.7이면 boundary가 축소될 경우가 생길 때 그 boundary내에서 찍을 점의 개수는 최초 num_of_points부 터 시작해서 그것의 70%에 해당하는 값으로 계속해서 줄어듭니다. 즉, 1에 가까울수록 결과값이 정확해지고 0에 가까울수록 수행시간이 짧아집니다.


####[Parameter 4 : Reynolds_Number] 
- [타입 : double]
- [범위 : 0~]

시뮬레이션을 수행할 환경에서의 레이놀즈 수로, 2,000,000을 기준으로 이보다 작을 때와 클 때의 계산식이 다릅니다.

![Aspect ratio](/media/POST/000024/1.jpg)

실행 시에는 다음과 같이 입력합니다.

./OPT_PROPELLER –inp [FILENAME]

-inp 파라미터가 없을 경우 디폴트로 input.txt 파일이 입력됩니다.

프로그램이 성공적으로 종료되면 result 폴더에 output.txt 파일이 생성됩니다.

![Aspect ratio](/media/POST/000024/2.jpg)

위쪽에는 앞서 입력했던 input data가 그 아래에는 프로펠러가 최적의 효율을 낼 수 있는 pd, aeao, j값이 나타납니다. 

맨 아래에는 해당 pd, aeao, j값을 함수식에 대입했을 때 나타나는 값입니다.

##Simulation Manual
![Aspect ratio](/media/POST/000024/3.jpg)

![Aspect ratio](/media/POST/000024/4.jpg)

![Aspect ratio](/media/POST/000024/5.jpg)

![Aspect ratio](/media/POST/000024/6.jpg)
