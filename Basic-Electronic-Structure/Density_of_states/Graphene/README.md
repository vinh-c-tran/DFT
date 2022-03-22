# Graphene Density of States
Graphene is a 2D arrangement of carbon atoms in a honeycomb lattice, or in other words, in an overlapping of two hexagonal/triangular lattices with two carbon atoms in its unit cell. It has a few significant electronic properties including a linear energy dispersion relation around the Dirac points. We want to try to find these electronic properties. 

## Quantum Espresso
To perform a density of states calculation in quantum espresso, we need to perform a series of calculations. 
1. Begin with a `pw.x` self consistent field (SCF) calculation (call `calculation = 'scf'`). 
2. Follow with a `pw.x` non-SCF calculation (set `calculation = 'nscf'`) (with a denser *k*-point grid). 
3. Use `dos.x` for post-processing. 
