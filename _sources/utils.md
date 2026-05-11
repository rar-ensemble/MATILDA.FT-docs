# Utilities and post-processing

In the `MATILDA.FT/utils` folder, there are some useful programs and features to post-process your data.







#### pyGridDens

This is a small python library that will read in the grid density from a particle-based simulation and store it in a numpy array for processing/plotting in Python.

Requirements: pybind11, should install simply as 
```
pip install pybind11
```
or
```
conda install pybind11
```

<!-- #### extract-resume.sh
`extract-resume.sh` operates on a lammpstrj file and pipes the final frame into a separate file that can be read as the configuration of the particles in the simulation box.

Prerequisites: this only uses standard bash commands. -->