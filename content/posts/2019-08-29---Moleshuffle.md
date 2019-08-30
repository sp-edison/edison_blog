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
####1. Parameter Section
At least one space between parameter name and its value is required. The order of parameters should not be changed. If one parameter contains multiple values, they are distinguished by spaces between them.

- $\mathbf{Supermolecule}$ $\mathbf{charge}$ : Net charge of the supermolecule containing all fragments used in basin-hopping sampling. Specified as integer.

- $\mathbf{Supermolecule}$ $\mathbf{mult}$ : Spin multiplicity the supermolecule containing all fragments used in basin-hopping sampling. Specified as integer.

- $\mathbf{Fragments}$ : Names of fragments used in basin-hopping sampling. The names should be same with those specified in geometry section at the bottom of the input file. If the same fragments are used multiple times, the name should be duplicated.

- $\mathbf{Fragments}$ $\mathbf{charge}$ : Charges of each fragment. The number of arguments should be same with that of fragments. Specified as integer.

- $\mathbf{Fragments}$ $\mathbf{mult}$ : Multiplicity values of each fragment. The number of arguments should be same with that of fragments. Specified as integer.

- $\mathbf{Bond}$ $\mathbf{Rotation}$ : Y/N. If specified as ‘Y’, the bonds written at the rotatable bond section are rotated randomly at each basin-hopping sampling cycle.

- $\mathbf{Interatomic}$ $\mathbf{crit}$ $\mathbf{coeff}$ : Typically, the value between 0.6~1.3 is used. At each cycle, fragments are arranged with random orientations and they are optimized. First they are overlapped with each other and separated with random directions until satisfying the toughness criterion: $r_{ij}>c_{tough}(R_i+R_j)$, where $r_{ij}$ is distance ij tough i j between atom i and j, $R_i$ and $R_j$ are covalent radii of atom i and j which belong to different fragments. Ctough value is taken from this parameter. The smaller the Ctough value, the higher probability the fragments react and form new bonds. If one wants to find noncovalent interactions among fragments, larger value is better.

- $\mathbf{Reactants}$ : Names of initial reactants. The names should be same with those specified in geometry section at the bottom of the input file. Their energies are used for comparing stabilities of sampled intermediates.

- $\mathbf{Reactants}$ $\mathbf{charge}$ : Charges of each reactant molecule. The number of arguments should be same with that of reactants. Specified as integer.

- $\mathbf{Reactants}$ $\mathbf{mult}$ : Multiplicity values of each reactant molecule. The number of arguments should be same with that of reactants. Specified as integer.

- $\mathbf{Num}$ $\mathbf{Epoints}$ : The number of basin-hopping sampling cycles.

- $\mathbf{Barrier(kcal/mol)}$ : If the energy of the sampled intermediate is larger than that of reactant energy plus the barrier value, this intermediate is regarded as unstable and deleted.

- $\mathbf{GMS_MaxOptCycle}$ : The GAMESS package is used for geometry optimization in each sampling. The maximum number of optimization cycles in GAMESS.

- $\mathbf{ConsiderTerminalHydrogens}$ : Specified as two binary integers. When applying the $\mathbf{NoFragmentBondBreak}$ and $\mathbf{ReactionShouldOccur}$ criteria for intermediate sampling, it specifies whether to consider terminal hydrogens together (1) or not (0).

- $\mathbf{NoOvervalency}$ : During intermediate screening, whether to allow overcoordinated atoms (1) or not (0). (Maximum coordination of C : 4, H: 1, O : 3)

- $\mathbf{NoFragmentBondBreak}$ : During intermediate screening, whether to allow broken fragment (1) or not (0). It is specified for EACH fragment. The number of binary integers should be same with that of fragments.

- $\mathbf{ReactionShouldOccur}$ : During intermediate screening, whether to allow intermediates whose connectivity is same with that of initial fragments(1) or not (0). If one wants to find only noncovalent interactions among fragments (ex. Hydrogen bonds), this value can be set to 0.

- $\mathbf{Temperature}$ : The temperature value (in Kelvin) used in applying the Metropolis criterion. The higher the temperature, the higher probability the unstable intermediates are accepted.

- $\mathbf{GMS}$ $\mathbf{semiempirical}$ : The name of the semiempirical model used in GAMESS geometry optimization and energy calculations. MNDO/AM1/RM1/PM3 can be used. The available atoms of each model are:

MNDO:H,Li,B,C,N,O,F,Na,Al,Si,P,S,Cl,K,Zn,Ge,Br,Rb,Sn,I, Hg, Pb.

AM1:H,B,C,N,O,F,Na,Mg,Al,Si,P,S,Cl,K,Ca,Zn,Ge,Br,Rb,Sn, I, Hg.

RM1 : All atoms of AM1 with H, C, N, O, F, P, S, Cl, Br, I revised.

PM3:H, Li, Be, C, N, O, Na, Mg, Al, Si, P, S, K, Ca, Zn, Ga, Ge, As, Se, Rb, Cd, In, Sn, Sb, Te, Hg, Tl, Pb, Bi.

####2. Rotatable bond section
The parameter section and the rotatable bond section are separated by one blank line. If the $\mathbf{Bond}$ $\mathbf{Rotation}$ parameter is specified as ‘Y’, the rotatable bonds are written at this section.

First the number of fragment is written, and the indices of four atoms specifying dihedral angles are written at the next lines. Multiple dihedral angles for multiple fragments also can be specified. This section should be ended with the ‘END’ line.

For example, the lines ‘1’ and ‘8 3 1 5’ are written in the input example. That means the atom 3 – atom 1 bond of fragment 1 (the ‘gly’ fragment) are randomly rotated by changing the dihedral angle defined by four atoms of indices 8, 3, 1, and 5.

####3. Geometry section
The rotatable bond section and the geometry section are separated by one blank line. For each fragment and reactant, Cartesian geometry should be provided in xyz file format. The geometry specification begins with the line ‘Filename: ###.xyz’. The name ‘###’ should be same with that of fragments and reactants specified at the parameter section.

The xyz format geometry is provided, and each geometry is ended with the ‘%%%%’ line. Note that xyz geometries of all the reactants and fragments should be included in the geometry section.

The geometry section is also ended with the ‘END’ line.

##3 Examples
These are simple examples of intermediates/local minima obtained through this basin- hopping sampling program.

####1. $H_2O$ hexamer
![Aspect ratio](/media/POST/000071/1.jpg)

####2. 1,3-butadiene + HCl
![Aspect ratio](/media/POST/000071/2.jpg)

####3. iPr-OSO2CH3 + CH3O- SN2 or E2 reaction
![Aspect ratio](/media/POST/000071/3.jpg)
![Aspect ratio](/media/POST/000071/4.jpg)

####4. Intramolecular aldol reaction of heptane-2,6-dione
![Aspect ratio](/media/POST/000071/5.jpg)
