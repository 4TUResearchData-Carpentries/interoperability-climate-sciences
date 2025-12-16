---
title: "NetCDF as a Model of Interoperability"
teaching: 40 # teaching time in minutes
exercises: 20 # exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is NetCDF and why is it widely used in climate science?
- How does NetCDF enable structural and semantic interoperability?
- What are the key features of NetCDF that support interoperability across platforms and tools?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

By the end of this episode, learners will be able to:
- Explain the role of NetCDF in achieving interoperability in climate science data.
- Identify the structural and semantic features of NetCDF that facilitate data integration.
- Demonstrate how to inspect NetCDF files for interoperability features.

::::::::::::::::::::::::::::::::::::::::::::::::

Content
✔ 1. Self-describing structure
    • Dimensions, variables, coordinates
    • Units, long_name, missing values
    • No external schema needed
    • Structural interoperability
✔ 2. Semantic layer: CF Conventions
    • Standardized variable names
    • Units
    • Grid mappings
    • Coordinate metadata
    • WHY this enables integration of ERA5 + CMIP6 + ICON + WRF
✔ 3. Massive community adoption
    • Used across climate, atmospheric, ocean, hydrology, geophysics
    • Supported by all major tools (xarray, MATLAB, R, Fortran)
    • NetCDF + CF = semantic interoperability backbone
✔ 4. Interoperability across platforms
    • HPC, Linux, macOS, cloud via OPeNDAP
    • Repositories: THREDDS, ERDDAP, 4TU

Hands-on Exercise
Participants:
    1. Open a NetCDF file (from the 4TU interface)
    2. Identify variable metadata
    3. Identify CF attributes
    4. Detect missing or inconsistent attributes


:::::::::: keypoints

- NetCDF is a self-describing, multidimensional data format widely used in climate science that enables structural interoperability through its predictable array structures and metadata conventions.
- The CF Conventions provide a semantic layer on top of NetCDF, standardizing variable names, units, and coordinate metadata to achieve semantic interoperability across diverse datasets.
- Widespread community adoption of NetCDF and CF, along with support from major tools and platforms, makes them foundational for interoperability in climate science data.

::::::::::::::::::::