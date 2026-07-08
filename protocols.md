---
title: "Technical interoperability: Data access protocols"
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
- [OPeNDAP](https://www.opendap.org/) 
- [OGC](https://www.ogc.org/) services  

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

The [**Data Access Protocol (DAP)**](https://opendap.github.io/dap4-specification/DAP4.html) is a protocol designed to enable remote access to structured scientific data.

[**OPeNDAP**](https://www.opendap.org/) is a widely adopted implementation of DAP.

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
- Jupyter Lab 

### Step 1 – Open a remote dataset

- Open Jupyter Lab and choose the appropiate environment of the lesson (see [Setup](../learners/setup.md))

- Launch Jupyter Lab, open a terminal and type:

```bash

jupyter lab

```

- Open a new notebook 
- Check installed libraries

```python
import xarray as xr
```

- Open a dataset

```python

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

- Now lets select a variable → "spectrum_width", using positional indexing and we will take a 10×10 subset along two dimensions. 

```python

ds["spectrum_width"].isel(time_processed_data=slice(0,10),range=slice(0,10))

```
- Now lets print the values of this subsetting

```python

ds["spectrum_width"].isel(time_processed_data=slice(0,10),range=slice(0,10)).values # to print values in the scren

```

- Slicing by the names of the dimensions

```python

ds["spectrum_width"].sel(
    time_processed_data=slice("2019-01-02T12:00:00.000000000", "2019-01-02T12:00:02.097152173"),
    range=slice(0, 1000)
)

```

- Using `head`

```python

ds["spectrum_width"].head()
ds["spectrum_width"].head(time_processed_data=10)
ds["spectrum_width"].head(range=2)
ds["spectrum_width"].head(range=2).to_pandas() # tabular view

```

```python

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

ds["spectrum_width"].head(range=10).plot()

```

You have multiple equivalent ways to express the same operation:

`.isel()` → positional slicing (what you used)
`.sel()` → coordinate-aware slicing
`.head()` → quick inspection
`.values` → raw data extraction
`.plot()` → visual interpretation



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


:::::::::::::::::::::: callout

## Demo: Can Ash combine two IDRA radar datasets?

Ash has found two IDRA radar files exposed through OPeNDAP:

* `IDRA_2009-04-27_06-08_raw_data.nc`
* `IDRA_2019-01-02_12-00_raw_data.nc`

Both files come from the same radar system and both are available remotely through OPeNDAP. At first, this suggests that they should be easy to compare. But before Ash can combine them, she needs to inspect whether they are structurally and semantically compatible.

In this demo, we will compare the two files using Python and `xarray`.

### Questions

* Can we open both datasets remotely without downloading the full files?
* Do both datasets contain the same variables?
* Do the key radar variables have the same dimensions and units?
* Can we extract the same variable from both years?
* Can we combine a selected variable into one analysis-ready object?
* Why might we want to save this combined subset as Zarr?

### Objectives

After this demo, learners should be able to:

* open NetCDF files remotely through OPeNDAP;
* inspect dimensions, variables, units, and metadata;
* compare the structure of two related datasets;
* select a common radar variable from both datasets;
* create a small combined dataset for comparison;
* understand why Zarr can be useful for repeated or scalable analysis.

### Setup

```python
import xarray as xr
import pandas as pd
```

We use the OPeNDAP data URLs, not the `.html` inspection pages.

```python
url_2009 = "https://opendap.4tu.nl/thredds/dodsC/IDRA/2009/04/27/IDRA_2009-04-27_06-08_raw_data.nc"
url_2019 = "https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/01/02/IDRA_2019-01-02_12-00_raw_data.nc"
```

### Step 1: Open the datasets remotely

```python
ds_2009 = xr.open_dataset(url_2009)
ds_2019 = xr.open_dataset(url_2019)
```

```python
ds_2009
```

```python
ds_2019
```

At this point, Ash has not manually downloaded the full files. She is inspecting the datasets remotely through OPeNDAP.

### Step 2: Inspect dimensions

```python
ds_2009.dims
```

```python
ds_2019.dims
```

Ash checks whether both files organise the data in a similar way. For example, she expects to see dimensions such as:

* `time_raw_data`
* `sample_beat_signal`
* `time_processed_data`
* `range`

This matters because variables can only be compared directly if their dimensions are compatible.

### Step 3: Compare variable names

```python
vars_2009 = set(ds_2009.data_vars)
vars_2019 = set(ds_2019.data_vars)

common_vars = sorted(vars_2009.intersection(vars_2019))
only_2009 = sorted(vars_2009.difference(vars_2019))
only_2019 = sorted(vars_2019.difference(vars_2009))

print("Common variables:")
print(common_vars)

print("\nOnly in 2009:")
print(only_2009)

print("\nOnly in 2019:")
print(only_2019)
```

This is Ash’s first interoperability check. If the files do not contain the same variables, she cannot simply reuse the same analysis code for both years.

### Step 4: Inspect key radar variables

Ash focuses on a few processed radar observables:

```python
radar_variables = [
    "equivalent_reflectivity_factor",
    "differential_reflectivity",
    "radial_velocity",
    "spectrum_width",
    "differential_phase",
]
```

```python
for var in radar_variables:
    print(f"\nVariable: {var}")
    print("2009 dimensions:", ds_2009[var].dims)
    print("2019 dimensions:", ds_2019[var].dims)
    print("2009 units:", ds_2009[var].attrs.get("units"))
    print("2019 units:", ds_2019[var].attrs.get("units"))
```

This check helps Ash answer practical questions:

* Does the same variable exist in both files?
* Is it organised over the same dimensions?
* Are the units the same?
* Is the meaning of the variable described in metadata?

For example, `equivalent_reflectivity_factor` is a radar variable related to precipitation, but it is not the same as rainfall amount. It is usually expressed in `dBZ`, while rainfall amount may be expressed in units such as `mm` or `mm h-1`.

### Step 5: Select one variable for comparison

Ash starts with `equivalent_reflectivity_factor`.

```python
var = "equivalent_reflectivity_factor"

refl_2009 = ds_2009[var]
refl_2019 = ds_2019[var]
```

```python
refl_2009
```

```python
refl_2019
```

Now she checks whether both arrays use the same dimensions.

```python
print(refl_2009.dims)
print(refl_2019.dims)
```

If both use `time_processed_data` and `range`, Ash can compare them more easily.

### Step 6: Create a small subset

To keep the demo fast, Ash selects only the first few time steps and the first part of the range dimension.

```python
subset_2009 = refl_2009.isel(time_processed_data=slice(0, 20), range=slice(0, 100))
subset_2019 = refl_2019.isel(time_processed_data=slice(0, 20), range=slice(0, 100))
```

This is an important practical benefit of OPeNDAP: Ash can request a subset of the remote data instead of downloading everything manually.

### Step 7: Add a year coordinate and combine the subsets

```python
subset_2009 = subset_2009.expand_dims(year=[2009])
subset_2019 = subset_2019.expand_dims(year=[2019])
```

```python
combined = xr.concat([subset_2009, subset_2019], dim="year")
combined
```

Ash now has one small combined object containing the same radar variable from two different years.

```python
combined.name = "equivalent_reflectivity_factor"

combined_ds = combined.to_dataset()

combined_ds
```

### Step 8: Add useful metadata

```python
combined_ds.attrs["title"] = "Small combined IDRA reflectivity subset for interoperability demo"
combined_ds.attrs["source_datasets"] = "IDRA OPeNDAP files from 2009-04-27 and 2019-01-02"
combined_ds.attrs["purpose"] = "Demonstration of remote access, variable inspection, subsetting, and combination"
combined_ds.attrs["warning"] = (
    "This is a small teaching subset. It is not a complete scientific rainfall or drizzle analysis."
)
```

This step shows learners that combining data is not only a technical operation. Ash also needs to preserve enough metadata to explain where the data came from and what processing decisions were made.

### Step 9: Save the combined subset as Zarr

For repeated analysis, Ash may want to store the small combined subset in a format that is efficient for chunked, cloud-friendly access.

```python
combined_ds.to_zarr("idra_reflectivity_subset.zarr", mode="w")
```

Later, she can reopen it directly:

```python
reopened = xr.open_zarr("idra_reflectivity_subset.zarr")
reopened
```

This creates a small analysis-ready version of the subset. Instead of repeating the same remote access and harmonisation steps every time, Ash can reuse the prepared Zarr version in later notebooks or workflows.

### Discussion

This demo shows three different interoperability layers.

First, **technical interoperability**: OPeNDAP allows Ash to access the files remotely and subset the data programmatically.

Second, **structural interoperability**: NetCDF exposes dimensions, variables, coordinates, and attributes in a predictable structure.

Third, **workflow interoperability**: Zarr allows Ash to store a prepared subset in a chunked format that can be reopened and reused in later analysis.

However, the demo also shows that interoperability is not automatic. Ash still has to inspect the variables, units, dimensions, coordinates, metadata, and provenance before deciding whether the datasets can be compared safely.

::::::::::::::::::::: instructor

You can use this part for discussion in the group. 

### Exercise: What would Ash need to check before trusting the comparison?

In small groups, inspect the two datasets and answer the following questions.

1. Do both files contain the same processed radar variables?
2. Do the variables use the same dimensions?
3. Are the units the same across the two files?
4. Are the time coordinates encoded in the same way?
5. Is the `range` coordinate comparable across both years?
6. Which metadata attributes help Ash understand the origin of the data?
7. Which metadata attributes are still missing or unclear?
8. Would you trust a direct comparison of `equivalent_reflectivity_factor` across these two files? Why or why not?
9. What would you document before sharing the combined subset with another researcher?
10. What would be the benefit of storing the harmonised subset as Zarr?

### Key message

Ash can access both IDRA files through OPeNDAP, inspect their NetCDF structure, select the same radar variable, and create a small combined subset. But meaningful comparison still depends on metadata, units, dimensions, coordinates, provenance, and clear documentation of the processing steps.

This is the practical meaning of interoperability: different datasets become useful together only when software can access them, humans can understand them, and workflows can reuse them reliably.


::::::::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::: keypoints

- Technical interoperability enables machine-to-machine data exchange through standardized protocols.

- OPeNDAP implements the DAP protocol for remote access to structured scientific datasets.

- Remote datasets can be explored without full download.

- Server-side subsetting reduces bandwidth and supports scalable workflows.

- Streaming protocols transform data repositories into interoperable computational infrastructure.

::::::::::
