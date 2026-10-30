# MATILDA.FT - Mesoscale simulations of soft matter systems

<!-- This is a small sample book to give you a feel for how book content is
structured.
It shows off a few of the major file types, as well as some sample content.
It does not go in-depth into any particular topic - check out [the Jupyter Book documentation](https://jupyterbook.org) for more information. -->

<!-- Check out the content pages bundled with this sample book to see more. -->

This software was originally published in the [Journal of Chemical Physics](https://doi.org/10.1063/5.0145006) in 2023, and the updated source code is maintained on [Github](https://www.github.com/rar-ensemble/MATILDA.FT). This is an in-progress set of documentation for the code.

***THIS DOCUMENTATION HAS NOT BEEN FULLY VETTED FOR PUBLIC USE AND MAY CONTAIN INACCURACIES***

$$
\mathcal{Z} = z_0 \int d\mathbf{r}^n \; e^{-\beta U(\mathbf r^n)} = z_1 \int \mathcal{D} \{w\} \; e^{-\mathcal{H}}
$$

The purpose of this code base is to be able to simulate using either field-theoretic simulations (FTS), such as self-consistent field theory or compelx Langevin, and particle-based simulations (PS) of the same (or at least very similar) family of models. We suggest starting with the Overview section to become acquainted with the code, how to run it, and its features.

```{tableofcontents}
```

### Recent changes

July 2025: MASSIVE overhaul to move to version 2.