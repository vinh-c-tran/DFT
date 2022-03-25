# Sholl Ch 2. Problem 3

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

## Python Scripts
We use the following Python scripts to update the quantum espresso input file and iterate through the possible lattice constant values. 
```python
def parse_output(outfile):
    """ Parses the quantum espresso output file
        Returns a dictionary with relevants in key-value pairs
        such as {'energy': energy_value}
    """
    
    with open(outfile, 'r') as outf:
        for line in outf:
            if (line.lower().startswith('     lattice parameter (alat)')):
                lattice_constant = float(line.split()[-2]) * 0.529177
            if (line.lower().startswith('!    total energy')):
                total_energy = float(line.split()[-2]) * 13.605698066
    
    result = {'energy': total_energy, 'lattice': lattice_constant}
    return result


def lattice_subs(file, lattice_parameter, offset = '0'):
    """ opens input file file and changes value for lattice constant """
    
    # check for proper input 
    assert type(lattice_parameter) == 'int' or 'numpy.int64', 'Provide k-points input as int or numpy.int64' 
    lat_string = "    celldm(1) = " + str(lattice_parameter) + "\n"
    
    # open the file 
    with open(file,'r') as input_file:
        lines = input_file.readlines()
    with open(file, 'w') as input_file:
        for line in lines:
            if line.split()[0] == 'celldm(1)':
                # write the k_points string 
                input_file.write(lat_string)
            else:
                input_file.write(line)
                
def problem_4(lattice_array):
    """ performs problem 4 calculations 
        finds energy as a function of lattice points 
    """
    
    # declare force array of same size as cut off array 
    energy_array = np.zeros(len(lattice_array))
    
    for i in range(len(lattice_array)):
        # update input file 
        lattice_subs("ScAl.lattice.in", lattice_array[i])
        
        # call pw.x 
        subprocess.run('pw.x -in ScAl.lattice.in > ScAl.lattice.out', shell=True)
        
        # parse output file 
        result = parse_output('ScAl.lattice.out')
        
        # get force and append to array 
        energy_array[i] = result['energy']
        
    return energy_array
```
Once we have the data from the quantum espresso files, we can then perform a fit to an equation of state. For instance we can use a `murnaghan` equation of state and perform fitting using `curve_fit` from `scipy.optimize`. 

```python
def murnaghan(a, a0, B0, B0_prime, E0):
    coeff_1 = 9*B0*a0**3/16/4
    brac_1 = ((a0/a)**2 - 1)**3 * B0_prime 
    brac_2 = ((a0/a)**2 - 1)**2 * (6 - 4*(a0/a)**2)
    return E0 + coeff_1 * (brac_1 + brac_2)
  
from scipy.optimize import curve_fit
popt_m, pcov_m = curve_fit(murnaghan, lattice_array, energy, p0 = [6.00, 2.68, 4.10, -5.81340065e+03])
```

## Results 
We can extract the fitted parameter `a_0` from `popt_m` and we find `a0 = 6.4409`. We also have the following plot
<p center = align> 
<img width="589" alt="Screen Shot 2022-03-25 at 12 42 40 PM" src="https://user-images.githubusercontent.com/76876169/160190670-91187f73-b5d1-46cd-a931-c033b7f872e0.png">
</p> 

## Visualization 
To this point we've trusted our input structure to be correct. But we can verify this by plotting it. In particular, we can do the following
1. Read in the `quantum espresso` input file into an `ase` `Atom` object using `ase.io` `read` 
2. Then, we can write out the `Atom` object to a `.cif` file to pass to Vesta for visualization or to just call `view()` 
< p align = center> 
<img width="300" alt="Screen Shot 2022-03-25 at 12 54 37 PM" src="https://user-images.githubusercontent.com/76876169/160192600-2a68f0a4-4baa-418d-9ea4-fe979ca4b852.png">
</p> 
