# Structural Optimization - Relax
Structural relaxation of atomic positions only 


## Quantum Espresso 
```fortran 
 &CONTROL
    calculation = 'relax',
    prefix      = 'Graphane',
    outdir      = './tmp',
    pseudo_dir  = '/Users/vinhtran/Documents/GitHub/DFT/pseudos',        
 /

 &SYSTEM
    ibrav     = 4,
    celldm(1) = 4.654,
    celldm(3) = 3.0,
    nat  = 4,
    ntyp = 2,
    ecutwfc = 20.0,
    ecutrho = 200.0, 

!    occupations = 'smearing',
!    smearing = 'm-v',
!    degauss = 0.01,
 /
 
 &ELECTRONS
    conv_thr = 1.0d-8
 /

 &IONS
    upscale = 100.0
 /
 
ATOMIC_SPECIES
   C  12.0107 C.pbe-n-kjpaw_psl.1.0.0.UPF
   H  1.00007 H.pbe-rrkjus_psl.1.0.0.UPF

ATOMIC_POSITIONS alat
   H    0.000000    0.0000000   0.400000
   C    0.000000    0.0000000   0.000000
   C    0.000000    0.5773503   0.000000
   H    0.000000    0.5773503  -0.400000
   
K_POINTS automatic
   9 9 1   0 0 0
```
