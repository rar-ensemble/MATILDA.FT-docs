# PS Molecules

The molecule initialization routines have been developed in a somewhat ad-hoc manner, but they have proven useful.


## Linear multiblock polymers

`molecule [architecture] [amount] [Number of blocks] [block 1 N, specices]  ... [optional args]`


**Optional arguments:**

`xrange`, `yrange`, `zrange`. These limit the range where the molecules are grown. By default, all molecules are grown as random walks at random locations in the simulation box grown from the first monomer, placed at random in the box. If one or more of the optional arguments are provided, then the first monomer will be placed in the range provided, but the remaining monomers will be grown randomly ignoring the range provided.

`bondType [list of integers]`. There must one bondType specified for each block of the copolymer; by default, the bonds will be defined as bond type 1.
\\
NOTE: Bonds for a junction between blocks will be defined as the bond type of the lowest indexed block.

`charge [charge on each block]`. Float values for the charge on each block. This will enable charges in the simulation and add charges to the written init.input.data configuration file.

`angleType [list of integers]`. There must be one angleType specific for each block of the copolymer; by default, angles are disabled. Setting angleType to 0 will make that block fully flexible.
\\
In block polymers, the 'middle' particle of the three involved in the angle determines the angle type. For example, declaring a molecule using `molecule linear nmolecs 1 3 5 A 10 B 5 A angleType 0 1 0` will make an A-B-A, coil-rod-coil triblock copolymer with particles 1-5 and 16-20 as the two A blocks, and 6-15 defining the B blocks. Since particle 6 is part of the B block with the angle potential, there *will* be an angle involving the set of particles $\{5,6,7\}$ and $\{14,15,16\}$. Similarly, if instead this same molecule command were instead appended with `angleType 1 2 3`, then both sets $\{5,6,7\}$ and $\{14,15,16\}$ would use angleType 2, but the angle defined between $\{4,5,6\}$ would use angleType 1.


## Block bottlebrushes



## Side-chain liquid crystalline polymers