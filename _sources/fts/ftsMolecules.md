# FTS Molecules

## Linear discrete Gaussian chains

## Liquid crystals
`molecule liquid-crystal phi [volume fraction] [int number of sites] [text list of species per site] [optional arguments]`

Liquid crystals are intended (for now) to be small, rigid, linear molecules with $N_{tot}$ particles, and each particle on the molecule has the same 'smearing' function (if employed). The default behavior is to assume only the middle bead interacts with the orientation/tensor potential, though the 'lc_style' variable below can change this.

Optional arguments:  
- `lc_style [text]`: Defines whether the LC potential applies on the 'middle' or 'all' sites. Default: 'middle'  
- `smear gaussian [sigma]`: smears each site over a unit Gaussian with std dev $\sigma$. Default: disabled.  
- `bond_length [b]`: Magnitude of separation between beads on the mesogen. Default: 1.  

```bash
molecule liquid-crystal phi 1.0 3 A A A
```

The molecule code computes the molecular partition function and its associated density fields:
$$
Q = \frac{1}{\Omega_d V} \int d\mb r \, \int d\mb u \; \prod_j^{N_{tot}} e^{-w(\mb r_j, \mb u)}, 
$$
where $\Omega_d = 2\pi$ and $4\pi$ in d=2D and 3D, respectively. The code assumes all $N_{tot}$ sites are colinear and the bonds are of a fixed length, so that specifying one particle and $\mathbf{u}$ specifies the position of each other particle. The field $w(\mb r, \mb u)$ is then evaluated at each particle position and convolved with the smearing function as needed. 

Assuming a tensor-form Maier-Saupe and an Edwards-like excluded volume potential both apply, then we would have  
$$
w(\mb r, \mb u) = \int d\mb r' \, h(\mb r-\mb r') \left[ i w_+(\mb r') + \
    \mb M(\mb r'):\left(\mb{uu} - \frac{\mb I}{\mathbb{d}}\right) \right]
$$
with $\mb M(\mb r)$ resulting from the Maier-Saupe potential. 


The other needed operators are the two density fields and the orientation field. In the $NVT$ ensemble, these take the form
$$
\begin{aligned}
    \tilde \rho(\mb r, \mb u)   &= - n \frac{ \delta \log Q}{\delta w(\mb r, \mb u)}  \\
                                &= \frac{n}{\Omega_d V Q} \prod_j^{N_{tot}} e^{-w(\mb r_j, \mb u)} \\
    \tilde \rho(\mb r)          &= \int d\mb u \; \tilde \rho(\mb r, \mb u) \\
    \tilde{ \mb S}(\mb r)       &= - n \frac{ \delta \log Q}{\delta \mb M (\mb r)} \\
                                &= \int d \mb u \; \left(\mb{uu} - \frac{\mb I}{\mathbb{d}}\right) \tilde \rho(\mb r, \mb u)
\end{aligned}
$$  
For smeared densities, each of the above operators would need to be further convolved with the smearing function prior to their use in the FTS forces that update the potential fields.