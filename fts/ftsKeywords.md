# FTS input file keywords

As with all simulations using this code, the first step is to define the box:
```box fts scft```

This creates a simulation box for SCFT calculations; this implementation will not do any stochastic sampling. 

Keywords for an FTS box follow. Anything inside of square brackets represents an argument to be defined by the user.

## boxLengths
``boxLengths [x-value y-value (optional)z-value]``

List of float box lengths in each dimension. There should be one float for every dimension, and the units are in the $R_g$ of the reference polymer length $N_r$.  

```
boxLengths 25.0 25.0
boxLengths 6.0 6.0 6.0
```



## chemFieldFreq
`chemFieldFreq [integer value]`

Number of iterations between writing the potential fields to human-readable files. Fields are written as complex values.





## densityFieldFreq
`densityFieldFreq [integer value]`

Number of iterations between writing the density fields to human-readable files. Fields are written as complex values.





## Dim
``Dim [integer value]``  

Dimensionality of the simulation box. The code is only explicitly tested for 2 and 3 dimensions.

``Dim 2``




## grid
`grid [x-value y-value (optional)z-value]`  

List of integer number of grid points in each dimension. There should be one integer for every dimension.  

```
grid 225 225
grid 63 63 63
```



## logFreq
`logFreq [value]`

Number of iterations between writes to the log file.

```
logFreq 100
```






## molecule
`molecule [architecture] [volume fraction] [architecture-specific arguments]`

**add link to molecule-specific page for details**



## potential
`potential ...`

**add link to potential-specific page for details**



## rho0
`rho0 [float value]`

Total monomer density of the simulation box. 

**Check units of rho0 - [Rg^-3] or [b^-3]?**


## species
`species [label]`

Label for the various species that will be simulated. In most contexts, species will be labeled 'A', 'B', etc, consistent with the notation used in most field-theoretic contexts. If a molecule is going to contain monomers of a given species, *the species must be defined before the molecule*.

```
species A
species B
```


## tolerance
`tolerance [style] [tolerance value]`

Tolerance used for convergence in an SCFT calculation. By default, this checks the change in the effective Hamiltonian, $\mathcal{H}$, between two print steps according to
```c++
if ( step % logFreq == 0 ) {
    if ( fabs(Heff.real() - Hold.real()) < tolerance) {
        break; // exits loop over steps
    }
    else {
        Hold = Heff;
    }
} 
```

Other options are
- `phi`: uses a maximum change in number fraction of a molecule as the tolerance. This only makes sense if there are components simulated in the $\mu$VT ensemble.
- `density`: **[Not yet implemented]** Uses an integrated change in the density field of species [index] as the tolerance, involving the net change on the entire density grid. The maximum is taken over all density species.
- `potential`: **[Not yet implemented]** Uses integrated change in the potential field of potential [index] as the tolerance, involving the net change on the entire density grid. The maximum is taken over all density species.

In all options, the indices are counted from 1


```
tolerance Heff 1.0E-6
tolerance phi 1.0E-4
tolerance density 1.0E-4
```

An example using `phi` and including the molecule definitions in the input file:
```
species A
species B
molecule linear activity 1.0 1 40 A
molecule linear activity 3.0 1 40 B
tolerance phi 1.0E-5
```
In the above more detailed example, this wil look at the change in the number of monomers of both homopolymers for the tolerance. In other words,
```c++
if ( step % logFreq == 0 ) {

    phi[i] = Molecs[i]->nSites / rho0 / V;   
    
    if ( fabs(phi[i] - phiOld[i]) > tolerance ) {
        bool CONVERGED = false;
    }

    phiOld[i] = phi[i];
    
} 
```
It assumes the total number of monomers for the denominator to compute $\phi$ is $\rho_0 V$.