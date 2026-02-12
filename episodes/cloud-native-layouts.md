---
title: "Cloud-Native Layouts"
teaching: 20
exercises: 25
---

:::::::::::::::::::::::::::::::::::::: questions  

- What does “cloud-native” mean in the context of scientific data?
- Why can NetCDF struggle in cloud environments?
- How is Zarr different from NetCDF?
- Which part of interoperability is affected by cloud-native layouts?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain what makes a data format cloud-native.
- Compare NetCDF and Zarr from a cloud-access perspective.
- Identify how cloud-native layouts influence structural interoperability.
- Create a virtual Zarr dataset from NetCDF using Kerchunk.

::::::::::::::::::::::::::::::::::::::::::::::::

## Cloud-Native Layouts

### What Does “Cloud-Native” Mean?

Cloud-native data layouts are designed for:

- Object storage (e.g., S3-compatible systems)
- Access over HTTP
- Parallel reads
- Loading only the pieces of data you need (lazy access)

In climate science, datasets such as:

- ERA5 (ECMWF reanalysis dataset)
- CMIP6 (climate model intercomparison dataset)

are often terabytes to petabytes in size.

Typical workflows include:

- Reading a single variable  
- Selecting one time slice  
- Extracting a spatial subset  
- Repeating this many times (e.g., for machine learning)

A cloud-native layout makes these repeated small reads efficient.



## NetCDF vs Zarr (Cloud Perspective)

### NetCDF

NetCDF (Network Common Data Form) is a widely used scientific data format.

Designed for:

- HPC systems
- Large files on shared storage
- Sequential or file-based access

In the cloud:

- Stored as a single binary file
- Harder to parallelize over HTTP
- Repeated slicing can be inefficient

NetCDF provides strong structural interoperability in traditional computing environments —  
but it is not optimized for object storage systems.


### Zarr

Zarr is a chunked, cloud-optimized array storage format.

Designed for:

- Object storage
- Many small chunks
- Parallel HTTP access

Data is stored as:

- Small chunk files
- JSON metadata
- A directory-like structure compatible with cloud object stores

Advantages in the cloud:

- Read only the chunks you need
- Many workers can read simultaneously
- Efficient for repeated slicing

Zarr is therefore considered **cloud-native**.



## What Changes in Interoperability?

Cloud-native layouts mainly affect: Structural Interoperability

They change:

- How data is physically organized
- How it is accessed
- How scalable it is

They do **not automatically change**:

- Variable names
- Units
- Coordinate conventions

That part belongs to **semantic interoperability**, which still relies on:

- CF conventions
- Agreed metadata standards

So:

- NetCDF → structural interoperability (file-based)
- Zarr → structural interoperability (cloud-native)

Both can support semantic interoperability —  
but only if metadata conventions are respected.



## Relevance for climate workflows

Climate analysis increasingly runs in:

- Cloud notebooks
- Distributed systems
- Data-proximate compute environments

Cloud-native layouts:

- Reduce unnecessary data movement
- Enable scalable parallel processing
- Support interactive analysis of very large datasets

They allow structural interoperability to scale to modern data volumes.



## Hands-on session 
### NetCDF → Virtual Zarr with Kerchunk

#### Goal

Create a cloud-native representation of an existing NetCDF file  
without rewriting or duplicating the data.


## What is Kerchunk?

Kerchunk is a Python library that creates a **virtual Zarr dataset**  
from existing formats such as NetCDF or HDF5.

Instead of converting the file physically, Kerchunk:

- Scans the original NetCDF file
- Maps byte ranges to Zarr-style chunk references
- Writes a small JSON reference file

This JSON file behaves like a Zarr store.

Result:

- No data duplication
- No heavy conversion
- Immediate cloud-compatible access

Kerchunk improves structural interoperability  
by making legacy NetCDF archives accessible in cloud-native workflows.


### Step 1: Create a Kerchunk reference

This creates a small JSON file describing how the NetCDF file can be accessed as a Zarr dataset.

```python

import json
import fsspec
from kerchunk.hdf import SingleHdf5ToZarr

filename = "https://opendap.4tu.nl/thredds/dodsC/IDRA/2009/04/27/IDRA_2009-04-27_06-08_raw_data.nc"

with fsspec.open(url, mode="rb") as f:
    h5chunks = SingleHdf5ToZarr(f, url)
    reference = h5chunks.translate()

with open("example_reference.json", "w") as f:
    json.dump(reference, f)


```
### Step 1 : Open as virtual Zarr 

```python

import xarray as xr

ds = xr.open_dataset(
    "example_reference.json",
    engine="zarr",
    backend_kwargs={"consolidated": False}
)

ds


```

You now have a Zarr-style dataset without converting the original NetCDF file.

### Step 3: Inspect structure and metadata 

The semantics should remains the same. 

```python

ds.dims
ds.variables
ds.attrs


```

### Step 4 : Perfrom lazy slicing 

```python

subset = ds.isel(time=0)
subset

```

Notice that: Data is loaded lazily and only the required chunk is accessed.

It is the same dataset, same metadata but a different structural layout which offers better scalability in cloud environments.



:::::::::::::::::::::::::::: keypoints

- Cloud-native layouts are optimized for object storage and HTTP access.

- NetCDF works well on HPC systems but is not optimized for cloud-native access.

- Zarr stores data in chunks, enabling efficient parallel reads in cloud environments.

- Kerchunk enables cloud-style access to existing NetCDF archives without data duplication.

- Cloud-native layouts mainly influence structural interoperability, while semantic interoperability still depends on metadata standards such as CF conventions.

::::::::::::::::::::::::::::::::::::::::::::
