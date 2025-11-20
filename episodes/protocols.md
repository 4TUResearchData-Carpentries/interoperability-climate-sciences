---
title: "Technical interoperability: DAP protocol"
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

## What is 


::::::::::::::::::::::::::::::::::::: challenge

### Exercise: TRUE or FALSE?
Is this statement `true` or `false`?
> The `xarray.open_data()` function you used, has downladed the dataset file to
your computer.
Why do you think so?

:::::::::::::::::::: solution

### Solution
No, the data has been accessed with the DAP protocol, 
which allows to explore and summarise the dimensions of the data, 
but they have not been downloaded to the computer.

:::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::: keypoints

- DAP is a protocol that enables remote access to subsets of NetCDF files without needing to download the full dataset.
- DAP supports server-side subsetting and slicing of large NetCDF files, enabling scalable workflows for large climate and Earth-system datasets.
- Programmatic access to NetCDF via DAP lets tools like `xarray` python library 
  load, subset, and analyse datasets efficiently..


::::::::::::::::::::
