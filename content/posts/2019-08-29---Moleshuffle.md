---
title: "Moleshuffle"
date: "2019-08-29T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Moleshuffle"
category: "Computational Chemistry"
tags: 
 - "Basin-hopping"
 - "Potential energy surface"
 - "local minimum sampling"
description: "Monte Carlo basin-hopping을 이용한 화학 반응의 중간체 샘플링 프로그램 Manual"
---
##Reaction Intermediate Sampling Program using Monte Carlo Basin-hopping Manual
###1 Input example
![Aspect ratio](/media/POST/000071/0.jpg)

###2 Detailed description of input file
1. Parameter Section
At least one space between parameter name and its value is required. The order of parameters should not be changed. If one parameter contains multiple values, they are distinguished by spaces between them.

    - Supermolecule_charge : Net charge of the supermolecule containing all fragments used in basin-hopping sampling. Specified as integer.

    - Supermolecule_mult : Spin multiplicity the supermolecule containing all fragments used in basin-hopping sampling. Specified as integer.

    - Fragments : Names of fragments used in basin-hopping sampling. The names should be same with those specified in geometry section at the bottom of the input file. If the same fragments are used multiple times, the name should be duplicated.

    - Fragments_charge : Charges of each fragment. The number of arguments should be same with that of fragments. Specified as integer.

    - Fragments_mult : Multiplicity values of each fragment. The number of arguments should be same with that of fragments. Specified as integer.

    - Bond_Rotation : Y/N. If specified as ‘Y’, the bonds written at the rotatable bond section are rotated randomly at each basin-hopping sampling cycle.

    - Interatomic_crit_coeff : Typically, the value between 0.6~1.3 is used. At each cycle, fragments are arranged with random orientations and they are optimized. First they are overlapped with each other and separated with random directions until satisfying the toughness criterion: $r_{ij}>c_{tough}(R_i+R_j)$, where $r_{ij}$ is distance ij tough i j between atom i and j, $R_i$ and $R_j$ are covalent radii of atom i and j which belong to different fragments. Ctough value is taken from this parameter. The smaller the Ctough value, the higher probability the fragments react and form new bonds. If one wants to find noncovalent interactions among fragments, larger value is better.

    - Reactants : Names of initial reactants. The names should be same with those specified in geometry section at the bottom of the input file. Their energies are used for comparing stabilities of sampled intermediates.

    - Reactants_charge : Charges of each reactant molecule. The number of arguments should be same with that of reactants. Specified as integer.

