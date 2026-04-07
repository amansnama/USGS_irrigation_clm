# CTSM with USGS based Proportioning of Surface-Groundwater Irrigation

This project implements a USGS-based surface water to groundwater irrigation ratio into the Community Terrestrial Systems Model (CTSM), using gridded datasets derived from USGS HUC8-based irrigation data (2000–2020). The modification adjusts irrigation water sources in CLM5 according to observed ratios, providing a more realistic simulation of irrigation water use. It has been tested in the Mississippi River Basin with satisfactory results.

*USGS Irrigation withdrawal data source*:
- D. Martin et al., “Irrigation water use reanalysis for the 2000-20 period by HUC12, month, and year for the conterminous United States (ver. 2.0, September 2024).” U.S. Geological Survey, Sep. 26, 2024. doi: [10.5066/P9YWR0OJ](https://doi.org/10.5066/P9YWR0OJ).

## Features

- New namelist flag: `use_USGS_irrig_ratio`
- Implements logic to scale surface vs. groundwater irrigation to match USGS ratios
- Reads `USGS_mean` from surface dataset
- Developed on alpha-ctsm5.2.mksrf.23_ctsm5.1.dev171-1-g95dfcff30
- Compatible with CLM5.0+ and tested on NCAR's Derecho

## Background

In CLM5, surface water irrigates the river-threshold deficit volume deficit_volr whereas the remainder (deficit - deficit_volr) is fulfilled by groundwater irrigation. This modification scales deficit_volr by the observed surface irrigation ratio, upto the threshold volume available in the river.

## Repository Structure
```
.
├── ctsm_USGS_irrigation ← CTSM submodule with modified files
├── inputdata ← Surface datasets (NetCDF) and script to make them
├── modified-CTSM-files ← Modified CTSM files
├── README.log ← Detailed dev log
└── README.md ← You are here
```
### Modified CTSM files
```
    ├── bld
    │   └── namelist_files
    │       ├── namelist_defaults_ctsm.xml
    │       └── namelist_definition_ctsm.xml
    └── src
        ├── biogeophys
        │   └── IrrigationMod.F90
        └── main
            ├── ColumnType.F90
            ├── GridcellType.F90
            └── initVerticalMod.F90
```
## Getting Started

### 1. Get the updated CTSM files

Clone the repo with submodules
```bash
git clone --recurse-submodules https://github.com/amansnama/USGS_irrigation_clm.git
```
OR
Replace the original files with the files in `./modified-CTSM-files`.

See [CLM5 User Guide](https://escomp.github.io/ctsm-docs/versions/master/html/users_guide/index.html) for more details to run CLM5.

### 2. Required input dataset
Include USGS_mean variable in your surface dataset. USGS_mean dataset for the Continental United States at 0.5 and 0.05 deg is provided in the inputdata/ dir. Clip and merge it to your surface.

### 3. Set up a case using your modified CTSM
Create a new case. Use the merged surface dataset in your `user_nl_clm`.

## Developer Notes
For detailed design decisions, see [Developer README](README.log).

## Contact
Developed by Aman Shrestha (shrest66@msu.edu)

This work is part of a research effort to improve groundwater-surface water interaction representation in CLM5.