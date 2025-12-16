---
title: "Structural interoperability"
teaching: 35  # teaching time in minutes
exercises: 10  # exercise time in minutes
---


:::::::::::::::::::::::::::::::::::::: questions

* What makes a data format *structurally interoperable*?
* Why do community-defined formats matter for climate and atmospheric science?
* How do NetCDF, Zarr, and Parquet enforce predictable structure for machine-actionable workflows?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

* Understand structural interoperability and how it differs from semantic and technical interoperability.
* Describe how community data formats encode expectations about arrays, dimensions, metadata, and storage.
* Recognize which formats are suitable for multidimensional geoscience data and why.
* Explain why NetCDF + CF form the backbone of structural and semantic interoperability in climate science.

::::::::::::::::::::::::::::::::::::::::::::::::

## What is structural interoperability?

Structural interoperability refers to **how data are organized internally**, their dimensions, attributes, and encoding rules.
It ensures that different tools can parse and manipulate a dataset **without prior knowledge of a custom schema** or ad-hoc documentation. 

In particular for the field of climate & atmospheric sciences, for a dataset to be structurally interoperable:

* Arrays must have **known shapes and consistent dimension names** (e.g., time, lat, lon, height).
* Metadata must follow **predictable rules** (e.g., attributes like units, missing_value, long_name).
* Coordinates must be **clearly defined**, enabling slicing, reprojection, or aggregation.

Tools such as `xarray`, `Panoply`, and `netCDF4` libraries rely on these predictable rules to enable cross-platform analysis.

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


::::::::::::::::::::::::::: challenge

### Structural expectations encoded in open standards (Think-Pair-Discuss)

Which structural expectation are required for:
- automated alignment of datasets along common dimensions (e.g., time, latitude, longitude)
- georeferencing and spatial indexing
- correct interpretation of units, scaling factors, and missing values
- efficient analysis of large datasets using chunking and compression


:::::::::solution

### Solution


### 1. Automated alignment of datasets along common dimensions

*(e.g., time, latitude, longitude)*

**Required structural expectation:**

* **Named, ordered dimensions**

**Explanation:**
Automated alignment requires that datasets explicitly declare:

* Dimension names (e.g., `time`, `lat`, `lon`)
* Dimension order and length

Open standards such as NetCDF and Zarr enforce explicit dimension definitions. Tools like xarray rely on this structure to automatically align arrays across datasets without manual intervention.


### 2. Georeferencing and spatial indexing

**Required structural expectations:**

* **Coordinate variables**
* **Explicit relationships between coordinates and data variables**

**Explanation:**
Georeferencing depends on the structural presence of coordinate variables that:

* Map array indices to real-world locations
* Are explicitly linked to data variables via dimensions

This enables spatial indexing, masking, reprojection, and subsetting. Without declared coordinates, spatial operations require external knowledge and break structural interoperability.


### 3. Correct interpretation of units, scaling factors, and missing values

**Required structural expectation:**

* **Consistent attribute schema**

**Explanation:**
Open standards require metadata attributes to be:

* Explicitly declared
* Attached to variables
* Machine-readable (e.g., `units`, `scale_factor`, `add_offset`, `_FillValue`)

This allows tools to correctly interpret numerical values without relying on external documentation. While the *meaning* of units is semantic, their presence and location are structural requirements.


### 4. Efficient analysis of large datasets using chunking and compression

**Required structural expectation:**

* **Chunking and compression mechanisms defined by the standard**

**Explanation:**
Formats such as NetCDF4 and Zarr define:

* How data is divided into chunks
* How chunks are compressed
* How chunks can be accessed independently

This structural organization enables parallel, lazy, and out-of-core computation, which is essential for large-scale climate and atmospheric datasets.

::::::::::::::::::instructor

### Instructor tip (optional)

If you want to deepen discussion, ask participants:

* Which of these expectations would still work if metadata were incomplete?
* Which expectations primarily benefit machines rather than humans?

This naturally leads into the next section on **NetCDF structure** and the boundary between structural and semantic interoperability.
:::::::::::::::::::::::::::::
         
:::::::::::::::::

:::::::::::::::::::::::::::::::::::::::


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


### NetCDF (classic and NetCDF4)

NetCDF provides a robust, self-describing structure well-suited for multidimensional climate data.

**1. Self-describing structure (Structural layer):**

* Dimensions define axes (time, lat, lon, level).
* Variables store multidimensional arrays.
* Coordinate variables describe spatial/temporal context.
* Attributes encode units, fill values, variable names, etc.
* No external schema is required—the file contains its own structural metadata.

This makes NetCDF an exemplar of **structural interoperability**.

**2. Semantic layer via CF conventions:**

While CF is not part of NetCDF per se, it builds semantic meaning on top of the structural layer by specifying:

* Standard names with defined physical meaning.
* Units that follow UDUNITS conventions.
* Grid mappings and projections.
* Relationships among coordinates (e.g., bounds, vertical coordinate types).

NetCDF without CF is structurally interoperable, but **not fully semantically interoperable**.

**3. Community adoption ensures ecosystem interoperability:**

* Used in climate models, reanalysis, satellite retrievals, and oceanographic observations.
* Supported across programming languages and scientific workflows.
* Forms the backbone of CMIP, CORDEX, ERA5, and many national meteorological archives.

NetCDF + CF combination represents the **de facto standard for interoperable multidimensional geoscience data**.

---

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

---

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



## 4. Why community formats matter for interoperability

### Predictable structure

Tools (xarray, netcdf4-python, cf-checker) know exactly how to interpret data.

### Shared vocabulary

Dimensions and variables follow conventions (e.g., time, latitude, longitude).

### Long-term sustainability

Formats endure because communities maintain them, ensuring stability and backward compatibility.

### Machine-actionability

Machines can inspect the structure, parse metadata, and perform transformations autonomously.

### Foundation for reproducible workflows

Consistent structure enables automated pipelines, FAIR-compliant processing, and cross-dataset integration.



## Exercise (10 minutes): Identify Structural Interoperability Strengths and Weaknesses

### Prompt

You receive three datasets describing surface temperature:

1. A CSV file containing columns named `t`, `x`, `y` with no units.
2. A NetCDF file containing `tas(time, lat, lon)` with CF-compliant metadata.
3. A Zarr store containing chunked arrays but missing coordinate metadata.

**Questions (Think–Pair–Share or Multiple Choice):**

* Which dataset is structurally interoperable?
* Which dataset is semantically interoperable?
* Which dataset would require additional metadata to be reusable in an analysis pipeline?
* How does each format support or hinder cross-dataset alignment?

### Expected outcomes

* CSV lacks structural constraints → low structural interoperability.
* NetCDF+CF provides both structural and semantic interoperability → most reusable.
* Zarr provides structural array organization but without metadata loses interoperability.




:::::::::: keypoints

- Community formats are data formats widely adopted and maintained by scientific communities to ensure interoperability, consistency, and usability of datasets.
- Key community formats in climate science include NetCDF for multidimensional data, Zarr for cloud-native storage, and Parquet for tabular data.
- Using community formats facilitates data sharing, integration, and long-term preservation by providing predictable structures and shared vocabularies.
- Structural interoperability is enforced by data models
- NetCDF is a self-describing, multidimensional data format widely used in climate science that enables structural interoperability through its predictable array structures and metadata conventions.
- The CF Conventions provide a semantic layer on top of NetCDF, standardizing variable names, units, and coordinate metadata to achieve semantic interoperability across diverse datasets.
- Widespread community adoption of NetCDF and CF, along with support from major tools and platforms, makes them foundational for interoperability in climate science data.

::::::::::::::::::::



