# Graphene Density of States
Graphene is a 2D arrangement of carbon atoms in a honeycomb lattice, or in other words, in an overlapping of two hexagonal/triangular lattices with two carbon atoms in its unit cell. It has a few significant electronic properties including a linear energy dispersion relation around the Dirac points. We want to try to find these electronic properties. 

## Quantum Espresso
To perform a density of states calculation in quantum espresso, we need to perform a series of calculations. 
1. Begin with a `pw.x` self consistent field (SCF) calculation (call `calculation = 'scf'`). 
2. Follow with a `pw.x` non-SCF calculation (set `calculation = 'nscf'`) (with a denser *k*-point grid). 
3. Use `dos.x` for post-processing. 


## Step 1: `pw.x` SCF Calculation
We begin with an `scf` calculation. For this we use the following input file for quantum espresso. Note that graphene, a two-dimensional hexagonal structure, we set `ibrav = 4` and specify the lattice constant *a* with `celldm(1) = 4.654` and lattice constant *c* through `celldm(3) = c/a = 3.0` to be large so the only dimension of interest is the 2D XY plane. Lastly, we put two carbon atoms in our unit cell. Note that in this case, one atom is situated at the origin and the other is located at `sqrt(3)/2` away. 

```fortran
 &CONTROL
    calculation = 'scf',
    prefix      = 'graphene',
    outdir      = './temp',
    pseudo_dir  = '/Users/vinhtran/Documents/GitHub/DFT/pseudos' 
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
   C  12.0107 C.pbe-n-kjpaw_psl.1.0.0.UPF
   
ATOMIC_POSITIONS alat
   C    0.000000    0.0000000   0.000000
   C    0.000000    0.5773503   0.000000
   
K_POINTS automatic
   9 9 1   0 0 0
``` 

## Step 2: `pw.x` Non-SCF Calculation 
Next, in the same directory, we perform a `calculation = 'nscf'` call. To this end, we can use the following input file where the notable changes are that the calculation call in the `&CONTROL` card is now `calculation = 'nscf'`, in the `&SYSTEM` card we now specify an `occupation = 'tetrahedra_opt'` and change the `K-points` to be denser with `12x12x1` points. 

Then in the command line we just call `pw.x -in graphene.step_two_nscf.in > graphene.step_two_nscf.out`. 

```fortran
 &CONTROL
    calculation = 'nscf',
    prefix      = 'graphene',
    outdir      = './temp',
    pseudo_dir  = '/Users/vinhtran/Documents/GitHub/DFT/pseudos',        
 /

 &SYSTEM
    ibrav     = 4,
    celldm(1) = 4.654,
    celldm(3) = 3.0,
    nat  = 2,
    ntyp = 1,
    ecutwfc = 20.0,
    ecutrho = 200.0,

    occupations='tetrahedra_opt'
 /
 
 &ELECTRONS
    conv_thr = 1.0d-8
 /
 
ATOMIC_SPECIES
   C  12.0107 C.pbe-n-kjpaw_psl.1.0.0.UPF
   
ATOMIC_POSITIONS alat
   C    0.000000    0.0000000   0.000000
   C    0.000000    0.5773503   0.000000
   
K_POINTS automatic
   12 12 1 0 0 0
```

## Step 3: Passing to a DOS Code 
We use the following input and call `dos.x -in dos.graphene.in > dos.graphene.out` to run this. Note, a really important note, for some reason this will return an `namelist` error if we don't have a blank line at the bottom of the text file after the final `\` (for some reason...). This then returns a file called `graphene.dos` (as specified by `filedos = 'graphene.dos'` in the input file) which is a text file with energy values in eV and the density of states (and integrated density of states). 
```fortran
 &DOS
    prefix = 'graphene',
    outdir = './temp'
    fildos = 'graphene.dos'
 /
 
```
## Step 4: Plotting 
We can then pass the output file `graphene.dos` to a plotter. In particular, we can read in this file into a Pandas dataframe in Python/Jupyter and visualize it by calling matplotlib. In particular, we can entirely interface with quantum espresso through python and a jupyter notebook for instant visualization which makes it an attractive option. 
```python3
dos_data = pd.read_csv('graphene.dos', delim_whitespace = True,\
                        skiprows=1, header = None, names = ['E (eV)', 'DOS', 'Int DOS'])
                        
dos = dos_data.to_numpy()
energy = dos.transpose()[0]
density_of_states = dos.transpose()[1]
int_dos = dos.transpose()[2]
```
<img width="523" alt="Screen Shot 2022-03-22 at 12 16 59 PM" src="https://user-images.githubusercontent.com/76876169/159558624-a61da790-00ac-440a-a9eb-897d054172ec.png">

