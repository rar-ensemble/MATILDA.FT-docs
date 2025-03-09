# PS Potentials
$
\newcommand{\mb}{\mathbf}
\newcommand{\mc}{\mathcal}
$

## Gaussian potential
`potential gaussian [group I] [group J] [prefactor float] [std deviation float]`

Imposes a Gaussian non-bonded potential between groups I and J of the form  
$ U_{nb} = \int d\mathbf{r} \int d\mathbf{r}' \, \rho_I(\mathbf{r}) \, u_G(|\mathbf{r} - \mathbf{r}'|) \, \rho_J(\mathbf{r}')$  
with Gaussian potential $u_G(r) = A \left( \frac{1}{2\pi \sigma^2} \right)^{3/2} e^{-r^2/2\sigma^2}$. The prefactor is the value of $A$, and the standard deviation $\sigma$ controls the range of the potential. This potential has become a common way to 'smear' the delta functions that are commonly used in SCFT calculations to regularize the models, and the non-local potential gives rise to (weak) liquid-like correlations in the fluid. 



If potentials act between the same group (e.g., through a Helfand potential where $U = \frac{\kappa}{2\rho_0} \int_r \int_r' \, \rho_+(r) u_G(r-r') \rho_+(r)$), then the force is accumulated twice in the ps_potentials.cu calcForces() routine.