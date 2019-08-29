---
title: "Waveguide"
date: "2019-08-29T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Waveguide"
category: "Nano Physics"
tags: 
 - "TE mode"
 - "TM mode"
 - "cutoff frequency"
description: "Waveguide Manual"
---
##Rectangular waveguides
## Realistic case
###Rectangular waveguides are standard.
###What is the aspect ratio of rectangular waveguides?
The interior of a standard rectangular wave guide has a 2:1 aspect ratio in most cases.

![Aspect ratio](/media/POST/000067/0.jpg)

##Equations
###Eigenvalue equations, defined on xy-plane.
- TM mode
$$
\frac{\partial^2E_z^0(x,y)}{\partial x^2}+\frac{\partial^2E_z^0(x,y)}{\partial y^2}+h^2E_z^0(x,y)=0
$$
- TE mode
$$
\frac{\partial^2H_z^0(x,y)}{\partial x^2}+\frac{\partial^2H_z^0(x,y)}{\partial y^2}+h^2H_z^0(x,y)=0
$$

###Boundary conditions
- Upper/lower plates
- Left/right plates

##Exercise
###In order to enhance your understanding on the subject,
A waveguide simulator has been developed.

Using the web browser, please visit https://nano.edison.re.kr

![Aspect ratio](/media/POST/000067/1.jpg)

![Aspect ratio](/media/POST/000067/2.jpg)

![Aspect ratio](/media/POST/000067/3.jpg)

![Aspect ratio](/media/POST/000067/4.jpg)

##WG16
###It has 𝑎 = 2.29 cm and 𝑏 = 1.02 cm
- 25% margin for the lower bound
- 5% margin for the upper bound
- The lowest cutoff frequency is 6.55 GHz.
- The second lowest cutoff frequency is 13.10 GHz. (From what? TE20 mode.)

## Origin of different modes
###Now, select “ImageViewer” tool.
You will see the modes in the cross section.

##Analogy
###Infinite potential well problem
Inside 2D well,
$$
-\frac{\hbar^2}{2m}\Bigg(\frac{\partial^2}{\partial x^2}+\frac{\partial^2}{\partial y^2}\Bigg)\Phi(x,y)=E\Phi(x,y)
$$

Yes, this Schrödinger equation has the same form.

The solution has the form of 
$$
\Phi(x,y)=A_{mn}\sin\Bigg(\frac{m\pi}{a}x \Bigg)\sin\Bigg(\frac{n\pi}{b}y \Bigg)
$$

![Aspect ratio](/media/POST/000067/5.jpg)

## Ground state = dominant
###From this analogy,
The ground state (whose energy is lowest) is corresponding to the dominant mode (whose cutoff frequency is lowest).

The energy of an eigenmode $(m,n)$ is given by
$$
E_{mn}=\frac{\hbar^2}{2m}\Bigg(\frac{m\pi}{a} \Bigg)^2+\frac{\hbar^2}{2m}\Bigg(\frac{n\pi}{b} \Bigg)^2
$$

Of course, $m \neq 0$ and $n \neq 0$.(No particle at all)

The term related with the long side, $\frac{\hbar^2}{2m}(\frac{m\pi}{a})^2$, generates smaller contribution to $E_{mn}$, because that $\frac{1}{a} < \frac{1}{b}$.

##TM solution
###Back to our problem
- Electric field:
$$
E_z^0(x,y)=E_0\sin\Bigg(\frac{m\pi}{a}x \Bigg)\sin\Bigg(\frac{n\pi}{b}y \Bigg)\\
h^2=\Bigg(\frac{m\pi}{a} \Bigg)^2+\Bigg(\frac{n\pi}{b}\Bigg)^2
$$
- Cutoff frequency
$$
f_c=\frac{1}{2\sqrt{\mu\epsilon}}\sqrt{\Bigg(\frac{m}{a}\Bigg)^2+\Bigg(\frac{n}{b}\Bigg)^2}
$$

## Dominant TM mode, TM
###Since $m\neq 0$ and $n\neq 0$,
The dominant TM mode becomes $TM_{11}$ mode.

Cutoff frequency:
$$
f_c=\frac{1}{2\sqrt{\mu\epsilon}}\sqrt{\Bigg(\frac{1}{a}\Bigg)^2+\Bigg(\frac{1}{b}\Bigg)^2}
$$

##TE solution
###Only the boundary conditions are different!
- Magnetic field:
$$
H_z^0(x,y)=H_c\cos{\Bigg(\frac{m\pi}{a}x\Bigg)}\cos{\Bigg(\frac{n\pi}{b}y\Bigg)}\\
h^2=\Bigg(\frac{m\pi}{a}\Bigg)^2+\Bigg(\frac{n\pi}{b}\Bigg)^2
$$
- Cutoff frequency: 
$$
f_c=\frac{1}{2\sqrt{\mu\epsilon}}\sqrt{\Bigg(\frac{m}{a}\Bigg)^2+\Bigg(\frac{n}{b}\Bigg)^2}
$$
Same with TM mode

## Dominant TE mode, TE
###Since one of 𝑚 and 𝑛 may be zero,
The dominant TE mode becomes $TE_{10}$ mode.

(Why TE10 instead of TE01?)

Cutoff frequency:
$$
f_c = \frac{1}{2\sqrt{\mu\epsilon}}\sqrt{\Bigg(\frac{1}{a}\Bigg)^2} < \frac{1}{2\sqrt{\mu\epsilon}}\sqrt{\Bigg(\frac{1}{a}\Bigg)^2+\Bigg(\frac{1}{b}\Bigg)^2}
$$

Therefore $TE_{10}$ mode is the dominant mode of a rectangular waveguide.

## Things to be checked
###Let’s check the followings!
- In the rectangular waveguide, $TM_{00}$ mode (or $TM_{10}$ or $TM_{01}$) is not allowed. Therefore, the TEM mode cannot be the dominant mode.

- For given $(m,n)$, the cutoff frequency is the same. However, since the lowest allowed pair is different between two cases, the $TE_{10}$ mode becomes the dominant mode.

## Homework
###Consider a standard waveguide.

###Specification:
- Calculate the recommended frequency range. (25% lower margin, 5% upper margin)
- Change the aspect ratio of your waveguide by 20% longer a and 20% shorter a.
- Discuss about the change of the frequency range.
- (Total 3 simulations at least!)
