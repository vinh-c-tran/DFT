# Density of States
The density of states $g(\epsilon)$ is defined such that when we multiply it by an infinitesimal energy $d\epsilon$, that is, $g(\epsilon)d(\epsilon)$ that we get the total number of eigenstates with energies between $\epsilon$ and $\epsilon + d\epsilon$. 

This is a quantity that's important for then calculating other things as well like band structures and other staples in solid state. 

## Quantum Espresso
To perform a density of states calculation in quantum espresso, we need to perform a series of calculations. 
1. Begin with a `pw.x` self consistent field (SCF) calculation (call `calculation = 'scf'`). 
2. Follow with a `pw.x` non-SCF calculation (set `calculation = 'nscf'`) (with a denser *k*-point grid). 
3. Use `dos.x` for post-processing. 
