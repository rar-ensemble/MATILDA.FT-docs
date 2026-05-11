## PS input file keywords

As with all simulations using this code, the first step is to define the box:
```box ps```

Keywords for an PS box follow. Anything inside of square brackets represents an argument to be defined by the user.

### angle


### blockSize
`blockSize [int value]`

Number of GPU threads within a threadblock on the GPU. Typical values are in the range of 128 - 512.

```
blockSize 256
```


### bond
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


### boxLengths
``boxLengths [x-value y-value (optional)z-value]``

List of float box lengths in each dimension. There should be one float for every dimension, and the units are in $b$.

```
boxLengths 25.0 25.0
boxLengths 6.0 6.0 6.0
```

### datFileName
``datFileName [string]``

Name of the data file that stores the time step values, energies, etc

```
datFileName data.dat
datFileName garbage.dat
```

### Dim
``Dim [integer value]``  

Dimensionality of the simulation box. The code is only explicitly tested for 2 and 3 dimensions.

``Dim 2``


### endBox
``endBox``

Terminates the initialization of the current simulation box. The 'closing' argument of `box ps`


### fieldFreq
``fieldFreq [integer value]``

Frequency of writing field data to the disk. 

```
fieldFreq 100
```

### doCharges
``doCharges``

Enables charges for reading from .data configuration file. This flag is not needed if using the `molecule` initializations.

### grid
`grid [x-value y-value (optional)z-value]`  

List of integer number of grid points in each dimension. There should be one integer for every dimension.  

```
grid 225 225
grid 63 63 63
```

### gsdFreq
`gsdFreq [integer value]`

Number of time steps between writes to gsd file.


### gsdName
`gsdName [string name]`

Rename output gsd file to 'name'.


### initDataFileName
`initDataFileName [string name]`

Changes the name of the starting configuration data file name. Default: init.input.data.



### integrator
`integrator [group name] [integrator name] [options]`

Defines the integrator used during the time integration.

Allowed integrator names: `GJF`, which uses the Gronbeck-Jensen and Farago [*Mol Phys* 2013] to discretize the Stoermer-Verlet algorithm.  

Optional arguments:

`delt [float value]`: size of time step to use. Default value 0.002.

```
integrator all GJF
integrator all GJF delt 0.005
```



### logFreq
`logFreq [value]`

Number of iterations between writes to the log file.

```
logFreq 100
```

### MAXANGLES
`MAXANGLES [int value]`

Maximum number of angles allowed on each particle. Should be defined before molecules are created.  
Default value: 3

```
MAXANGLES 9
```

### MAXBONDS
`MAXBONDS [int value]`

Maximum number of bonds allowed on each particle. Should be defined before molecules are created. Default value: 2

```
MAXBONDS 4
```


### molecule
`molecule [architecture] [amount] [Number of blocks] [block 1 N, specices]  ... [optional args]`

Add molecules to the simulation box of type 'architecture'. Multiple molecules can be added before the `endBox` command.

See the [molecules section](./psMolecules.md) for complete details and examples.

### Nr
`Nr [integer]`

Reference chain length that will be used for some initialization tools that will be developed in the future.

### NSEXTRA

### pmeorder

### potential
`potential [style] [parameters]`

Defines a non-bonded potential, typically acting between two groups. See the *particle potentials* page for details.


### randSeed
`randSeed [int value]`

Set the random number seed to `value` for *both* CPU and GPU random number generators.  

Default value: set to the clock; this makes a different seed for each simulation that is started at different times.  

NOTE: if one uses scripts to initialize many simulations and they all spawn and begin with rapid succession, you may end up with multiple jobs running with the same RNG seed.

```
randSeed 777
```


### readData 
`readData [string filename]`

Reads a lammps-style input configuration. The expected format is angle-style, though the order and spacing of the data in the file is more rigid in this code than LAMMPS. 

If charges are contained in the provided data file, `doCharges` must be given before readData.

If additional molecules are to be added to the configuration provided in the data file using the `molecule` command, then readData must precede the `molecule` commands.

### rho0
`rho0 [float]`

Sets the monomer density used in some of the initialization routines.


### species
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

### trajFileName
`trajFileName [string name]`

Change the name of the lammpstrj file to the given name. Default is traj.lammpstrj.



### trajFreq
`trajFreq [integer value]`

Set the frequency for writing to the lammpstrj file. These files are much less efficient than the GSD format and should be used sparingly.


### verbose
`verbose [val]`

Sets the code for the current box to be 'verbose' if val = 1, meaning tons of information will be printed to the screen about where the code is. This also enables some calls to cudaDeviceSynchronize(), which can be useful for debugging. This WILL slow the code down and the total number of steps run in this mode should be minimal. 

Default: 0 (false)

```
verbose 1
```
