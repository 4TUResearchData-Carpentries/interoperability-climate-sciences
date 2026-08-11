---
site: sandpaper::sandpaper_site
---

This lesson is about Interoperability in Climate and Atmospheric Sciences. The value of scientific data depends *not only* on its scientific content but on how easily it can be found, accessed, integrated, and reused by others, whether they are human researchers or automated computational workflows.

This course focuses on how to create *first-class* research outputs using the [NetCDF](https://www.unidata.ucar.edu/software/netcdf) format and publishing them through the [4TU.ResearchData](https://data.4tu.nl/) repository. *First class* datasets are

- easily found through rich, machine-actionable metadata,

- reliably accessed using open standards and stable identifiers,

- seamlessly integrated with other datasets and

- semantically understood by humans and machines.

The main message of this lesson is that datasets do not interoperate by themselves; systems interoperate through data that are structured, documented, standardized, and semantically well described. A technically available dataset may still be hard to reuse if its formats, metadata, identifiers, units, vocabularies, and schema are unclear or idiosyncratic. When these elements follow shared standards, the dataset becomes interoperable in the FAIR sense: it can be interpreted and reused across tools, repositories, notebooks, dashboards, cloud workflows, and AI pipelines with far less manual repair.
   

## Target audience

This lesson is intended for researchers in the climate and atmospheric sciences who handle multidimensional NetCDF datasets and intend to make their data and software more reusable by others.


## Ash’s challenge: combining climate data for rainfall and drizzle research

Ash is studying the spatial and temporal distribution of rainfall and drizzle in Europe. She wants to compare climate model output with satellite observations, urban sensor measurements, radar or aircraft observations, national meteorological datasets, and datasets deposited in research repositories.

At first, the data ecosystem looks rich. She can search across platforms such as [Copernicus Climate Data Store](https://cds.climate.copernicus.eu/), [NASA EarthData](https://www.earthdata.nasa.gov/), the [KNMI Data Platform](https://dataplatform.knmi.nl/), and [4TU.ResearchData](https://data.4tu.nl/). Many datasets are open, downloadable, and described online. Some platforms provide climate model output, others provide satellite products, national weather observations, radar composites, or research datasets deposited by individual research groups.

At 4TU.ResearchData, Ash finds a dataset from the **IRCTR Drizzle Radar (IDRA)**. IDRA is a high-resolution, polarimetric X-band radar developed by TU Delft and located at the Cabauw experimental site in the Netherlands. It is designed to observe low-reflectivity precipitation such as drizzle and light rain within a local observation radius. This makes it highly relevant for Ash’s research question, because drizzle is often difficult to capture consistently across different observation systems.


::::::::::::::::::::::::::::::::: callout 

The [real time measurements by IDRA](http://ftp.tudelft.nl/TUDelft/irctr-rse/idra/index.html) are available online.

::::::::::::::::::

The problem is not simply finding data. The problem is making different datasets work together.

For rainfall and drizzle research, Ash may encounter precipitation data in many different forms. Some files are [NetCDF](https://www.unidata.ucar.edu/software/netcdf/), [CSV](https://www.rfc-editor.org/rfc/rfc4180), [GeoTIFF](https://www.ogc.org/standard/geotiff/), [Excel](https://support.microsoft.com/en-us/excel/file-formats-that-are-supported-in-excel), [HDF5](https://www.hdfgroup.org/solutions/hdf5/), [GRIB](https://community.wmo.int/en/activity-areas/wis/grib-edition-2), or [Zarr](https://zarr-specs.readthedocs.io/en/latest/specs.html). Some datasets can be accessed through [APIs](https://www.ibm.com/think/topics/api), [OPeNDAP](https://www.opendap.org/), [THREDDS](https://www.unidata.ucar.edu/software/tds/), [WMS services](https://www.ogc.org/standard/wms/), or [cloud-native object storage](https://guide.cloudnativegeo.org/), while others require manual download from a web interface.


Even when the data is available, it may not be immediately clear how to combine it. One dataset may describe `precipitation_flux`, another may use `rainfall_rate`, `rain_intensity`, `precipitation_amount`, `RR`, `reflectivity`, `equivalent_reflectivity_factor`, or `DBZH`. These names do not always represent the same physical quantity. Some describe rainfall accumulation over a time interval, some describe instantaneous rainfall rate, and others describe radar reflectivity, which is related to precipitation but is not the same as rainfall amount.

Units may also differ or be missing. Rainfall can be expressed in `mm`, `mm h-1`, `kg m-2 s-1`, or accumulated over `5 minutes`, `1 hour`, `1 day`, or a `model time step`. Radar variables may use units such as `dBZ`, while coordinates may be stored inside the file, described in a separate document, exposed through an API response, or not documented clearly at all.

Spatial and temporal alignment adds another challenge. A satellite product may provide gridded observations over Europe. A climate model may provide daily or hourly output on a coarser grid. A national meteorological service may provide radar composites every 5 minutes. IDRA may provide local high-resolution radar measurements around Cabauw. Urban sensors may measure rainfall at specific locations. To compare these sources, Ash needs to understand not only the data values, but also their resolution, coordinate reference system, time coverage, processing level, uncertainty, provenance, and version.

![**Interoperability turns fragmented climate data into connected, reusable research workflows**. *Image created with AI*](episodes/fig/ash_challenge.png)


To combine these datasets reliably, Ash needs to answer a sequence of questions:

1. **Can I find the right datasets?**
   Are they described in APIs(Application Programming Interfaces) in a way that supports search by time, location, variable, version, and data type?

2. **Can I read the data structure?**
   Are the files organized using community formats such as NetCDF, Zarr, GeoTIFF, or Parquet, with explicit dimensions, variables, coordinates, and attributes?

3. **Can I understand what the variables mean?**
   Do the datasets use shared metadata conventions, controlled vocabularies, standard names, units, coordinate systems, and provenance information?

4. **Can I access the data programmatically?**
   Can Ash use APIs, [OPeNDAP](https://www.opendap.org/) , [THREDDS](https://www.unidata.ucar.edu/software/tds), or other standard access mechanisms instead of downloading everything manually?

5. **Can I work with the data at scale?**
   Can she subset remote files, read only the variables and time periods she needs, or use cloud-native layouts such as [Zarr](https://zarr-specs.readthedocs.io/en/latest/specs.html) or [Kerchunk](https://fsspec.github.io/kerchunk/) for repeated analysis?

6. **Can I reproduce and automate the workflow?**
   Are dataset versions, identifiers, metadata, and access routes stable enough for notebooks, dashboards, pipelines, or AI(Artifical Intelligence) workflows?

:::::::::: instructor

This lesson follows Ash’s investigation step by step. Learners first diagnose why “open” or “available” data is not automatically interoperable. Then they inspect datasets through the three layers of interoperability:

* **Structural interoperability:** how data are organized, encoded, and made readable by tools.
* **Semantic interoperability:** how variables, units, coordinates, and scientific meaning are made clear and machine-actionable.
* **Technical interoperability:** how data and metadata can be accessed, exchanged, queried, and reused across systems.



::::::::::::::::::::::::::::::::





## Learning objectives 

By the end of this lesson, we aim to equip the learners with: A practical checklist for designing reusable climate and atmospheric datasets from the beginning: use community formats, apply semantic conventions, expose data through stable access mechanisms, and prepare data layouts that can support scalable analysis.

Specifically , learners will learn how to: 

- Assess climate and atmospheric datasets to identify structural, semantic, and technical interoperability barriers that prevent reliable reuse and combination across sources.

- Analyze a NetCDF dataset to identify how its data model, dimensions, variables, coordinates, and attributes enable structural interoperability.

- Evaluate whether a NetCDF dataset provides machine-actionable scientific meaning by examining its use of conventions, standard names, units, and coordinate metadata.

- Use OPeNDAP with Python to access, inspect, subset, and visualize remote NetCDF data while distinguishing metadata retrieval from actual data transfer.

- Use REST API requests to search, retrieve, create, and update repository metadata, explaining how programmatic access supports technical interoperability and reproducible RDM workflows.

- Compare NetCDF, Zarr, and Kerchunk-based access patterns to determine how cloud-native layouts affect structural interoperability, scalability, and efficient reuse of large climate datasets.

- Evaluate the AI-readiness of a climate data infrastructure by linking structural, semantic, and technical interoperability components to scalable, reproducible, and trustworthy machine-learning workflows.


## References and Glossary

For further reading and definitions of key terms introduced in this workshop, consult the [Reference](learners/reference.md) section. 

:::::: prereq

To follow this lesson, learners should already be able to have :

- Working knowledge in Python (write and execute short scripts in Python)
- Awareness of NetCDF format

:::::::::::::

