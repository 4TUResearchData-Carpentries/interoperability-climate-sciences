---
site: sandpaper::sandpaper_site
---

This lesson introduces interoperability in climate and atmospheric sciences as the ability to make research data usable across different tools, systems, and workflows with minimal dataset-specific intervention. 

Climate and atmospheric research routinely combines heterogeneous and often large datasets produced by models, satellites, radar systems, sensors, and research infrastructures. Making these data available is therefore not enough: researchers and software must also be able to determine how the data are organised, understand what they mean, and access them through predictable technical mechanisms.

The course approaches this challenge through three complementary layers of interoperability: structural interoperability, which concerns how data are organised and represented; semantic interoperability, which concerns how scientific meaning is expressed and shared; and technical interoperability, which concerns how independent systems access and exchange data and metadata.

Using climate and atmospheric data as the practical context, learners work with community formats and conventions such as NetCDF and the CF Conventions, inspect and subset remote datasets through DAP/OPeNDAP, interact with repository metadata through Web APIs, and explore how Zarr and Kerchunk support selective and scalable access to large multidimensional datasets. 

Together, these episodes show how interoperability depends on coordinated choices about data structure, scientific meaning, access mechanisms, storage layouts, and reproducibility rather than on any single format or technology.

## Learning objectives 

- Assess a climate or atmospheric dataset in terms of structural, semantic, and technical interoperability and identify barriers to its reuse.

- Analyse how the structure of a scientific dataset, particularly the NetCDF data model, enables software to identify and process its dimensions, variables, coordinates, attributes, and relationships.

- Evaluate whether scientific variables are described with sufficient shared, machine-actionable meaning for reliable interpretation and comparison, using semantic resources.

- Use DAP/OPeNDAP with Python to inspect and subset remote NetCDF data while distinguishing remote metadata access from data transfer.

- Use a Web API to programmatically query and retrieve research data and metadata and explain how APIs support machine-to-machine interoperability.

- Compare conventional NetCDF, Zarr, and Kerchunk-based access models to explain how cloud-native layouts support selective and scalable access to large multidimensional datasets.

- Explain AI readiness as a task-dependent property of a data workflow by connecting structural, semantic, and technical interoperability with scalable access and reproducibility requirements.
   


## Target audience

This lesson is intended for researchers in the climate and atmospheric sciences who handle multidimensional NetCDF datasets and intend to make their data and software more reusable by others. It is also intended for support staff that need capacity in those topics.


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






## References and Glossary

For further reading and definitions of key terms introduced in this workshop, consult the [Reference](learners/reference.md) section. 

:::::: prereq

To follow this lesson, learners should already be able to have :

- Working knowledge in Python (write and execute short scripts in Python)
- Awareness of NetCDF format

:::::::::::::

