## FTS input file keywords

As with all simulations using this code, the first step is to define the box:
```box fts scft```

This creates a simulation box for SCFT calculations; this implementation will not do any stochastic sampling. 

Keywords for an FTS box follow. Anything inside of square brackets represents an argument to be defined by the user.

#### boxLengths
``boxLengths [x-value y-value (optional)z-value]``

List of float box lengths in each dimension. There should be one float for every dimension, and the units are in the $R_g$ of the reference polymer length $N_r$.  

```
boxLengths 25.0 25.0
boxLengths 6.0 6.0 6.0
```



#### chemFieldFreq
`chemFieldFreq [integer value]`

Number of iterations between writing the potential fields to human-readable files. Fields are written as complex values.





#### densityFieldFreq
`densityFieldFreq [integer value]`

Number of iterations between writing the density fields to human-readable files. Fields are written as complex values.





#### Dim
``Dim [integer value]``  

Dimensionality of the simulation box. The code is only explicitly tested for 2 and 3 dimensions.

``Dim 2``




#### grid
`grid [x-value y-value (optional)z-value]`  

List of integer number of grid points in each dimension. There should be one integer for every dimension.  

```
grid 225 225
grid 63 63 63
```



#### logFreq
`logFreq [value]`

Number of iterations between writes to the log file.

```
logFreq 100
```




#### maxSteps
`maxSteps [value]`

Integer maximum total number of possible simulation steps to take in the calculation. SCFT simulations can terminate before performing `maxSteps` iterations if convergence criteria is reached.

```
maxSteps 7500
```



#### molecule
`molecule [architecture] [volume fraction] [architecture-specific arguments]`

**add link to molecule-specific page for details**



#### potential
`potential ...`

**add link to potential-specific page for details**



#### rho0
`rho0 [float value]`

Total monomer density of the simulation box. 

**Check units of rho0 - [Rg^-3] or [b^-3]?**


#### species
`species [label]`

Label for the various species that will be simulated. In most contexts, species will be labeled 'A', 'B', etc, consistent with the notation used in most field-theoretic contexts. If a molecule is going to contain monomers of a given species, *the species must be defined before the molecule*.

```
species A
species B
```
