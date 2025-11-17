---
title: "Cloud-Native LayoutsI"
teaching: 20  # teaching time in minutes
exercises: 25 # exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are cloud-native data layouts?
- Why cloud-native layouts are important for interoperability in climate science data?
- What are key technologies for cloud-native data layouts?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the concept of cloud-native data layouts.
- Recognize the importance of cloud-native layouts for interoperability in climate science data.
- Identify key technologies used in cloud-native data layouts.

::::::::::::::::::::::::::::::::::::::::::::::::

Content
✔ What is a cloud-native layout?
    • Chunked, object-based storage
    • Parallel IO
    • Lazy loading
    • Works natively with Dask, Ray, Spark, Pangeo
✔ Why cloud-native matters for climate datasets
    • ERA5, CMIP6, GOES are TB/PB scale
    • Repeated slicing for ML
    • Cloud-native = scalable structural interoperability
✔ Key technologies
    • Zarr
    • Kerchunk
    • Parquet
    • Example projects: ERA5-to-Zarr, GOES-16 COG/Zarr
✔ NetCDF vs cloud-native
    • NetCDF = interoperable but not cloud-native
    • Zarr = cloud-native but depends on semantic standards (CF)

Hands-on Exercise: Convert NetCDF → Virtual Zarr
Using Kerchunk:
NetCDF → Zarr reference → open with xarray → inspect CF metadata

Reinforces structural interoperability.


:::::::::: keypoints

- Cloud-native data layouts, such as Zarr and Parquet, are designed for efficient storage and access in cloud environments, enabling scalable and parallel processing of large climate datasets.
- Cloud-native layouts enhance interoperability by allowing seamless integration with distributed computing frameworks like Dask, Ray, and Spark, facilitating efficient data slicing and analysis.
- Key technologies for cloud-native data layouts include Zarr for chunked storage, Kerchunk for virtual datasets, and Parquet for tabular data, all of which support scalable and interoperable workflows in climate science.

::::::::::::::::::::
