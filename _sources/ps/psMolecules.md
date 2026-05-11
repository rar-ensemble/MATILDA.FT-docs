# PS Molecules

Currently, only linear and grafted linear (e.g., comb or bottle brush) molecules are supported.

## Linear multiblock polymers

Architecture must be `linear`; nonlinear architectures (stars, bottlebrushes, etc) will be added in the future.

Amount can be specified as `phi` or `nmolecs`. If `nmolecs`, must be followed by an integer specifying the number of molecules of this type to add. If `phi` is used, the number of molecules to add is computed as $n = \textrm{int}(\rho_0 V \phi / N_{tot})$, where $N_{tot}$ is the sum of $N$ for each block in this molecule, $\rho_0$ is the nominal monomer density, and $V$ is the system volume. 
\\
NOTE: The value of $\phi$ provided is *only* used to compute the number of molecules using the formula above. If multiple molecules are defined and the sum of all of the $\phi$ values provided is greater than 1, *this will not be caught*. 
\\
NOTE: The number of molecules is computed assuming that all monomers have equal volume.

Each block is defined as the integer number of monomers followed by the species label. All species must be defined prior to using them in a molecule.

### Optional arguments:

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

## Grafted linear polymers

The above commands can be augmented to graft linear polymers along the backbone. To implement grafting, the **final** command passed to `molecule` must be `grafting` followed by a syntax similar to the above for linear polymers. Each block of the main backbone must have a section in the grafting command. Some examples follow.

```
molecule linear phi 0.5  1  40 A  grafting 1 20 A
```
This example creates a linear chain with 40 A monomers along the main backbone and grafts at each monomer that are 20 A monomers long.