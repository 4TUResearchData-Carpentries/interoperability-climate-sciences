---
title: "Technical interoperability: Streaming protocols"
teaching: 30
exercises: 15 
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is technical interoperability?
- What is the DAP (Data Access Protocol)?
- How does OPeNDAP enable remote access without full download?
- What happens when we open a remote NetCDF file using `xarray.open_dataset()`?
- Why are streaming protocols essential for large-scale scientific workflows?

:::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

By the end of this episode, learners will be able to:

- Define technical interoperability in the context of scientific data infrastructures.
- Explain how DAP enables interoperable machine-to-machine data access.
- Access a remote NetCDF dataset via OPeNDAP using Python.
- Perform server-side subsetting of variables and dimensions.
- Distinguish between metadata access and actual data transfer.

::::::::::::::::::::::::::::::::::::::::::::::::

## What is technical interoperability?

Technical interoperability concerns **machine-to-machine communication**.

A system is technically interoperable when independent systems can exchange and access data through standardized protocols without manual intervention.

If structural interoperability answers:

> “Can I read this file?”

Technical interoperability answers:

> “Can I access and exchange this data across systems in a scalable way?”

This layer operates below semantics.  
It is about **transport, protocol, and infrastructure**.

Examples include:

- HTTP  
- REST APIs  
- OPeNDAP  
- OGC services  

In scientific data infrastructures, technical interoperability enables remote analysis workflows.



## Why file download is not scalable

Large scientific datasets (climate reanalysis, ocean models, satellite archives) often reach:

- Tens of gigabytes  
- Terabytes  
- Petabytes  

Downloading entire files:

- Is inefficient  
- Consumes bandwidth  
- Duplicates storage  
- Breaks reproducibility pipelines  

Modern workflows require:

- Remote access  
- Server-side filtering  
- On-demand subsetting  
- Integration into automated pipelines  

This is where streaming protocols become essential.


## DAP and OPeNDAP

The **Data Access Protocol (DAP)** is a protocol designed to enable remote access to structured scientific data.

**OPeNDAP** is a widely adopted implementation of DAP.

DAP allows:

- Access to metadata without full download  
- Server-side slicing (e.g., select time range, variable subset)  
- Transmission of only requested data  

In practice, this means:

You interact with a dataset hosted on a remote server as if it were local — but only the necessary data is transferred.

This is technical interoperability in action.


## Hands-on: Accessing NetCDF via OPeNDAP in Python

We now move from concept to practice.

We will use:

- `xarray`
- A remote OPeNDAP endpoint
- A NetCDF dataset hosted on a THREDDS server

### Step 1 – Open a remote dataset

- Open the terminal and the python interpreter

```bash

python3

```

- Then write the following lines. Hit `Enter` after each line. 

```python
import xarray as xr

url = "https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/01/02/IDRA_2019-01-02_12-00_raw_data.nc"

ds = xr.open_dataset(url,engine="pydap")

ds

```

::::::::::::::::: instructor

Most of the cases , a warning is prompted. This warning is normal when using pydap with a THREDDS OPeNDAP server. It is not an error and your dataset should still load correctly. The warning simply means that PyDAP could not detect whether the server supports DAP2 or DAP4, so it defaults to DAP2, which is the older protocol.

The OPeNDAP protocol has two main versions:

DAP2 – legacy but widely supported (many THREDDS servers still use it)
DAP4 – newer, more efficient protocol

PyDAP tries to infer the protocol automatically. If it cannot, it falls back to DAP2, which triggers the warning.The server (opendap.4tu.nl) is a THREDDS server, and these typically expose DAP2 endpoints, so this behavior is expected.

:::::::::::::::::

::::::::::::::::::::::: instructor

You can go back to the exercise of the Episode of structural interoperability : **Identify the structural elements in a NetCDF file**
:::::::::::::::::::::::


Observe:

- The dataset structure loads immediately.

- Dimensions and metadata are visible.

- The file has not been fully downloaded.

What happened?

Only metadata and coordinate information were accessed.

### Step 2 – Select a variable

```python
ds["spectrum_width"] # still no full download, just metadata
```

### Step 3 – Perform server-side subsetting

- Actual data transfer occurs

```python

ds["spectrum_width"].isel(time_processed_data=slice(0,10),range=slice(0,10))

ds["spectrum_width"].isel(time_processed_data=slice(0,10),range=slice(0,10)).values # to print values in the scren

ds["spectrum_width"].isel(time_processed_data=0).values #one radar profile (1D slice)

ds["spectrum_width"].isel(range=1).values # One time series  



```

Now actual data transfer occurs — but only for:

- One variable

- A limited time window

This is server-side subsetting enabled by DAP.

### Step 4 Plotting a profile 

```python

import matplotlib.pyplot as plt 

ds["spectrum_width"].isel(time_processed_data=0).plot()

plt.show()

```
## Relevance for resarch workflows


Streaming protocols enable:

- Scalable climate analysis (ERA5, CMIP6)

- AI/ML training pipelines

- Reproducible notebooks
- Cloud-based workflows
- Data repository integration

Technical interoperability ensures that:

Data repositories are not only storage systems, they become computational infrastructure.

::::::::::::::::::::::::::::::::::::: challenge

## Technical interoperability — True or False?

Indicate whether each statement is True or False and justify your answer.

- Opening a remote dataset with xarray.open_dataset() automatically downloads the entire file.

- DAP enables server-side filtering before data transfer.

- Streaming protocols replace the need for structural interoperability.

- OPeNDAP works independently of file formats.

- Technical interoperability enables automated workflows across infrastructures.


:::::::::::::::::::::::::::::::::::::::::::::: solution

**False**. Only metadata is accessed initially; data is transferred upon explicit selection.

**True**. Subsetting occurs on the server before transmission.

**False**. Technical interoperability depends on structural interoperability.

**False**. DAP operates on structured data models (e.g., NetCDF).

**True**. It enables scalable machine-to-machine access. 
 
::::::::::::::::::::::::::::::::::::::::::::::::::::::: 


::::::::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::: keypoints

- Technical interoperability enables machine-to-machine data exchange through standardized protocols.

- OPeNDAP implements the DAP protocol for remote access to structured scientific datasets.

- Remote datasets can be explored without full download.

- Server-side subsetting reduces bandwidth and supports scalable workflows.

- Streaming protocols transform data repositories into interoperable computational infrastructure.

::::::::::