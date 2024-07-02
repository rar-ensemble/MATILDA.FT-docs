## Input file keywords

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