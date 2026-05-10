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

Architecture must be `linear`; nonlinear architectures (stars, bottlebrushes, etc) will be added in the future.

Amount can be specified as `phi` or `nmolecs`. If `nmolecs`, must be followed by an integer specifying the number of molecules of this type to add. If `phi` is used, the number of molecules to add is computed as $n = \textrm{int}(\rho_0 V \phi / N_{tot})$, where $N_{tot}$ is the sum of $N$ for each block in this molecule, $\rho_0$ is the nominal monomer density, and $V$ is the system volume. 
\\
NOTE: The value of $\phi$ provided is *only* used to compute the number of molecules using the formula above. If multiple molecules are defined and the sum of all of the $\phi$ values provided is greater than 1, *this will not be caught*. 
\\
NOTE: The number of molecules is computed assuming that all monomers have equal volume.

Each block is defined as the integer number of monomers followed by the species label. All species must be defined prior to using them in a molecule.

**Optional arguments:**

`xrange`, `yrange`, `zrange`. These limit the range where the molecules are grown. By default, all molecules are grown as random walks at random locations in the simulation box grown from the first monomer, placed at random in the box. If one or more of the optional arguments are provided, then the first monomer will be placed in the range provided, but the remaining monomers will be grown randomly ignoring the range provided.

`bondType [list of integers]`. There must one bondType specified for each block of the copolymer; by default, the bonds will be defined as bond type 1.
\\
NOTE: Bonds for a junction between blocks will be defined as the bond type of the lowest indexed block.

`charge [charge on each block]`. Float values for the charge on each block. This will enable charges in the simulation and add charges to the written init.input.data configuration file.

`angleType [list of integers]`. There must be one angleType specific for each block of the copolymer; by default, angles are disabled. Setting angleType to 0 will make that block fully flexible.
\\
In block polymers, the 'middle' particle of the three involved in the angle determines the angle type. For example, declaring a molecule using `molecule linear nmolecs 1 3 5 A 10 B 5 A angleType 0 1 0` will make an A-B-A, coil-rod-coil triblock copolymer with particles 1-5 and 16-20 as the two A blocks, and 6-15 defining the B blocks. Since particle 6 is part of the B block with the angle potential, there *will* be an angle involving the set of particles $\{5,6,7\}$ and $\{14,15,16\}$. Similarly, if instead this same molecule command were instead appended with `angleType 1 2 3`, then both sets $\{5,6,7\}$ and $\{14,15,16\}$ would use angleType 2, but the angle defined between $\{4,5,6\}$ would use angleType 1.


```
molecule linear phi 0.5 1 40 A xrange 0.0 25.0
molecule linear nmolecs 10 2 20 A 20 B
molecule linear phi 0.1 3 10 A 20 B 10 C bondType 1 2 1
```

In the above examples, the first command makes enough linear, type-A homopolymer molecules to be 50\% by number on a monomer basis. The first monomer is placed in the x range given, and the rest of the chain is grown as a random walk from that starting monomer.

The second example makes ten diblock copolymer molecules with $N_{tot} = 40$ and 20 monomers per block.

The third example makes a triblock copolymer with $N_A=10, N_B=20,$ and $N_C=10$. The bond types for the A and C blocks are type 1, while those for the middle block are of type 2. The A-B junction will be defined with a type 1 bond, while the B-C junction will be defined as type 2.

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
