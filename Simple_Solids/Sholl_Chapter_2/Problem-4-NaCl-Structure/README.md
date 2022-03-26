# Sholl Ch 2. Problem 4

## Problem Statement 
Another common structure for AB compounds is the NaCl structure. In
this structure, A and B atoms alternate along any axis of the simple cubic structure. Predict the lattice parameter for ScAl in this structure and show by comparison to your results from the previous exercise that ScAl does not prefer the NaCl structure.


## Problem Learning Goals
 - How to create a structure with more than one type of atom in its unit cell, in this case, a NaCl structure. 
 - Lattice constant optimization 

## Quantum Espresso 
We perform the optimization in a "standard" way where we define an array of different lattice constant values and loop over them, run our quantum espresso code, and write out the data points. 

### Construction of the Structure 
For the construction of the CsCl structure, we set `ibrav=0` and manually construct the cell. We put a `Sc` at the origin and an `Al` at `0.0 0.5 0.0` and `0.5 0.0 0.0` and so on to get an alternating structure. We end up constructing a unit cell with 8 atoms in it. We can check our structure by reading it into `ASE` in an `atom` environment and then either plotting it in `ase.io.view` or by exporting it as a `.cif` file and passing it to `Vesta`. In the end we have something of the following form. 

<p align = center> 
<img width="350" alt="Screen Shot 2022-03-25 at 3 38 10 PM" src="https://user-images.githubusercontent.com/76876169/160220498-b8812012-b013-4c32-a6a5-77c7f233b17b.png">
</p> 

### Input File 

```fortran 
&CONTROL 
    calculation = 'scf' 
    prefix = 'ScAl' 
    outdir = './temp' 
    pseudo_dir = '/Users/vinhtran/Documents/GitHub/DFT/pseudos' 
/ 
&SYSTEM
    ibrav = 1
    celldm(1) = 12.0
    nat = 8
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
    Al  0.00    0.50    0.00
    Al  0.50    0.00    0.00
    Sc  0.50    0.50    0.00
    Al  0.00    0.00    0.50
    Sc  0.00    0.50    0.50
    Sc  0.50    0.00    0.50
    Al  0.50    0.50    0.50 
K_POINTS automatic 
    4   4   4   0   0   0 
    
``` 

### Python Scripts
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
We can extract the fitted parameter `a_0` from `popt_m` and we find `a0 = 10.6785` Bohr. We also have the following plot with the fitted curve overlaid. We see that in this case that the lattice constant is larger. However, we also notice that when compared to problem 3 that the energy per atom would be `-893.5` eV compared to `-894.25` eV in the CsCl structure. Though slight, this indicates that ScAl prefers the CsCl structure over the NaCl structure!
<p center = align> 
<img width="913" alt="Screen Shot 2022-03-25 at 7 07 32 PM" src="https://user-images.githubusercontent.com/76876169/160220616-039680a5-b058-44c5-a8c4-59d2da657ef6.png">

</p> 
