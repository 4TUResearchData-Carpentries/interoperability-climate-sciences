---
title: "Semantic interoperability"
teaching: 5 # FIXME teaching time in minutes
exercises: 15 # FIXME exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- Why are metadata standards important for interoperability?
- What are the CF Conventions and how do they contribute to semantic interoperability?
- How can you evaluate and improve the CF compliance of a NetCDF file?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

By the end of this episode, learners will be able to: 

- Explain the role of metadata standards in achieving semantic interoperability.
- Describe the CF Conventions and their key components.
- Evaluate and improve the CF compliance of a NetCDF file.

::::::::::::::::::::::::::::::::::::::::::::::::

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

## Metadata & Semantic Standards 

Content
Headline: The data must be self-describing, standardized, and semantically consistent.
Participants learn:
    • How to check CF compliance
    • Convert metadata
    • Identify interoperability “holes”
✔ This reinforces the semantic layer.

Hands-on Exercise
    • Validate CF compliance
    • Fix a dataset missing standard_name or units
    • Add coordinate metadata where missing


:::::::::: keypoints

- Metadata standards are crucial for semantic interoperability as they provide shared meaning across datasets through standardized vocabularies, units, and metadata conventions.
- The CF Conventions are a widely used climate and forecast metadata standard that defines variable names, units, coordinate systems, and grid information to facilitate data integration and reuse.
- Evaluating and improving CF compliance of NetCDF files ensures that datasets adhere to established standards, enhancing their interoperability and usability in climate science workflows.

::::::::::