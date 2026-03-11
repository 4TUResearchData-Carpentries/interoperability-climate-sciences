---
title: "Structural interoperability"
teaching: 35  # teaching time in minutes
exercises: 10  # exercise time in minutes
---


:::::::::::::::::::::::::::::::::::::: questions

* What is structural interoperability?

* How do open standards and community governance enable structurally interoperable research data?

* Which structural expectations must a data format satisfy to support automated, machine-actionable workflows?

* Which open standards are commonly used in climate and atmospheric sciences to achieve structural interoperability?

* What is NetCDF's data structure?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

* Explain structural interoperability in terms of data models, dimensions, variables, and metadata organization.

* Identify the role of open standards and community-driven governance in ensuring long-term structural interoperability.

* Describe the key structural expectations required for automated alignment, georeferencing, metadata interpretation, and scalable analysis.

* Analyze a NetCDF file to identify its core structural elements (dimensions, variables, coordinates, and attributes).
::::::::::::::::::::::::::::::::::::::::::::::::

## What is structural interoperability?

Structural interoperability refers to **how data are organized internally**, their dimensions, attributes, and encoding rules.
It ensures that different tools can parse and manipulate a dataset **without prior knowledge of a custom schema** or ad-hoc documentation. 

In particular for the field of climate & atmospheric sciences, for a dataset to be structurally interoperable:

* Arrays must have **known shapes and consistent dimension names** (e.g., time, lat, lon, height).
* Metadata must follow **predictable rules** (e.g., attributes like units, missing_value, long_name).
* Coordinates must be **clearly defined**, enabling slicing, reprojection, or aggregation.

Structural interoperability answers the question: **Can machines understand how this dataset is structured without human intervention?**



## Structural interoperability relies on open standards

To attain structural interoperability, two key aspects are essential: machine-actionability and longevity. Open standards ensure that these two aspects are met. An open standard is not merely a published file format. From the perspective of structural interoperability, an open standard guarantees that:

- The data model is publicly specified (arrays, dimensions, attributes, relationships)
- The rules for interpretation are explicit, not inferred from software behavior
- The specification can be independently implemented by multiple tools

- The standard evolves through transparent versioning, avoiding silent breaking changes

These properties ensure that a dataset remains structurally interpretable even when:

- The original software is no longer available
- The dataset is reused in a different scientific domain
- Automated agents, rather than humans, perform the analysis


Usually these standards are adopted and maintained by a non-profit organization and its ongoing development is driven by a community of users and developers on the basis of an open-decision making process

Formats such as NetCDF, Zarr, and Parquet emerge from broad communities like:

* Unidata (NetCDF)
* Pangeo (cloud-native geoscience workflows)
* Open Geospatial Consortium (OGC) & World Meteorological Organization (WMO) 

These groups define formats that encode **stable, widely adopted structural constraints** that machines and humans can rely on.

### Structural Interoperability is about Data Models, not just data formats

Structural interoperability does not emerge from file extensions alone. It is enforced by an underlying data model that defines:

- What kinds of objects exist (arrays, variables, coordinates)

- How those objects relate to one another

- Which relationships are mandatory, optional, or forbidden

- Community standards such as NetCDF and Zarr succeed because they define and constrain a formal data model, rather than allowing arbitrary structure.

This is why structurally interoperable datasets can be:

- Programmatically inspected

- Automatically validated

- Reliably transformed across tools and platforms

::::::::::::::::::::::::::: challenge

### Structural expectations encoded in open standards (Think-Pair-Discuss)

Which structural expectation are required for:

- automated alignment of datasets along common dimensions (e.g., time, latitude, longitude)
- georeferencing and spatial indexing
- correct interpretation of units, scaling factors, and missing values
- efficient analysis of large datasets using chunking and compression


:::::::::solution

### Solution


**Automated alignment of datasets along common dimensions** , *(e.g., time, latitude, longitude)*

- Required structural expectation: *Named, ordered dimensions*

**Explanation:**
Automated alignment requires that datasets explicitly declare:

* Dimension names (e.g., `time`, `lat`, `lon`)
* Dimension order and length

Open standards such as NetCDF and Zarr enforce explicit dimension definitions. Tools like xarray rely on this structure to automatically align arrays across datasets without manual intervention.


**Georeferencing and spatial indexing**

- Required structural expectations: *Coordinate variables*, *Explicit relationships between coordinates and data variables*

**Explanation:**
Georeferencing depends on the structural presence of coordinate variables that:

* Map array indices to real-world locations
* Are explicitly linked to data variables via dimensions

This enables spatial indexing, masking, reprojection, and subsetting. Without declared coordinates, spatial operations require external knowledge and break structural interoperability.


**Correct interpretation of units, scaling factors, and missing values**

- Required structural expectation: *Consistent attribute schema*

**Explanation:**
Open standards require metadata attributes to be:

* Explicitly declared
* Attached to variables
* Machine-readable (e.g., `units`, `scale_factor`, `add_offset`, `_FillValue`)

This allows tools to correctly interpret numerical values without relying on external documentation. While the *meaning* of units is semantic, their presence and location are structural requirements.


**Efficient analysis of large datasets using chunking and compression**

- Required structural expectation: *Chunking and compression mechanisms defined by the standard*

**Explanation:**
Formats such as NetCDF4 and Zarr define:

* How data is divided into chunks
* How chunks are compressed
* How chunks can be accessed independently

This structural organization enables parallel, lazy, and out-of-core computation, which is essential for large-scale climate and atmospheric datasets.

::::::::::::::::::instructor

### To link it to semantic interoperability for next section

If you want to deepen discussion, ask participants:

* Which of these expectations would still work if metadata were incomplete?
* Which expectations primarily benefit machines rather than humans?

This naturally leads into the next section on **NetCDF structure** and the boundary between structural and semantic interoperability.
:::::::::::::::::::::::::::::
         
:::::::::::::::::

:::::::::::::::::::::::::::::::::::::::




## NetCDF 

 Once upon a time, in the 1980s at University Corporation for Atmospheric Research (UCAR) and Unidata, a group of researchers came together to address the need for a common format for atmospheric science data. Back then the group of researchers committed to this task asked the following question: how to effectively share my data? such as it is portable and can be read easily by humans and machines? the answer was NetCDF. 

 NetCDF (Network Common Data Form) is a set of software libraries and self-describing, machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data. 

![A NetCDF file consists of dimensions, variables, and attributes](fig/fig_1_netcdf.png)


**Self-describing structure (Structural layer):**

* One file carries data and metadata, usable on any system. This means that there is a header which describes the layout of the rest of the file, in particular the data arrays, as well as arbitrary file metadata in the form of name/value attributes. 
* Dimensions define axes (time, lat, lon, level).
* Variables store multidimensional arrays.
* Coordinate variables describe spatial/temporal context.
* Attributes are metadata per variable, and for the whole file. 
* Variable attributes encode units, fill values, variable names, etc.
* Global attributes provide dataset-level metadata (title,creator, convention,year).
* No external schema is required, the file contains its own structural metadata.


## Other useful open standards used in the field

### Zarr (cloud-native, chunked, distributed)

Zarr is a format designed for scalable cloud workflows and interactive high-performance analytics.

Structural characteristics:

* Stores arrays in chunked, compressed pieces across object storage systems.
* Supports hierarchical metadata similar to NetCDF.
* Uses JSON metadata that tools interpret consistently (e.g., xarray → Mapper).
* Works seamlessly with Dask for parallelized computation.

Why it matters:

* Enables analysis of petabyte-scale datasets (e.g., NASA Earthdata Cloud).
* Structural rules are community-governed (Zarr v3 specification).
* Allows fine-grained access—no need to download entire files.

Zarr is becoming central for cloud-native structural interoperability.


### Parquet (columnar, schema-driven)

Parquet offers efficient tabular storage and is ideal for:

* Station data
* Metadata catalogs
* Observational time series

Structural characteristics:

* Strongly typed schema ensures column-level consistency.
* Supports complex types but enforces stable structure.
* Columnar layout enables fast filtering and analytical queries.

In climate workflows:

* Parquet is used for catalog metadata in Intake, STAC, and Pangeo Forge.
* Structural predictability supports machine-discoverable datasets.

Parquet complements NetCDF/Zarr, addressing non-array use cases.




:::::::::::::::::::: challenge

## Identify the structural elements in a NetCDF file 

Please perform the following steps to explore the structural elements of a NetCDF file:

    1. Open a NetCDF file : https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/01/02/IDRA_2019-01-02_quicklook.nc.html
    2. Identify variable metadata
    3. Identify the global attributes
    4. Identify the dimensions and coordinate variables

:::::::: solution

### 1. Open the NetCDF file

The dataset can be explored through the OPeNDAP interface:

https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/01/02/IDRA_2019-01-02_quicklook.nc.html

The dataset structure shows several variables and one main dimension.


### 2. Variable metadata

The dataset contains the following variables:

**time (Float64)**  
- Dimension: `time = 1440`  
- `standard_name: time`  
- `units: hours since 2019-01-02 00:00:00`  
- `axis: T`  

This variable acts as the **time coordinate**.

**quicklook (Int16)**  
- Dimension: `time`  
- `long_name: quicklook`  
- `comment: Information about available IDRA data in the data acquisition settings.`  

This is the **main data variable** in the dataset.

**iso_dataset (String)**  
Contains descriptive metadata about the dataset, such as title, abstract, keywords, spatial and temporal coverage.

**product (String)**  
Contains metadata about the product generation process, including:
- start and end date of data
- format version
- reference documentation
- originator

**station_details (String)**  
Contains metadata about the measurement location, including:
- station name
- latitude and longitude
- elevation
- WMO station identifier



### 3. Global attributes

Examples of dataset-level attributes include:

- `title: IDRA Quicklook`
- `institution: Delft University of Technology`
- `history: Quicklook data accompanying datasets of processed and raw IDRA data`
- `references: Design of a High Resolution X-band Doppler Polarimetric Radar`
- `Conventions: CF-1.4`
- `location: CESAR observatory, the Netherlands`
- `source: Ground-based polarimetric weather radar`

These attributes describe the **dataset as a whole**.



### 4. Dimensions and coordinate variables

**Dimension**

- `time = 1440`

This indicates that the dataset contains **1440 time steps**.

**Coordinate variable**

- `time(time)`

The variable `time` defines the coordinate values for the time dimension.

**Data variable**

- `quicklook(time)`

This variable stores the actual measurement values along the time dimension.


### Summary of structural elements

| Element | Example |
|-------|-------|
| Dimension | `time = 1440` |
| Coordinate variable | `time(time)` |
| Data variable | `quicklook(time)` |
| Global attributes | `title`, `institution`, `source`, `Conventions` |
| Metadata variables | `iso_dataset`, `product`, `station_details` |

These structural elements allow software tools to understand **how the dataset is organized**, which is essential for **structural interoperability**.

::::::::::::::


::::::::::::::::::::::


:::::::::: keypoints

- Structural interoperability concerns how data are organized, not what they mean. 
- Open standards are essential for machine-actionability and long-term reuse.
- Structural interoperability is enforced by data models, not file extensions.
- Structural interoperability is enforced by data models.
- Standards maintained by communities (e.g. Unidata, Pangeo, OGC/WMO) encode shared structural contracts that tools and workflows can reliably depend on.
- NetCDF exemplifies structural interoperability for multidimensional geoscience data.

::::::::::::::::::::



