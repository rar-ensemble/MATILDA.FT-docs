# Overview

## Installation

TBD.


## Getting started

All simulations require at least one input file to run. 

## Command-line options

### Input file name

The default input file name is 'input'. If this file is not found, the program will immediately exit. To change the input file name, simply provide the argument '-in' followed by the file name. Example:
```bash
matilda.ft -in scft-input
```

### Device selection

By default, CUDA will choose device '0' upon startup. You can manually select a different device with the '-device' option, followed by an integer. Example:
```bash
matilda.ft -device 1
matilda.ft -in scft-input -device 1
matilda.ft -device 1 -in scft-input
```
The last two examples in the above should yield identical results; the order of the arguments does not matter.


## Global commands

The input file is separated into 'global commands' and 'box commands' Typically, 'box commands' deal with creating molecules, potentials, and initializing the information required to run a simulation. 'global commands' deal with calling the routines to create a simulation box, running simulations, and modifying simulation boxes.

The supported global commands are explained in the subsections below.

### box

### run
`run [string ensemble] [integer max steps]`

ensemble can be NVT (ps or fts), VT (fts only), or findSpinodal (fts only)

Options:  
`NVT [integer max steps]`
`VT [integer max steps]` (fts only, to allow $\mu$VT simulations)
`findSpinodal [options]` (fts only, options include phiTolerance, fScale, molecule, nSteps, initial_f)

### modify



## Sample input files
