---
title: "Introduction"
teaching: 30 # teaching time in minutes
exercises: 10 # exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- Why interoperability is important when dealing with research data? 
- What are the three layers of interoperability?
- How can you identify if a dataset is interoperable or not?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand why interoperability matters in climate & atmospheric science
- Recognize the 3 layers: structural, semantic, technical
- Identify interoperable vs non-interoperable datasets

::::::::::::::::::::::::::::::::::::::::::::::::

## Understand the importance of interoperability for data reuse

Climate and atmospheric research relies on combining many heterogeneous data sources, each produced with different instruments, models, resolutions, file formats, and metadata conventions. This makes interoperability not just helpful—but essential—for meaningful scientific analysis, reproducibility, and large-scale modeling.

::::::::::::::::::::::::::: challenge

### Understand the importance of interoperability for data reuse

Make it a multiple choice (or transform it to think-pair-share to enable more discussions)
You have two datasets about ocean temperature — one in CSV format with unclear column names, and one in NetCDF format following CF conventions.
Which dataset would be easier to reuse and why?

A) CSV — because it’s a simple text file

B) NetCDF — because it follows shared conventions

C) Both are equally reusable


:::::::::solution

### Solution

B) NetCDF — because it follows shared conventions

:::::::::::::::::

:::::::::::::::::::::::::::::::::::::::

### Specific data challenges in the climate & atmospheric sciences

- Heterogeneous data origins
Climate research integrates satellite retrievals, weather models, climate simulations, in-situ sensors, radar, lidar, aircraft measurements, and reanalysis datasets—each with its own structure, conventions, and processing workflows.

- Different spatial and temporal resolutions
Satellite images may be daily or hourly at 1 km resolution, while climate models may provide monthly or daily outputs on coarse grids; combining them requires consistent metadata and alignment.

- Multiple file formats and data models
Data may come as GRIB, NetCDF, GeoTIFF, HDF5, CSV, or Zarr, each with different structural assumptions that affect processing and interpretation.

- Inconsistent metadata quality
Missing units, inconsistent variable names, unclear coordinate systems, or non-standard attributes are frequent issues—making semantic interoperability a major challenge.

- Large data volume and velocity
Earth observation missions (e.g., Sentinel, GOES), reanalysis products (ERA5), and high-resolution climate simulations produce terabytes to petabytes of data, making efficient, interoperable access necessary.

- Different access mechanisms and services
Data are distributed across portals using APIs, OPeNDAP servers, cloud object storage, FTP, THREDDS catalogs, proprietary download tools, or manual interfaces—requiring technical interoperability to automate workflows.

- Versioning and reproducibility issues
Climate datasets evolve frequently (e.g., reprocessed satellite series, new CMIP6 versions), and without stable identifiers or catalog metadata, reproducibility becomes difficult across institutions.

- Need for multi-model and multi-dataset comparisons
Studies such as model evaluation, bias correction, and data assimilation depend on aligning diverse datasets that were never originally designed to work together.

### Why interoperability is essential

Interoperability is essential in climate and atmospheric science because researchers routinely work with multiple heterogeneous datasets that were never originally designed to work together. By ensuring that data are described consistently, stored in predictable structures, and accessed through standard mechanisms, interoperability makes it possible to combine and reuse data efficiently across research workflows.

First, **interoperability enables data reuse**: when datasets follow shared metadata conventions and formats, researchers can easily understand what variables represent, how they were produced, and how they can be used in new contexts. This avoids redundant effort and saves time across research groups.

Second, **interoperability enables integration across sources—for example**, combining model output with satellite observations, radar measurements, in-situ sensors, and reanalysis datasets. These data sources differ in resolution, structure, access method, and semantics; without shared standards, aligning them becomes difficult or impossible.

Third, **interoperability reduces friction in data pipelines**. Standardized formats, consistent metadata, and machine-actionable APIs allow workflows to run smoothly without manual cleaning, renaming, or restructuring. This is especially critical when handling large, frequently updated datasets typical in climate research.

Finally, **interoperability is required for automation, AI, dashboards, and multi-disciplinary science**. Machine learning pipelines, automated monitoring systems, and interactive applications rely on consistent, accessible, and machine-readable data. Without interoperability, these tools break or require extensive custom engineering.

In short, interoperability is what makes the diverse, high-volume data ecosystem of climate and atmospheric science usable, scalable, and scientifically trustworthy.

## Identify the three layers of interoperability

### Semantic interoperability = meaning

Semantic interoperability ensures that data carries shared, consistent meaning across institutions and tools.
This is achieved through:
- standard vocabularies
- controlled terms
- variable naming conventions 
- units
- coordinate definitions

Examples include CF standard names, ACDD attributes, and ESGF controlled vocabularies. Without semantic interoperability, datasets cannot be reliably interpreted, compared, or combined.

### Structural interoperability = representation

Structural interoperability ensures that data is organized, stored, and encoded in predictable, machine-actionable ways. This is achieved through:

- common file formats

- shared data models

consistent dimension and array structures

Examples include NetCDF, Zarr, and Parquet, which define how variables, coordinates, and metadata are stored. Structural interoperability allows tools across programming languages and platforms to read data consistently.

### Technical interoperability = access

Technical interoperability ensures that data can be accessed, exchanged, and queried using standard, machine-readable mechanisms. This is achieved through:

- APIs

- remote access protocols

- web services

- cloud object storage interfaces

Examples include OPeNDAP, THREDDS and REST APIs. Technical interoperability enables automated workflows, cloud computing, and scalable analytics.

## Key elements of interoperable research workflows

Interoperable research workflows rely on a set of shared practices, formats, and technologies that allow data to be exchanged, understood, and reused consistently across tools and institutions. In climate and atmospheric science, these elements form the backbone of scalable, reproducible, and machine-actionable data ecosystems.

    • Community formats (NetCDF, Zarr, Parquet) provide a common structural foundation.

    These formats encode data in predictable ways, with clear rules about dimensions, variables, and internal structure. NetCDF remains the dominant community standard for multidimensional geoscience data, while Zarr offers a cloud-native representation suitable for large-scale, distributed computing. Parquet complements both by providing an efficient columnar format for tabular or metadata-rich data. Using community formats ensures that tools across languages and platforms can interpret datasets consistently.


    • Standardized metadata (CF conventions) provide the semantic layer needed for meaningful interpretation.

    CF conventions define variable names, units, coordinate systems, and grid attributes so that datasets from different sources “speak the same language.” This allows climate model output, satellite observations, and reanalysis products to be aligned and compared reliably.

    • Stable APIs enable technical interoperability by providing machine-readable access to data and metadata.

    APIs based on HTTP and JSON allow automated workflows, programmatic data publication, and integration between repositories, processing systems, and analysis tools. A stable, well-documented API ensures that downstream services and scripts continue to function even as data collections evolve.

    • Catalogs (STAC, Intake-ESM) provide the discovery layer that makes datasets findable and indexable.

    These catalogs describe what datasets exist, where they are stored, how they can be accessed, and what metadata they contain. They enable researchers and automated tools to search across large collections by variable, time, domain, or spatial footprint, making data integration far more efficient.

    • Cloud-native layouts make large datasets scalable and performant.

    By storing data as independent chunks in object storage, formats such as Zarr allow parallel, lazy, and distributed access—ideal for big climate datasets, serverless workflows, and AI pipelines. This ensures that even multi-terabyte archives can be streamed efficiently without requiring full downloads.

Together, these elements work as a coordinated system: community formats provide structure, metadata provides meaning, APIs provide access, catalogs provide discoverability, and cloud-native layouts provide scalability.

This combination is what enables truly interoperable research workflows in modern climate and atmospheric science.




::::::::::::::::::::::::::: challenge

### True/False or Agree/Disagree with discussion afterwards 

- “As long as data are open access, they are interoperable.”
- “Metadata standards help ensure interoperability.”
- “As long as data is using an open standard format is interoperable” (hint to connect to the next section)

:::::::::solution

### Solution

F,T,F

:::::::::::::::::

:::::::::: instructor

This exercise is for discussion in Plenum nad it can serves as a good link to the next section

:::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::: challenge

### Discuss with you peer: 

Participants inspect a small dataset and answer:
    • What is its structure?
    • What metadata does it have?
    • How is it accessed?


:::::::::solution

### Solution

Participants should identify whether the dataset is interoperable based on the three layers discussed (structural, semantic, technical).
:::::::::::::::::

::::::::::::: instructor

for the good data example go here: https://opendap.4tu.nl/thredds/catalog/IDRA/2019/01/02/catalog.html?dataset=IDRA_scan/2019/01/02/IDRA_2019-01-02_quicklook.nc

the bad example is in data/iris_dataset_bad_example.csv

:::::::::::::


:::::::::: keypoints

- Interoperability means that data, tools, and systems can work together automatically and reliably with minimal manual intervention.

- Interoperability operates across multiple layers: structural (how data is represented), semantic (how data is described), and technical (how data is accessed and exchanged).

-  All three layers must function together—if any layer fails (semantic, structural, or technical), data cannot be reused effectively.

- Interoperability is crucial in climate & atmospheric science because research integrates highly heterogeneous data sources such as models, satellite products, observations, and reanalysis datasets.

- Key elements that support interoperability in research workflows include community formats, standardized metadata, stable APIs, catalogs, and cloud-native layouts.


::::::::::::::::::::
