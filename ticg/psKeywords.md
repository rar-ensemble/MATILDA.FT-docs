## PS input file keywords

As with all simulations using this code, the first step is to define the box:
```box fts scft```

Keywords for an PS box follow. Anything inside of square brackets represents an argument to be defined by the user.

#### blockSize


#### bond
`bond [bond type integer] [style] [parameters]`

Defines parameters for each bond style used in the simulation. The `bond type integer` must match the molecule definitions where the molecules are defined, either using the `molecule` command or reading from a data/gsd file. Style must be `harmonic` or `fene`.

Style options and parameters:

`harmonic [spring constant] [equil dist]`

Uses a potential $u(\mathbf{r}_{ij}) = k \; (|\mathbf{r}_{ij}|-r_0)^2$ where $k$ and $r_0$ are the force constant and equilibrium distance, respectively.

`fene [prefactor] [maximum distance]`

Uses a potential $u(\mathbf{r}_{ij}) = - k\; r_{max}^2 \log \left[ 1-(|\mathbf{r}_{ij}|/r_{max})^2 \right]$.

```
bond 1 harmonic 1.5 0.0
bond 2 harmonic 500 1.0
bond 3 fene 30.0 1.5
```


#### boxLengths
``boxLengths [x-value y-value (optional)z-value]``

List of float box lengths in each dimension. There should be one float for every dimension, and the units are in $b$.

```
boxLengths 25.0 25.0
boxLengths 6.0 6.0 6.0
```

#### datFileName
``datFileName [string]``

Name of the data file that stores the time step values, energies, etc

```
datFileName data.dat
datFileName garbage.dat
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

#### gsdFreq

#### gsdName

#### integrator



#### logFreq
`logFreq [value]`

Number of iterations between writes to the log file.

```
logFreq 100
```

#### MAXANGLES

#### MAXBONDS

#### molecule
optional: xrange, yrange, zrange. All set the range for first monomer, rest is grown as random walk.

#### Nr

#### NSEXTRA

#### pmeorder

#### randSeed

#### rho0


#### species
`species [label] [optional arguments]`

Label for the various species that will be simulated. In most contexts, species will be labeled 'A', 'B', etc, consistent with the notation used in most field-theoretic contexts. If a molecule is going to contain monomers of a given species, *the species must be defined before the molecule*.

Optional arguments:

`mass [float]`

Floating point mass of the particles. Default value 1.0.

`mobility [float]`

Floating point mobility of the particles. Default value 1.0.

```
species A
species B mass 2.0
```
```
species A mobility 1.0
```


#### potential
`potential [style] [group I] [group J] [parameters]`

