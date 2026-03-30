---
site: sandpaper::sandpaper_site
---

This lesson is about Interoperability in Climate and Atmospheric Sciences. The value of scientific data depends *not only* on its scientific content but on how easily it can be found, accessed, integrated, and reused by others, whether they are human researchers or automated computational workflows.

This course focuses on how to create *first-class* research outputs using the NetCDF format and publishing them through the 4TU.ResearchData repository. By following community best practices, these datasets can:

- be easily found through rich, machine-actionable metadata,

- be reliably accessed using open standards and stable identifiers,

- be seamlessly integrated with other datasets and

- be confidently reused .

Throughout this course, you will learn how to produce NetCDF datasets that meet these standards, datasets that are not only scientifically valuable today, but that remain accessible, interoperable, and reusable for years to come.
   

## Target audience

This lesson is intended for researchers in the climate and atmospheric sciences who handle multidimensional NetCDF datasets and intend to make their data and software more reusable by others.

## Leo’s challenge: combining climate data (use case)


Leo is studying extreme heatwaves in Europe. He wants to compare his climate model results with satellite observations, urban sensor data, and aircraft measurements.

He starts searching across platforms like Copernicus Climate Data Store, NASA EarthData, and 4TU.ResearchData. At first, everything seems available. But once he begins working with the data, problems appear: Data is spread across different repositories with different access methods, files come in many formats (NetCDF, CSV, GeoTIFF, Excel), and variable names, units, and metadata are inconsistent or unclear. Instead of focusing on heatwaves, Leo spends days just trying to understand and prepare the data. Leo’s problem is not a lack of data or tools. It is a lack of interoperability.

- Data was not created using shared standards
- Metadata is not machine-readable or consistent
- Datasets are difficult to combine across sources

If datasets followed community practices , Leo could:

- Find data faster
- Access it programmatically
- Combine datasets without manual cleanup
- Focus on science instead of data wrangling

This is why interoperability matters: it turns data into something that can be easily reused, combined, and trusted. This lesson helps researchers in climate and atmospheric sciences recognize and apply this essential aspect of modern research.



## Learning objectives 

By the end of this lesson, learners will be able to: 

- Analyze climate and atmospheric datasets to distinguish interoperable from non-interoperable systems across structural, semantic, and technical layers.

- Analyze a NetCDF dataset to evaluate how its data model, dimensions, variables, and metadata organization enable structural interoperability.

- Evaluate the semantic interoperability of a NetCDF dataset using CF Conventions and explain how shared vocabularies enable machine-actionable meaning.

- Apply OPeNDAP to access and subset remote NetCDF datasets, distinguishing between metadata retrieval and data transfer in distributed infrastructures.

- Apply and analyze REST API principles to programmatically create and manage repository metadata, explaining how APIs operationalize technical interoperability.

- Analyze how cloud-native data layouts (NetCDF vs Zarr) affect performance, scalability, and structural interoperability in distributed environments.

- Evaluate a research data infrastructure against AI-readiness requirements by linking structural, semantic, and technical interoperability components to scalable machine learning workflows. 

:::::: prereq

To follow this lesson, learners should already be able to have :

- Working knowledge in Python (write and execute short scripts in Python)
- Awareness of NetCDF format

:::::::::::::
