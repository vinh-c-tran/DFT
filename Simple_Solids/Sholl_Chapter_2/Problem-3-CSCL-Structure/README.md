# Sholl Ch2. Problem 3

## Problem Statement 
A large number of solids with stoichiometry AB form the CsCl structure. In this structure, atoms of A define a simple cubic structure and atoms of B reside in the center of each cube of A atoms. Define the cell vectors
and fractional coordinates for the CsCl structure, then use this structure to predict the lattice constant of ScAl.

## Problem Learning Goals
 - How to create a structure with more than one type of atom in its unit cell 
 - Lattice constant optimization 

## Quantum Espresso 
We perform the optimization in a "standard" way where we define an array of different lattice constant values and loop over them, run our quantum espresso code, and write out the data points. 

For the construction of the CsCl structure, we define the unit cell as a SC cell using `ibrav = 1` and then manually place an `Al` atom at the center of the cube. So this is like a BCC structure with the middle atom replaced by a different type of atom. 

```fortran 
&CONTROL 
    calculation = 'scf' 
    prefix = 'ScAl' 
    outdir = './temp' 
    pseudo_dir = '/Users/vinhtran/Documents/GitHub/DFT/pseudos' 
/ 
&SYSTEM
    ibrav = 1 
    celldm(1) = 5.0 
    nat = 2 
    ntyp = 2 

    ecutwfc = 30.0 

    occupations = 'smearing' 
    degauss = 0.02
    smearing = 'gauss' 
/
&ELECTRONS 
/ 
&IONS
/
ATOMIC_SPECIES
    Sc  44.955908   Sc_ONCV_PBE-1.0.oncvpsp.upf
    Al  26.981538   Al.pbe-n-kjpaw_psl.1.0.0.UPF
ATOMIC_POSITIONS alat 
    Sc  0.00    0.00    0.00 
    Al  0.50    0.50    0.50 
K_POINTS automatic 
    2   2   2   0   0   0 

``` 
