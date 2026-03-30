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

NetCDF provides strong structural interoperability in traditional computing environments,  
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
- A directory-like structure compatible with object storage  

Advantages in the cloud:

- Read only the chunks you need  
- Many workers can read simultaneously  
- Efficient for repeated slicing  

Zarr is considered **cloud-native** because it:

1. Reduces unnecessary data movement  
2. Enables scalable parallel processing  
3. Supports interactive analysis of very large datasets  



## What changes in Interoperability?

Cloud-native layouts mainly affect **structural interoperability**.

They change:

- How data is physically organized  
- How it is accessed  
- How scalable it is  

They do **not automatically change**:

- Variable names  
- Units  
- Coordinate conventions  

That belongs to **semantic interoperability**, which still relies on:

- CF conventions  
- Agreed metadata standards  

So:

- NetCDF → structural interoperability (file-based)  
- Zarr → structural interoperability (cloud-native)  

**Both can support semantic interoperability, but only if metadata conventions are respected.**



## Converting a dataset from file-based to cloud-native (hands-on session)

In this session, we will use Kerchunk to create a virtual Zarr dataset from an existing NetCDF file.  

This allows us to access the dataset in a cloud-native way **without modifying or copying the original data**.

:::::::::::::::::::::::::: instructor

This session can be delivered as a live-coding demonstration.  
Walk through each step, explain the concepts, and let learners follow along.

:::::::::::::::::::::::::::



### NetCDF → Virtual Zarr with Kerchunk

#### Goal

Create a cloud-native representation of an existing NetCDF file  
without rewriting or duplicating the data.


#### What is Kerchunk?

Kerchunk creates a **virtual Zarr dataset** from existing formats such as NetCDF or HDF5.

Instead of converting data, Kerchunk:

- Reads the original file structure  
- Maps byte ranges to Zarr-style chunk references  
- Writes a small JSON reference file  

Result:

- The original file remains unchanged  
- The JSON acts as a reference layer  
- No data duplication  
- No heavy conversion  
- Immediate cloud-compatible access  


#### Different ways of accessing remote data

| Approach | What happens                                                    | Who does the work |
|----------|------------------------------------------------------------------|------------------|
| OPeNDAP  | Server interprets the dataset and sends subsets                 | Server           |
| Kerchunk | Client reconstructs dataset structure and reads chunks directly | Client           |


#### Step 1: Create a Kerchunk reference

- Open JupyterLab in your project folder  
- Continue in your notebook or create a new one  

```python
import json
from kerchunk.netCDF3 import NetCDF3ToZarr
````

In this step, we are **not converting data**.
We are creating a JSON file that describes how to access the data.

> We are transforming **how data is accessed**, not the data itself.

```python
# Direct file endpoint (raw file access, not OPeNDAP)
file_url = "https://opendap.4tu.nl/thredds/fileServer/IDRA/2019/01/02/IDRA_2019-01-02_12-00_raw_data.nc"

# Inspect the NetCDF file and build a mapping:
# Zarr chunks → byte ranges in the original file
ref = NetCDF3ToZarr(file_url, inline_threshold=100).translate()

# Save the mapping as JSON (no data stored here)
with open("idra_ref.json", "w") as f:
    json.dump(ref, f)
```


#### Step 2: Open as a virtual Zarr dataset

`xarray` behaves as if it is reading a Zarr dataset,
but data is still coming from the original NetCDF file.

```python
import xarray as xr

ds_ref = xr.open_dataset(
    "reference://",
    engine="zarr",
    backend_kwargs={
        "consolidated": False,
        "storage_options": {
            "fo": "idra_ref.json"
        },
    },
)
```


#### Step 3: Inspect structure and metadata

The semantics remain the same.

```python
ds_ref.dims
ds_ref.variables
ds_ref.attrs
```

> Only the **access pattern** has changed, not the data meaning.


#### Step 4: Perform lazy slicing

```python
# Select a subset (only required chunks are accessed)
subset = ds_ref.isel(time=0)

subset
```

* No full dataset is loaded
* Only relevant chunks are accessed
* Access is lazy and chunk-based



#### Conceptual comparison

| Feature             | OPeNDAP                    | Kerchunk                    |
| ------------------- | -------------------------- | --------------------------- |
| Access type         | Protocol-based             | Storage-based               |
| Endpoint            | `/dodsC/`                  | `/fileServer/`              |
| Who interprets data | Server                     | Client                      |
| Data model          | File-oriented              | Chunk-oriented              |
| Scalability         | Limited by server          | Scales with client + cloud  |
| Parallel access     | Limited                    | Natural (chunk-based)       |
| Reusability         | Low                        | High (JSON reusable)        |
| Setup complexity    | Low                        | Medium                      |
| Performance         | Server + network dependent | Chunk-wise, parallel access |


## When to use what?

### Use OPeNDAP when:

* You want quick remote access
* You rely on existing data services (e.g., THREDDS)
* You are doing exploratory analysis
* You do not control data storage

Typical use: browsing datasets, small analyses


### Use Kerchunk when:

* You want cloud-native workflows
* You need scalable or parallel processing
* You work with large datasets
* You integrate with:

  * `zarr`
  * `dask`
  * object storage (S3, etc.)

Typical use: large-scale analysis, pipelines, reproducible workflows



:::::::::::::::::::::::::::: keypoints

* Cloud-native layouts are optimized for object storage and HTTP access.
* NetCDF works well on HPC systems but is not optimized for cloud-native environments.
* Zarr stores data in chunks, enabling efficient parallel access.
* Kerchunk enables cloud-native access to NetCDF without data duplication.
* Kerchunk changes the access pattern, not the data itself.
* Cloud-native layouts affect structural interoperability, while semantic interoperability depends on metadata standards such as CF conventions.

::::::::::::::::::::::::::::::::::::::::::::

```

