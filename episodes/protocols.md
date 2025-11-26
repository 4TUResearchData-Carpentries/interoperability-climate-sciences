---
title: "Technical interoperability: DAP protocol"
teaching: 30 # FIXME teaching time in minutes
exercises: 10 # FIXME exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is the DAP protocol?
- Why is DAP an example of technical interoperability?
- How to access a NetCDF file using OPeNDAP interface, via DAP protocol?
- How to open and read a NetCDF file programmatically via DAP protocol using `open_dataset`
from `xarray` Python library.
- How to explore and manipulate a NetCDF dataset in Python.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
After completing this episode, learners will be able to:

- Explain why DAP protocol enables technical interoperability across tools and platforms.

- Open and inspect a remote NetCDF dataset using OPeNDAP (DAP) and a DAP URL in Python.

- Programmatically extract variables or subsets of data from a remote NetCDF dataset using `xarray`.

- Understand the difference between accessing data via DAP and downloading a full dataset.

::::::::::::::::::::::::::::::::::::::::::::::::


Earth and atmospheric sciences datasets commonly stored in NetCDF format often reach sizes of several gigabytes or more. Downloading these files just to inspect or extract a small portion can be inefficient, slow, or even impossible for users with limited local resources.

To enable more efficient access, the Earth science community often exposes NetCDF files through **DAP** (Data Access Protocol). DAP is a web-based protocol that allows analysis softwares (for example specific libraries of Python or R) to request only metadata or specific subsets of a dataset. This makes it possible to work with remote datasets almost as if they were local, without downloading entire files.

This capability is what makes DAP a core technology for technical interoperability: the same DAP URL can be opened by dozens of software tools (Python, R, MATLAB, Panoply, Ferret, browsers, command line tools), all using the same protocol and all accessing the same underlying data. The protocol acts as a consistent interface across platforms, programming languages, and institutions.

DAP is commonly accessed through an implementation called **OPeNDAP**.
OPeNDAP is the software system that implements the DAP protocol and allows servers to publish NetCDF files for remote access.



## What is the DAP Protocol 

The Data Access Protocol (DAP) is a web-based protocol used to access structured scientific data (such as NetCDF files) over the internet. It allows your programme to:

- request dataset metadata

- request individual variables

- subset variables by dimension

- extract slices or ranges of data

A dataset served via DAP typically has a URL ending in something like:

```
.../thredds/dodsC/.../file.nc
```

This is not downloaded like a normal file. Instead, it is accessed through the DAP protocol.

## Accessing a NetCDF File via DAP
In Python, the most commonly used library for working with NetCDF and multidimensional labeled arrays is [`xarray`](https://docs.xarray.dev/en/stable/index.html).
`xarray` has built-in support for opening DAP/OPeNDAP URLs using the `netCDF4` engine, which allows remote datasets to be accessed just like local NetCDF files.

::::::::::::::::::::::::::::::::::::: callout
## IDRA dataset
The IDRA dataset contains daily weather-radar measurements from an X-band radar located on the Cabauw tower in the Netherlands. It provides detailed scans of precipitation around the tower—such as drizzle, rain, or fog—showing how it develops and moves over time. The files also include quick daily overviews and basic raw data, all stored in standard NetCDF format.

Read more :
- [4TU.ResearchData: IDRA documentation](https://data.4tu.nl/articles/_/12727727/3)
- [Data publication: 4TU.ResearchData: IDRA dataset](https://data.4tu.nl/articles/_/12696887/1)
:::::::::::::::::::::::::::::::::::::::::::::

The dataset is served via OPeNDAP at the following URL:

```
https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/10/06/IDRA_2019-10-06_standard_range.nc
```

```
import xarray as xr

# DAP URL of the IDRA dataset
url = "https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/10/06/IDRA_2019-10-06_standard_range.nc"

# Open the dataset with xarray
ds = xr.open_dataset(url)
ds
```

## Inspecting Metadata and Variables


::::::::::::::::::::::::::::::::::::: challenge

### Exercise: TRUE or FALSE?
Is the following statement `true` or `false`?
> The `xarray.open_dataset()` function you used, has downladed the dataset file to
your computer.
Why do you think so?

:::::::::::::::::::: solution

### Solution
No — the dataset has not been downloaded to your computer.

When you open a dataset using a DAP URL, `xarray` communicates with the remote server using the DAP protocol. DAP allows the client (your Python session) to request only metadata and specific variable subsets as needed. The full NetCDF file remains on the server; only small requested portions are transferred.

:::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::: keypoints

- DAP is a web-based protocol that enables remote access to NetCDF data without downloading the full file.
- DAP supports server-side subsetting, allowing clients to request only the specific variables, time ranges, or slices they need.
- Tools like xarray can open DAP endpoints using a URL, allowing efficient inspection, subsetting, and analysis of large datasets.
- This approach supports interoperable workflows, where many tools (e.g. R, Python, MATLAB) can access the same dataset consistently via the same protocol.


::::::::::::::::::::
