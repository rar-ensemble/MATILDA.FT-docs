# PS Potentials

## Gaussian potential
`potential gaussian [group I] [group J] [prefactor] [std deviation]`

Imposes a Gaussian non-bonded potential between groups I and J of the form  
$ U_{nb} = A\, \int d\mathbf{r} \int d\mathbf{r}' \, \rho_I(\mathbf{r}) \, u_G(\mathbf{r} - \mathbf{r}') \, \rho_J(\mathbf{r}')$  



If potentials act between the same group (e.g., through a Helfand potential where $U = \frac{\kappa}{2\rho_0} \int_r \int_r' \, \rho_+(r) u_G(r-r') \rho_+(r)$), then the force is accumulated twice in the ps_potentials.cu calcForces() routine.