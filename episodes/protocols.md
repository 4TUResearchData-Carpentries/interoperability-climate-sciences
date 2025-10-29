---
title: "Protocols to retrieve web-hosted data"
teaching: 0 # FIXME teaching time in minutes
exercises: 0 # FIXME exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do you write a lesson using Markdown and `{sandpaper}`?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the DAP protocol to access web hosted netcdf data
- Access a NetCDF file using OpenDAP interface, via DAP protocol.(to be discussed)
- Read a NetCDF file programatically, using DAP protocol - with `open_dataset` from `xarray` Python library.
- Explore and manipulate a NetCDF file programatically.

::::::::::::::::::::::::::::::::::::::::::::::::


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
