# FTS Molecules

## Linear discrete Gaussian chains

## Liquid crystals
`molecule liquid-crystal phi [volume fraction] [int number of sites] [text list of species per site] [optional arguments]`

Liquid crystals are intended (for now) to be small, rigid, linear molecules with $N_{tot}$ particles, and each particle on the molecule has the same 'smearing' function (if employed). The default behavior is to assume only the middle bead interacts with the orientation/tensor potential, though the 'lc_style' variable below can change this.

Optional arguments:  
- `lc_style [text]`: Defines whether the LC potential applies on the 'middle' or 'all' sites. Default: 'middle'  
- `smear gaussian [sigma]`: smears each site over a unit Gaussian with std dev $\sigma$. Default: disabled.

```bash
molecule liquid-crystal phi 1.0 3 A A A
```

The molecule code computes the molecular partition function and its associated density fields:
$$
Q = \frac{1}{\Omega_0 V} \int d\mb r \, \int d\mb u \; \prod_j^{N_{tot}} e^{-w(\mb r_j, \mb u)}, 
$$
where $\Omega_0 = 2\pi$ and $4\pi$ in 2D and 3D, respectively. The code assumes all $N_{tot}$ sites are colinear and the bonds are of a fixed length, so that specifying one particle and $\mathbf{u}$ specifies the position of each other particle. The field $w(\mb{r}, \mathbf{u})$