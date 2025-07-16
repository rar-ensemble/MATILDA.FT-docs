# FTS Potentials

$$
\newcommand{\mb}{\mathbf}
\newcommand{\mc}{\mathcal}
$$


## Edwards potential

$$
\beta U = \frac{1}{2 u_0} \int d\mathbf{r} \; [\hat\rho(\mathbf{r})]^2
$$

## Helfand potential

$$
\beta U = \frac{1}{2\kappa} \int d\mathbf{r} \; [\hat \rho( \mathbf{r}) - \rho_0]^2
$$


## Incompressibility

This is the Helfand potential in the limit $\kappa \rightarrow \infty$, though using the limiting behavior tends to be more effective than just using a large value of $\kappa$. The partition function is modified to include an explicit constraint, 

$$
\delta [ \hat \rho_{+}(\mathbf{r}) - \rho_0] = \int \mathcal{D}w_+ \; e^{-i \int d\mathbf{r} \, w_+(\mathbf{r}) \, [ \hat \rho_{+}(\mathbf{r}) - \rho_0] }
$$

## Flory potential

Dimensionless form, the product $\chi_{IJ}N_r$ is specified in input file. Generates two fields, $w_{IJ}^{(+)}$ and $w_{IJ}^{(-)}$.

$$
\beta U_{FH} = \frac{\chi_{IJ} N_r}{C} \int d\mathbf{r} \; \hat \rho_I(\mathbf{r}) \, \hat \rho_J(\mathbf{r})
$$

## Particle potential

Nomenclature likely to change, but prefactor is specified as bare $\chi$ (not $\chi N_r$).

$$
\beta U = \frac{\chi_{IP}}{C} \int d\mb r \; \hat \rho_I(\mb r) \, \hat \rho_{P}(\mb r)
$$