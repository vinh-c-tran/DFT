# Sholl Ch2. Problem 3

## Problem Statement 
A large number of solids with stoichiometry AB form the CsCl structure. In this structure, atoms of A define a simple cubic structure and atoms of B reside in the center of each cube of A atoms. Define the cell vectors
and fractional coordinates for the CsCl structure, then use this structure to predict the lattice constant of ScAl.

## Problem Learning Goals
 - How to create a structure with more than one type of atom in its unit cell 
 - Lattice constant optimization 

## Quantum Espresso 
We perform the optimization in a "standard" way where we define an array of different lattice constant values and loop over them, run our quantum espresso code, and write out the data points. 

For the construction of the CsCl structure, we define the unit cell as a SC cell using `ibrav = 1` 

```fortran 

``` 
