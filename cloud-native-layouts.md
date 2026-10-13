---
title: "Cloud-Native Layouts"
teaching: 20
exercises: 25
---

:::::::::::::::::::::::::::::::::::::: questions

- What problem are cloud-native data layouts designed to solve?
- What is object storage, and how does it differ from a traditional filesystem?
- Why is a conventional NetCDF file not considered a cloud-native layout?
- How does Zarr organise multidimensional data differently?
- How do cloud-native layouts affect interoperability?
- How can Kerchunk bridge existing NetCDF archives and cloud-oriented workflows?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

By the end of this episode, learners will be able to:

- Explain why large scientific datasets require efficient selective access.
- Describe the relationship between object storage, chunking, and cloud-native data layouts.
- Compare conventional NetCDF files and Zarr from a cloud-access perspective.
- Explain how cloud-native layouts affect structural and technical interoperability while preserving the need for semantic conventions.
- Create a virtual Zarr-compatible representation of an existing NetCDF dataset using Kerchunk.

::::::::::::::::::::::::::::::::::::::::::::::::


## Why do we need cloud-native data layouts?

Scientific datasets are becoming increasingly large. In climate and atmospheric sciences, a dataset may contain many variables, thousands of time steps, global spatial coverage, multiple vertical levels, and several model runs or ensemble members. Large data collections such as [ERA5](https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=overview) or [CMIP6](https://wcrp-cmip.org/cmip-phases/cmip6/) can extend across terabytes or petabytes of data.

Researchers, however, rarely need an entire collection for a particular analysis. A researcher may need only one variable, a short time period, one pressure level, or a small geographical region. For example: *Air temperature over the Netherlands during July 2025.* The full collection may be extremely large, while the requested subset represents only a small fraction of it. This creates an important data-access problem: **How can we efficiently retrieve only the parts of a very large dataset that we actually need?**


### The limits of a file-based access model

Traditional scientific workflows are often organised around files. A researcher finds a file on a repository or server, downloads or opens it, selects the required observations, and performs the analysis.

This model works well when files are reasonably small or when most of their contents are needed. It becomes less efficient when a large file must be accessed repeatedly for small subsets. A 20 GB global dataset, for example, may contain only 50 MB relevant to one region and one time period.

The same problem becomes more noticeable when an analysis repeatedly requests different subsets:

```text
temperature → January → Europe
temperature → February → Europe
precipitation → January → Europe
temperature → January → South America
```

Large-scale analyses and machine-learning workflows may perform many such reads. Modern data infrastructures therefore increasingly aim to make smaller parts of a dataset independently accessible. The conceptual shift is from: *"Give me this entire file."* Towards: *"Give me only the pieces of data I need."* This is the problem that **cloud-native data layouts** are designed to address.


## Cloud-native layouts and object storage

In this context, **cloud-native does not simply mean that data are stored in the cloud**. A large scientific file can be uploaded to cloud infrastructure without changing its internal organisation. If applications still interact with it as one large file, its access model has not fundamentally changed.

A **cloud-native data layout** organises data so that small parts of a dataset can be accessed efficiently and independently over a network. These layouts are particularly well suited to object storage and to selective or parallel remote access. The important distinction is therefore: *Data stored in the cloud isnt a Cloud-native data layout*. The key question is not only **where the data are stored**, but **how they are organised for access**.


### What is object storage?

Traditional scientific computing commonly uses a **filesystem**, where files are organised in directories and accessed through paths such as:*/data/climate/temperature_2025.nc.* Applications can open the file and navigate to different positions within it through filesystem operations.

Object storage uses a different model. Data are stored as independent **objects**, each identified by a key. These objects are commonly grouped into containers called **buckets**. Conceptually, an object store may contain:

```text
Storage bucket
│
├── climate/temperature/chunk_001
├── climate/temperature/chunk_002
├── climate/temperature/chunk_003
└── climate/temperature/metadata
```

The names may look hierarchical, but the apparent directory structure is generally constructed from object keys rather than from a traditional filesystem hierarchy. Applications interact with object storage through network interfaces. Amazon S3 is a well-known example, and many research infrastructures provide S3-compatible services. Because separate objects can be requested independently, object storage is well suited to distributed workflows in which several processes need different pieces of a dataset at the same time. This makes the **physical layout of the scientific data** especially important.


## NetCDF and Zarr from a cloud perspective

### NetCDF: a primarily file-oriented representation

NetCDF is a widely used scientific data format, particularly in climate, atmospheric, oceanographic, and Earth sciences. It provides an established multidimensional data model consisting of dimensions, variables, coordinates, and attributes. Conventional `.nc` files work very well on local computers, shared filesystems, and high-performance computing systems. In this episode, when we refer to NetCDF, we mean this conventional **NetCDF file representation**. A NetCDF dataset is commonly packaged into a single binary file. NetCDF-4 files may already contain internal chunks through their underlying HDF5 representation, but those chunks remain inside the same file.

```text
dataset.nc
│
├── dimensions
├── variables
├── metadata
└── data
```

If that file is uploaded to object storage, the storage system still sees one large object:

```text
bucket
│
└── dataset.nc
```

Remote access is still possible. Software may use HTTP byte-range requests or specialised data services to retrieve selected regions of the file. However, the conventional NetCDF representation was not designed so that portions of a multidimensional array become separate, independently addressable storage objects. For occasional remote access this may be entirely adequate. It becomes less convenient for workflows involving many repeated subsets, many parallel readers, or very large distributed collections.

### Zarr: a cloud-oriented chunked representation

Zarr is a format for storing multidimensional arrays together with the metadata needed to interpret them. Like NetCDF, it can represent scientific data with multiple dimensions and variables, but it uses a different physical storage model.

Instead of packaging the complete dataset inside one binary file, Zarr divides arrays into smaller pieces called **chunks**. Each chunk represents a region of a multidimensional array and can be read independently.

```text
Large multidimensional array

+---------+---------+---------+
| chunk 1 | chunk 2 | chunk 3 |
+---------+---------+---------+
| chunk 4 | chunk 5 | chunk 6 |
+---------+---------+---------+
| chunk 7 | chunk 8 | chunk 9 |
+---------+---------+---------+
```

A Zarr dataset also stores metadata describing the arrays, including their shape, data type, dimensions, and chunk structure. A simple storage representation may therefore look like:

```text
temperature.zarr/
│
├── metadata
├── chunk_0_0_0
├── chunk_0_0_1
├── chunk_0_1_0
├── chunk_0_1_1
└── ...
```

Zarr can be used on local or shared filesystems, but this organisation becomes especially useful in object storage. When a Zarr dataset is placed there, its chunks and metadata can be stored as independently addressable objects:

```text
bucket
│
└── temperature.zarr/
    ├── metadata
    ├── chunk_0_0_0
    ├── chunk_0_0_1
    ├── chunk_0_1_0
    ├── chunk_0_1_1
    └── ...
```

When a researcher requests a particular variable, time period, or geographical region, software can determine which chunks contain the required values and retrieve those chunks rather than the complete dataset. The same organisation supports parallel access because different applications or computational workers can request different chunks at the same time.

```text
             Zarr dataset
                  │
       ┌──────────┼──────────┐
       ▼          ▼          ▼
    worker 1   worker 2   worker 3
       │          │          │
    chunk A    chunk B    chunk C
```

The important point is not simply that Zarr uses chunks: NetCDF-4 can also use internal chunking. From a cloud perspective, the key difference is that a Zarr storage layout can expose portions of the arrays as **independently addressable parts of the storage system**. This close fit between chunked multidimensional arrays and independently accessible storage objects is what makes Zarr well suited to cloud and object-storage environments.

:::::::::::::::::::::::::: instructor

For an introductory lesson, it is sufficient to describe Zarr in terms of independently readable chunks. More advanced Zarr layouts can also use **sharding**, where several chunks are grouped into larger storage objects.

:::::::::::::::::::::::::::


### Why is it useful to know both NetCDF and Zarr?

Zarr should not be understood simply as a replacement for NetCDF. NetCDF remains fundamental to climate and atmospheric sciences, with large archives, established tools, repositories, conventions, and workflows built around it. For local computing and many HPC workflows, conventional NetCDF files remain an effective solution. Zarr becomes particularly relevant when multidimensional datasets need to be accessed repeatedly over a network, stored in object-storage infrastructure, or processed using distributed computing. The main distinction in this episode is therefore the **storage and access model**:

```text
NetCDF

multidimensional data
        │
        ▼
conventional file representation
        │
        ▼
filesystem or file-oriented remote access
```

```text
Zarr

multidimensional data
        │
        ▼
chunk-oriented representation
        │
        ▼
selective and parallel access
```

Both can represent multidimensional scientific data. What changes is how those data are physically organised and retrieved.



:::::::::::::::::::::::::: challenge

### Exercise: What changes in interoperability?

Imagine that the same scientific dataset is moved from a conventional NetCDF file representation to a cloud-native layout such as Zarr. **Think individually for 1–2 minutes, then discuss with a partner:**

Which aspects of interoperability change when moving from a conventional NetCDF file representation to a cloud-native layout such as Zarr? Which aspect is not automatically changed or improved by this move?

As you discuss, consider these three questions:

1. Does the **physical organisation of the data** change?
2. Does the **way software accesses the data** change?
3. Does the **scientific meaning of variables, units, and coordinates** automatically change?

Be prepared to explain your reasoning to the group.


:::::::::::::::::: solution

Moving from a conventional NetCDF file representation to a cloud-native layout such as Zarr mainly affects structural and technical interoperability.

Structural interoperability changes because the physical organisation of the data changes. A conventional NetCDF dataset is commonly packaged into a single file, whereas Zarr organises multidimensional arrays into chunks that can be stored and accessed independently.

Technical interoperability also changes because software can interact with the dataset differently. In a cloud-native layout, applications can retrieve only the chunks they need and multiple processes can access different chunks in parallel. This makes the dataset better aligned with object storage, HTTP-based access, and distributed computing.

Semantic interoperability is not automatically improved simply by changing the storage layout. The scientific meaning of the data still depends on metadata conventions, units, standard names, coordinate descriptions, and other semantic information. A conversion to Zarr can preserve the existing semantic metadata, but Zarr itself does not make a dataset semantically interoperable. If important metadata are missing or lost during conversion, semantic interoperability can still be poor.

::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::


## Hands-on: NetCDF → virtual Zarr-compatible access with Kerchunk

Many scientific archives already contain large collections of NetCDF files. Rewriting all of those datasets as Zarr may require additional storage, processing time, and changes to established preservation workflows. This raises another question: **Can we obtain a chunk-oriented access model without rewriting the original NetCDF data?**.One approach is **Kerchunk**. Kerchunk creates a reference description that allows existing files such as NetCDF or HDF5 to be viewed through a Zarr-compatible access model. It does not copy the scientific data into a new Zarr store. Instead, it inspects the source file, determines where portions of the data are located, and records references to the corresponding byte ranges.
The original NetCDF file remains unchanged. The reference description acts as a mapping layer between a chunk-oriented view of the dataset and the bytes stored in the original file. Kerchunk therefore changes **how existing data can be accessed**, rather than converting the data into a new physical Zarr copy.


In this exercise, we will create a Kerchunk reference for an existing NetCDF dataset and open that reference with `xarray`.

:::::::::::::::::::::::::: instructor

This activity works well as guided live coding.

The example below uses a NetCDF3 file, so `NetCDF3ToZarr` is used. NetCDF4/HDF5 datasets require the corresponding HDF5 translator.

Use the direct `/fileServer/` endpoint because Kerchunk needs access to the bytes of the original file rather than the `/dodsC/` OPeNDAP service.

:::::::::::::::::::::::::::


### Step 1: Create the Kerchunk reference

First import the required packages:

```python
import json
from kerchunk.netCDF3 import NetCDF3ToZarr
```

We will use the direct file endpoint for the IDRA dataset:

```python
file_url = "https://opendap.4tu.nl/thredds/fileServer/IDRA/2019/01/02/IDRA_2019-01-02_12-00_raw_data.nc"
```

Kerchunk inspects the NetCDF structure and creates references between a Zarr-compatible representation and byte ranges in the original file:

```python
ref = NetCDF3ToZarr(
    file_url,
    inline_threshold=100
).translate()
```

We then save the reference description as JSON:

```python
with open("idra_ref.json", "w") as f:
    json.dump(ref, f)
```

At this point, we have **not created another copy of the scientific data**. The JSON file describes how the original data can be found and interpreted.


### Step 2: Open the reference with xarray

We can now ask `xarray` to open the reference using the Kerchunk backend:

```python
import xarray as xr

ds_ref = xr.open_dataset(
    "idra_ref.json",
    engine="kerchunk",
    storage_options={
        "remote_protocol": "https",
        "remote_options": {
            "asynchronous": True,
        },
    },
)

ds_ref
```

From the researcher's perspective, the result behaves like an `xarray.Dataset`. The numerical data still reside in the original NetCDF file.


### Step 3: Inspect the structure and metadata

We can inspect the dataset in the same way as other `xarray` datasets:

```python
ds_ref.dims
ds_ref.variables
ds_ref.attrs
```

The access mechanism has changed, but the variables and metadata originate from the same underlying dataset.


### Step 4: Make a lazy selection

Select one time step:

```python
subset = ds_ref.isel(time_raw_data=0)

subset
```

This defines which portion of the dataset is required. It does not necessarily mean that all selected numerical values have already been transferred from the remote file.

To explicitly trigger retrieval of the selected data, use:

```python
subset.load()
```

## OPeNDAP and Kerchunk solve related problems differently

Earlier in the lesson, we used **OPeNDAP** to access subsets of NetCDF datasets remotely. Both OPeNDAP and Kerchunk can avoid downloading an entire dataset before analysis, but their architectures differ.

| Aspect | OPeNDAP | Kerchunk |
| --- | --- | --- |
| Main mechanism | Remote data-access protocol | Reference-based access to source bytes |
| Example endpoint | `/dodsC/` | Direct file access such as `/fileServer/` |
| Dataset interpretation | Primarily performed by the data service | Encoded in a reference mapping and interpreted by the client-side stack |
| Subsetting | Server processes the request and returns the requested subset | Client uses references to locate required regions of the source files |
| Original data copied? | No | No |
| Scaling model | Depends strongly on the server and service infrastructure | Can exploit chunk-aware and parallel reads from suitable storage |

With OPeNDAP, the server provides the data-access service. The client requests a subset, and the server interprets the dataset and returns the requested data. With Kerchunk, the reference description tells the client where the required data are located in the original file. The central difference is therefore **where the information needed to locate and access the subset resides**.


## Choosing an approach

There is no single storage format or access mechanism that is best for every workflow.

**NetCDF with OPeNDAP** is useful when datasets are already published through an established service such as THREDDS and researchers need remote subsetting without changing the underlying archive.

**Zarr** is particularly useful when the data provider controls the storage layout and wants to publish large multidimensional datasets for object-storage, selective-access, or distributed-computing workflows.

**Kerchunk** provides a bridge when large NetCDF or HDF5 archives already exist and rewriting them into Zarr would be undesirable. A comparatively small reference layer can expose those files through a Zarr-compatible access model.

Cloud-oriented workflows therefore do not necessarily require abandoning existing scientific formats. Different approaches can support different infrastructures and access patterns.


:::::::::::::::::::::::::::: keypoints

- Cloud-native data layouts address the problem of efficiently accessing small parts of very large remote datasets.
- Object storage manages independently addressable objects rather than providing the same access model as a traditional filesystem.
- A conventional NetCDF file can be stored in the cloud without becoming a cloud-native data layout.
- Zarr uses a chunk-oriented representation that maps naturally onto selective and parallel access in object storage.
- Cloud-native layouts affect structural and technical interoperability; semantic interoperability still depends on conventions such as CF.
- Kerchunk provides a Zarr-compatible reference view of existing NetCDF or HDF5 data without duplicating the underlying scientific data.
- OPeNDAP, Zarr, and Kerchunk provide different approaches to efficient remote scientific-data access.

::::::::::::::::::::::::::::::::::::::::::::
