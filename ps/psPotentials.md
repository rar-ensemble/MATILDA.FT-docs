# PS Potentials
$
\newcommand{\mb}{\mathbf}
\newcommand{\mc}{\mathcal}
$

## Overall notes about potentials

### Pair potentials
If potentials act between the same group (e.g., through a Helfand potential where $U = \frac{\kappa}{2\rho_0} \int d{\mb r} \int d\mb{r}' \, \rho_+(\mb{r}) \, u_G(\mb{r}-\mb{r}') \, \rho_+(\mb{r}')$) with $u_G$ a unit Gaussian, then the force is accumulated twice in the ps_potentials.cu calcForces() routine. If such a 'self'-interacting potential is used, take care when defining the prefactor such that it includes the factor of 1/2.

### Smearing and pair potentials



## Gaussian potential
`potential gaussian [group I] [group J] [prefactor float] [std deviation float]`

Imposes a Gaussian non-bonded potential between groups I and J of the form  
$ U_{nb} = \int d\mathbf{r} \int d\mathbf{r}' \, \rho_I(\mathbf{r}) \, u_G(|\mathbf{r} - \mathbf{r}'|) \, \rho_J(\mathbf{r}')$  
with Gaussian potential $u_G(r) = A \left( \frac{1}{2\pi \sigma^2} \right)^{3/2} e^{-r^2/2\sigma^2}$. The prefactor is the value of $A$, and the standard deviation $\sigma$ controls the range of the potential. This potential has become a common way to 'smear' the delta functions that are commonly used in SCFT calculations to regularize the models, and the non-local potential gives rise to (weak) liquid-like correlations in the fluid. 


