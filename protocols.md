---
title: "Technical interoperability: Streaming protocols"
teaching: 30 # FIXME teaching time in minutes
exercises: 10 # FIXME exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is the DAP protocol?
- Why is DAP an example of interoperability?
- How to access a NetCDF file using OpenDAP interface, via DAP protocol?
- How to read a NetCDF file programatically, using DAP protocol - with `open_dataset` from `xarray` Python library.
- How to explore and manipulate a NetCDF file programatically.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand why DAP is an interoperable protocol.
- Know how to access and read a NetCDF file using DAP protocol.

::::::::::::::::::::::::::::::::::::::::::::::::

Content

✔ Streaming protocols = Technical interoperability
OPeNDAP / DAP allow:

    • Access without downloading
    • Server-side subsetting
    • Efficient slicing for large NetCDF
    
✔ Why this matters
    • Enables scalable workflows (ERA5, ORAS5, CMIP6)
    • Facilitates AI training pipelines

Hands-on Exercise (Python)
    • Use Python client to read remote NetCDF via OPeNDAP
    • Perform a small selection (time window, variable subset)
    • Compare cost vs full file download


::::::::::::::::::::::::::::::::::::: challenge

### Exercise: TRUE or FALSE?
Is this statement `true` or `false`?
> The `xarray.open_data()` function you used, has downladed the dataset file to your computer.
Whay do you think so?

:::::::::::::::::::: solution

### Solution
No, the data has been accessed with the DAP protocol, which allows to explore and summarise the dimensions of the data, but they have not been downloaded to the computer.

:::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::: keypoints

- OPeNDAP (DAP) is a protocol that enables remote access to subsets of scientific datasets without downloading entire files, exemplifying technical interoperability.
- Using OPeNDAP allows efficient server-side subsetting and slicing of large NetCDF files, facilitating scalable workflows for large climate datasets.
- Programmatic access to NetCDF files via OPeNDAP can be achieved using libraries like xarray, enabling efficient data exploration and manipulation.


::::::::::::::::::::