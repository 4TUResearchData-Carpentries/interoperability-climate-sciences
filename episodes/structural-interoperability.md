---
title: "Structural interoperability"
teaching: 60  # teaching time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are community formats?
- How do community formats contribute to interoperability in climate science data?
- What are some key community formats used in climate science?
- What is NetCDF and why is it widely used in climate science?
- How does NetCDF enable structural and semantic interoperability?
- What are the key features of NetCDF that support interoperability across platforms and tools?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the role of community formats in achieving interoperability.
- Identify key community formats used in climate science data.
- Explain the role of NetCDF in achieving interoperability in climate science data.
- Identify the structural and semantic features of NetCDF that facilitate data integration.
- Demonstrate how to inspect NetCDF files for interoperability features.

::::::::::::::::::::::::::::::::::::::::::::::::

## Community standards

In this section, we will explore how community formats, standardized metadata conventions, stable APIs, catalogs, and cloud-native layouts contribute to interoperability in climate science data.

Content
    • Formats arise from community consensus (Unidata, Pangeo, CF, OGC, WMO)
    • They encode stable expectations about arrays, metadata, and dimensions
Key formats:
    • NetCDF – multidimensional, self-describing, CF support
    • Zarr – cloud-native, chunked, distributed
    • Parquet – tabular, ideal for metadata, catalogs, station data
Why community formats matter
    • Predictable structure
    • Shared vocabulary
    • Long-term sustainability
    • Machine-actionability
    • Foundation for reproducible workflows

## NetCDF

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

- Community formats are data formats widely adopted and maintained by scientific communities to ensure interoperability, consistency, and usability of datasets.
- Key community formats in climate science include NetCDF for multidimensional data, Zarr for cloud-native storage, and Parquet for tabular data.
- Using community formats facilitates data sharing, integration, and long-term preservation by providing predictable structures and shared vocabularies.

- NetCDF is a self-describing, multidimensional data format widely used in climate science that enables structural interoperability through its predictable array structures and metadata conventions.
- The CF Conventions provide a semantic layer on top of NetCDF, standardizing variable names, units, and coordinate metadata to achieve semantic interoperability across diverse datasets.
- Widespread community adoption of NetCDF and CF, along with support from major tools and platforms, makes them foundational for interoperability in climate science data.

::::::::::::::::::::



