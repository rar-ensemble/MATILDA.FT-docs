## PS input file keywords

As with all simulations using this code, the first step is to define the box:
```box ps```

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
`molecule [architecture] [amount] [Number of blocks] [block 1 N, specices]  ... [optional args]`

Add molecules to the simulation box of type 'architecture'. Multiple molecules can be added before the `endBox` command.

Architecture must be `linear`; nonlinear architectures (stars, bottlebrushes, etc) will be added in the future.

Amount can be specified as `phi` or `nmolecs`. If `nmolecs`, must be followed by an integer specifying the number of molecules of this type to add. If `phi` is used, the number of molecules to add is computed as $n = \textrm{int}(\rho_0 \phi / N_{tot})$, where $N_{tot}$ is the sum of $N$ for each block in this molecule. 
\\
NOTE: The value of $\phi$ provided is *only* used to compute the number of molecules using the formula above. If multiple molecules are defined and the sum of all of the $\phi$ values provided is greater than 1, *this will not be caught*. 
\\
NOTE: The number of molecules is computed assuming that all monomers have equal volume.

Each block is defined as the integer number of monomers followed by the species label. All species must be defined prior to using them in a molecule.

Optional arguments: `xrange`, `yrange`, `zrange`. These limit the range where the molecules are grown. By default, all molecules are grown as random walks at random locations in the simulation box grown from the first monomer, placed at random in the box. If one or more of the optional arguments are provided, then the first monomer will be placed in the range provided, but the remaining monomers will be grown randomly ignoring the range provided.

```
molecule linear phi 0.5 1 40 A xrange 0.0 25.0
molecule linear nmolecs 10 2 20 A 20 B
```

In the above examples, the first command makes enough linear, type-A homopolymer molecules to be 50\% by number on a monomer basis. The first monomer is placed in the x range given, and the rest of the chain is grown as a random walk from that starting monomer.

The second example makes ten diblock copolymer molecules with $N_{tot} = 40$ and 20 monomers per block.

#### Nr
`Nr [integer]`

Reference chain length that will be used for some initialization tools that will be developed in the future.

#### NSEXTRA

#### pmeorder

#### randSeed

#### rho0
`rho0 [float]`

Sets the monomer density used in some of the initialization routines.


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

#### verbose
`verbose [val]`

Sets the code for the current box to be 'verbose' if val = 1, meaning tons of information will be printed to the screen about where the code is. This also enables some calls to cudaDeviceSynchronize(), which can be useful for debugging. This WILL slow the code down and the total number of steps run in this mode should be minimal. 

Default: 0 (false)

```
verbose 1
```
