# Graphene Density of States
Graphene is a 2D arrangement of carbon atoms in a honeycomb lattice, or in other words, in an overlapping of two hexagonal/triangular lattices with two carbon atoms in its unit cell. It has a few significant electronic properties including a linear energy dispersion relation around the Dirac points. We want to try to find these electronic properties. 

## Quantum Espresso
To perform a density of states calculation in quantum espresso, we need to perform a series of calculations. 
1. Begin with a `pw.x` self consistent field (SCF) calculation (call `calculation = 'scf'`). 
2. Follow with a `pw.x` non-SCF calculation (set `calculation = 'nscf'`) (with a denser *k*-point grid). 
3. Use `dos.x` for post-processing. 


## First Step: `pw.x` SCF Calculation
We begin with an `scf` calculation. For this we use the following input file for quantum espresso. Note that graphene, a two-dimensional structure, we define 
```fortran
 &CONTROL
    calculation = 'scf',
    prefix      = 'Graphene_1x1_PBE',
    ! otudir      = '/tmp',
    !pseudo_dir  = 'directory with pseudopotentials',        
 /

 &SYSTEM
    ibrav     = 4,
    celldm(1) = 4.654,
    celldm(3) = 3.0,
    nat  = 2,
    ntyp = 1,
    ecutwfc = 20.0,
    ecutrho = 200.0, 

!    occupations = 'smearing',
!    smearing = 'gaussian',
!    degauss = 0.01,
 /
 
 &ELECTRONS
    conv_thr = 1.0d-8
 /
 
ATOMIC_SPECIES
   C  12.0107 C.pbe-rrkjus.UPF
   
ATOMIC_POSITIONS alat
   C    0.000000    0.0000000   0.000000
   C    0.000000    0.5773503   0.000000
   
K_POINTS automatic
   9 9 1   0 0 0
``` 
