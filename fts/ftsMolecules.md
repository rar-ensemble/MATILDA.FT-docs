# FTS Molecules

## Linear discrete Gaussian chains

## Liquid crystals
`molecule liquid-crystal phi [volume fraction] [int number of sites] [text list of species per site] [optional arguments]`

Liquid crystals are intended (for now) to be small, rigid, linear molecules, and each molecule has the same 'smearing' function.

Optional arguments:  
- `lc_style [text]`: Defines whether the LC potential applies on the 'middle' or 'all' sites. Default: 'middle'  
- `smear gaussian [sigma]`: smears each site over a unit Gaussian with std dev $\sigma$. Default: disabled.

```bash
molecule liquid-crystal phi 1.0 3 A A A
```

