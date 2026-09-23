---
title: "Technical interoperability: Data access protocols"
teaching: 30
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is technical interoperability?

- What is the difference between storing data remotely and providing remote data access?

- What is the DAP (Data Access Protocol)?

- How does OPeNDAP enable remote access without full download?

- What happens when we open a remote NetCDF file using `xarray.open_dataset()`?

- Why are remote data-access protocols important for large-scale scientific workflows?

:::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

By the end of this episode, learners will be able to:

- Define technical interoperability in the context of scientific data infrastructures.

- Explain how DAP enables interoperable machine-to-machine data access.

- Access a remote NetCDF dataset via OPeNDAP using Python.

- Perform server-side subsetting of variables and dimensions.

- Distinguish between metadata access and actual data transfer.

::::::::::::::::::::::::::::::::::::::::::::::::

## What is technical interoperability?

So far, we have considered whether scientific data are structured in a predictable way and whether their scientific meaning can be understood consistently. Technical interoperability introduces another question: **How can one computer system access data held by another system?** A dataset may be perfectly structured and well described, but that does not automatically mean that another system can access it efficiently.

Technical interoperability concerns the mechanisms that allow independent systems to communicate and exchange data through agreed technical interfaces and protocols.

Imagine, for example, that a climate dataset is stored in a research data repository. A researcher may be interested only in one variable, covering a particular period and geographical region. One possible approach is to download the complete file and perform the selection locally. Another possibility is for the remote infrastructure to provide a mechanism through which the researcher can request only the required part of the dataset. This second possibility is particularly important for large scientific datasets and repetitive automated computational workflows because software can request only the variables, time periods, or spatial regions needed for each analysis, avoiding repeated transfer and storage of complete datasets. Technical interoperability therefore concerns not only whether data can be transferred, but also **how systems agree to request, exchange, and access those data across a network**.



## Storage is not the same as access

It is useful to distinguish **where data are stored** from **how data are accessed**. A NetCDF file may physically reside on institutional storage, repository infrastructure, object storage, or another remote storage system. However, knowing where the bytes are stored does not automatically tell us how researchers or software applications can interact with them. Between the storage system and the researcher there is often a **data-access service**. This service receives requests from clients, interacts with the underlying dataset, and returns the requested information. For example, a NetCDF file may be stored by a repository while a data server such as THREDDS or Hyrax makes that dataset accessible over the network. Scientific applications such as Python libraries can then communicate with the data server instead of directly interacting with the storage infrastructure. This service layer is an important component of technical interoperability.

A dataset being stored remotely, including in cloud infrastructure, therefore does not automatically make it technically interoperable. The infrastructure must also provide an agreed mechanism through which independent clients can access the data.



## A protocol defines how data are accessed

A **protocol** is an agreed set of rules describing how systems communicate with each other. HTTP, for example, defines rules for exchanging resources across the Web. Scientific data access often requires more than transferring an entire file. A scientific client may need to discover which variables a dataset contains, inspect its dimensions and metadata, or request only a particular variable, time interval, depth level, or geographical subset. A **scientific data-access protocol** defines how these kinds of requests and responses are expressed between a client and a remote data service. This is different from the internal structure of the file itself. NetCDF defines how arrays, dimensions, variables, and attributes are organised within a dataset. 

A protocol such as DAP defines how a remote client can discover and retrieve those structures across a network. The two therefore address different aspects of interoperability. NetCDF helps software understand **how the dataset is structured**, while DAP provides a standardized mechanism for **accessing that structured dataset remotely**.



### Different protocols, different ways of accessing remote data

Remote data can be made available through different technical mechanisms. The simplest form is ordinary **HTTP** or **HTTPS** file access. In this case, the client requests a file and the server transfers that file to the client. This model works very well for many datasets, particularly when files are relatively small or when the researcher needs the complete dataset. Scientific data infrastructures can also offer more specialized access mechanisms. **DAP** and **OPeNDAP**, for example, allow clients to interact with the internal structure of scientific datasets and request selected variables or subsets instead of necessarily retrieving the complete file. Geospatial communities use other standardized services as well. The Open Geospatial Consortium has developed standards such as the **Web Coverage Service**, which can provide access to spatial and temporal subsets of multidimensional geospatial data. 

These mechanisms do not necessarily compete with one another. The same dataset may be exposed through several access methods. A researcher might download a complete NetCDF file through HTTPS, while another researcher accesses selected variables from the same dataset through OPeNDAP. Data servers such as THREDDS are designed to support this type of architecture, allowing one underlying dataset to be exposed through multiple access services. In this episode, we focus on **DAP and OPeNDAP**, because they were specifically developed to support remote access to structured scientific datasets.



## DAP and OPeNDAP

### From distributed oceanography to remote data access

The origins of OPeNDAP go back to the early 1990s, when oceanographers were facing a problem that is still familiar today: important scientific datasets were distributed across researchers, institutions, and federal data centres, and different systems used different ways of storing and accessing them. The challenge was therefore not simply how to create another scientific file format. Formats such as NetCDF already provided mechanisms for representing structured scientific data. The problem was **how researchers could access those distributed datasets through a common mechanism, regardless of where the data were physically stored**.

A key moment came in **1993**, when a workshop at the University of Rhode Island, involving researchers from URI and MIT and supported by NASA, NOAA, and The Oceanography Society, explored the requirements for what became the **Distributed Oceanographic Data System (DODS)**. One of the remarkably simple ideas that emerged from this work was: **A URL equals a dataset.** and, even more importantly: **A URL with constraints equals a subset.** In other words, a scientific dataset could be treated as a resource accessible over the network, while additional information in the request could specify which part of that dataset should be returned. This principle anticipated the type of remote subsetting that is still central to OPeNDAP today.

By **1995**, the first Distributed Oceanographic Data System had been implemented. DODS combined the emerging World Wide Web with a common data model and data-access protocol so that scientific applications could retrieve distributed data through a standardized interface. The technology was already being presented to the broader Web community in 1995, including at the Fourth International World Wide Web Conference. Although the project started in **oceanography**, its underlying problem was not specific to ocean data. Climate science, atmospheric science, remote sensing, Earth observation, and many other disciplines faced the same challenge of accessing large and distributed scientific datasets. 

The technology therefore evolved from the oceanography-specific **DODS** into the more discipline-neutral **Data Access Protocol (DAP)** and the OPeNDAP software ecosystem. OPeNDAP Inc. was formed as a nonprofit organisation in **2000** to maintain and develop this infrastructure. There is an important terminology distinction between **DAP**, **OPeNDAP**, and the software that actually serves datasets. DAP is the protocol itself. It specifies how clients and servers communicate when accessing structured scientific data. OPeNDAP is the project and technology ecosystem responsible for developing DAP and associated software. A separate piece of software acts as the **data server**. The server implements the protocol and exposes datasets to remote clients. One example is **Hyrax**, the data server developed by OPeNDAP. Hyrax can expose supported scientific datasets through DAP. Another widely used data server in the Earth sciences is the **THREDDS Data Server**, which can also expose NetCDF datasets through OPeNDAP services. For researchers accessing existing datasets, the distinction may initially be invisible. They simply receive an OPeNDAP endpoint and use it from their scientific software.

For data providers and infrastructure operators, however, the distinction is important. Someone must operate the server that interprets DAP requests and connects those requests to the underlying scientific datasets. The main purpose, however, remained essentially the same: **Allow independent scientific clients to access distributed structured data through a common protocol.**

### How DAP works

The **Data Access Protocol (DAP)** defines a common way for clients to inspect the structure and metadata of a remote dataset and request selected data from it. A client can first discover which variables, dimensions, and metadata are available. It can then request only the data required for a particular analysis, such as one variable, a particular time interval, selected depth levels, or a geographical subset. The remote data service interprets the request, interacts with the underlying dataset, and returns the requested information.

This standardized interaction means that the client and server can be developed independently. A Python application, for example, does not need to understand how the provider's storage infrastructure works internally. Likewise, the data provider does not need to develop a special access mechanism for every possible analysis application. Both sides only need to understand the same protocol.

This separation is one of the main contributions of DAP to technical interoperability.

### From DAP2 to DAP4

The protocol has continued to evolve as scientific data and infrastructures have become more complex. **DAP2** became widely adopted and in 2005 was approved as a standard for NASA (National Aeronautics and Space Administration) Earth Science Data Systems. It remains common across existing scientific data infrastructures. In **2006**, OPeNDAP released the first version of **Hyrax**, its data server implementation, allowing providers to expose scientific datasets through DAP services.

Later, **DAP4** was developed to provide a richer data model capable of representing a broader range of modern scientific data structures. DAP4 was released in **2014** following collaboration involving OPeNDAP, Unidata, NOAA (National Oceanic and Atmospheric Administration), and NSF (National Science Foundation)-supported work. 

For this episode, however, the detailed technical differences between DAP2 and DAP4 are less important than the interoperability principle they share: **DAP provides a standardized conversation between a scientific data client and a remote data service.**



## Publishing a file is not the same as providing remote data access

Imagine two repositories containing exactly the same NetCDF file. The first repository allows users to download the file through HTTPS. The second repository provides the same download option but also exposes the dataset through DAP2 and DAP4.

From a structural interoperability perspective, both repositories may contain exactly the same NetCDF dataset. The variables, dimensions, attributes, and file structure have not changed. From a technical interoperability perspective, however, the access possibilities are different.

In the second repository, compatible scientific clients can inspect and retrieve selected parts of the dataset remotely. The repository therefore provides an additional technical access layer on top of the stored file. This distinction is important when selecting infrastructure for large multidimensional datasets.

Researchers should therefore ask not only: **Can this repository store my NetCDF files?** but also: **How will machines be able to access the data after I publish them?**

The file format and the repository access infrastructure solve different problems, and both contribute to the eventual reusability of the dataset.

::::::::::::::::::::::::::::: callout

## What if the repository does not provide OPeNDAP?

A research institution, project, or infrastructure provider can also operate its own OPeNDAP-compatible server. **Hyrax** is an open-source data server developed by OPeNDAP for this purpose. An organisation can configure Hyrax to expose scientific datasets stored on its own infrastructure and make them available to compatible remote clients through DAP. Instead of seeing OPeNDAP only from the user's perspective, participants can understand what happens on the provider side. A NetCDF file may initially exist only on local or institutional storage. Once a service such as Hyrax is configured to expose that file, clients elsewhere on the network can interact with it through a standardized DAP endpoint.

Technical interoperability is created by connecting data to interoperable services and protocols. It is not an intrinsic property of the storage location alone. However, operating Hyrax is not equivalent to depositing data in a trusted research data repository.

A repository typically provides preservation, persistent identifiers, metadata management, citation support, access governance, and long-term stewardship. Hyrax primarily provides a technical data-access service.

In a mature research infrastructure, these components can complement one another. The repository manages and preserves the research object, while a service such as THREDDS or Hyrax provides specialized machine access to its scientific contents.

For the purposes of this course, configuring Hyrax can therefore be treated as an optional provider-side example rather than as something every researcher is expected to operate themselves.

::::::::::::::::::::::::::::::::



## 4TU.ResearchData: A repository that exposes NetCDF data through DAP 

4TU.ResearchData provides a useful example of this architecture. NetCDF datasets deposited in the repository can be connected to a THREDDS infrastructure that exposes different access mechanisms for the same underlying data. A researcher can retrieve the complete file through an HTTP-based service when that is the most appropriate workflow. The same dataset can also be exposed through OPeNDAP using DAP2 or through DAP4, allowing compatible clients to interact with the dataset remotely. The researcher can therefore choose an access mechanism according to the computational task rather than being restricted to one method.



### Hands-on: Accessing remote NetCDF datasets stored in 4TU.ResearchData using the DAP protocol

We now move from concept to practice.

We will use:

- `xarray`

- A remote OPeNDAP endpoint

- A NetCDF dataset hosted on a THREDDS server

- Jupyter Lab 

### Step 1 – Open a remote dataset

- Open Jupyter Lab and choose the appropriate environment of the lesson (see [Setup](../learners/setup.md))

- Launch Jupyter Lab, open a terminal and type:

```bash

jupyter lab

```

- Open a new notebook 

- Check installed libraries

```python

import xarray as xr

```

- Open a dataset

```python

url = "https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/01/02/IDRA_2019-01-02_12-00_raw_data.nc"

ds = xr.open_dataset(url,engine="pydap")

ds

```

::::::::::::::::: instructor

In most cases, a warning is shown. This warning is normal when using pydap with a THREDDS OPeNDAP server. It is not an error and your dataset should still load correctly. The warning simply means that PyDAP could not detect whether the server supports DAP2 or DAP4, so it defaults to DAP2, which is the older protocol.

The OPeNDAP protocol has two main versions:

DAP2 – legacy but widely supported (many THREDDS servers still use it)

DAP4 – newer, more efficient protocol

PyDAP tries to infer the protocol automatically. If it cannot, it falls back to DAP2, which triggers the warning. The server (opendap.4tu.nl) is a THREDDS server, and these typically expose DAP2 endpoints, so this behavior is expected.

- Suppress the warning by changing the URL to start with `dap2://`

```python

url_dap2 = url.replace("https://", "dap2://").replace("http://", "dap2://")

ds_dap2 = xr.open_dataset(url_dap2, engine="pydap")

```

:::::::::::::::::

::::::::::::::::::::::: instructor

You can go back to the exercise of the Episode of structural interoperability : **Identify the structural elements in a NetCDF file**

:::::::::::::::::::::::



Observe:

- The dataset structure loads immediately.

- Dimensions and metadata are visible.

- The file has not been fully downloaded.

What happened?

Only metadata and coordinate information were accessed.

### Step 2 – Select a variable

```python

ds["spectrum_width"] # still no full download, just metadata

```

### Step 3 – Perform server-side subsetting

- Actual data transfer occurs

- Now let's select a variable → "spectrum_width", using positional indexing and we will take a 10×10 subset along two dimensions. 

```python

ds["spectrum_width"].isel(time_processed_data=slice(0,10),range=slice(0,10))

```

- Now let's print the values of this subsetting

```python

ds["spectrum_width"].isel(time_processed_data=slice(0,10),range=slice(0,10)).values # to print values on the screen

```

- Slicing by the names of the dimensions

```python

ds["spectrum_width"].sel(

    time_processed_data=slice("2019-01-02T12:00:00.000000000", "2019-01-02T12:00:02.097152173"),

    range=slice(0, 1000)

)

```

- Using `head`

```python

ds["spectrum_width"].head()

ds["spectrum_width"].head(time_processed_data=10)

ds["spectrum_width"].head(range=2)

ds["spectrum_width"].head(range=2).to_pandas() # tabular view

```

```python

ds["spectrum_width"].isel(time_processed_data=0).values #one radar profile (1D slice)

ds["spectrum_width"].isel(range=1).values # One time series  





```





Now actual data transfer occurs — but only for:

- One variable

- A limited time window

This is server-side subsetting enabled by DAP.

### Step 4: Plotting a profile

```python

import matplotlib.pyplot as plt 

ds["spectrum_width"].isel(time_processed_data=0).plot()

ds["spectrum_width"].head(range=10).plot()

```

You have multiple ways to interact with and retrieve parts of the remote dataset:

`.isel()` → positional slicing (what you used)

`.sel()` → coordinate-aware slicing

`.head()` → quick inspection

`.values` → raw data extraction

`.plot()` → visual interpretation





::::::::::::::::::::::::::::::::::::: challenge

##Technical interoperability — True or False?**

Indicate whether each statement is True or False and justify your answer.

- Opening a remote dataset with xarray.open_dataset() automatically downloads the entire file.

- DAP enables server-side filtering before data transfer.

- Remote data-access protocols replace the need for structural interoperability.

- Using OPeNDAP removes the need for a structured data model.

- Technical interoperability enables automated workflows across infrastructures.



:::::::::::::::::::::::::::::::::::::::::::::: solution

**False**. Only metadata is accessed initially; data is transferred upon explicit selection.

**True**. Subsetting occurs on the server before transmission.

**False**. Technical interoperability depends on structural interoperability.

**False**. DAP still depends on structured representations of variables, dimensions, metadata, and other data elements.

**True**. It enables scalable machine-to-machine access. 

::::::::::::::::::::::::::::::::::::::::::::::::::::::: 



::::::::::::::::::::::::::::::::::::::::::::::::::::::::





## Demo: Can Ash combine two IDRA radar datasets? (Optional)

Ash has found two IDRA radar files exposed through OPeNDAP:

* `IDRA_2009-04-27_06-08_raw_data.nc`

* `IDRA_2019-01-02_12-00_raw_data.nc`

Both files come from the same radar system and both are available remotely through OPeNDAP. At first, this suggests that they should be easy to compare. But before Ash can combine them, she needs to inspect whether they are structurally and semantically compatible.

In this demo, we will compare the two files using Python and `xarray`.


### Setup

```python

import xarray as xr

import pandas as pd

import matplotlib.pyplot as plt

```

We use the OPeNDAP data URLs, not the `.html` inspection pages.

```python

url_2009 = "https://opendap.4tu.nl/thredds/dodsC/IDRA/2009/04/27/IDRA_2009-04-27_06-08_raw_data.nc"

url_2019 = "https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/01/02/IDRA_2019-01-02_12-00_raw_data.nc"

```

- To suppress the warning when reading the file with `pydap`: 

```python

url_2009 = url_2009.replace("https://", "dap2://").replace("http://", "dap2://")

url_2019 = url_2019.replace("https://", "dap2://").replace("http://", "dap2://")

```

### Step 1: Open the datasets remotely

```python

ds_2009 = xr.open_dataset(url_2009,engine="pydap")

ds_2019 = xr.open_dataset(url_2019,engine="pydap")

```

```python

ds_2009

```

```python

ds_2019

```

At this point, Ash has not manually downloaded the full files. She is inspecting the datasets remotely through OPeNDAP.

### Step 2: Inspect dimensions

```python

ds_2009.dims

```

```python

ds_2019.dims

```

Ash checks whether both files organise the data in a similar way. For example, she expects to see dimensions such as:

* `time_raw_data`

* `sample_beat_signal`

* `time_processed_data`

* `range`

This matters because variables can only be compared directly if their dimensions are compatible.

### Step 3: Compare variable names

```python

vars_2009 = set(ds_2009.data_vars)

vars_2019 = set(ds_2019.data_vars)

common_vars = sorted(vars_2009.intersection(vars_2019))

only_2009 = sorted(vars_2009.difference(vars_2019))

only_2019 = sorted(vars_2019.difference(vars_2009))

print("Common variables:")

print(common_vars)

print("nOnly in 2009:")

print(only_2009)

print("nOnly in 2019:")

print(only_2019)

```

This is Ash’s first interoperability check. If the files do not contain the same variables, she cannot simply reuse the same analysis code for both years.

### Step 4: Inspect key radar variables

Ash focuses on a few processed radar observables:

```python

radar_variables = [

    "equivalent_reflectivity_factor",

    "differential_reflectivity",

    "radial_velocity",

    "spectrum_width",

    "differential_phase",

]

```

```python

for var in radar_variables:

    print(f"nVariable: {var}")

    print("2009 dimensions:", ds_2009[var].dims)

    print("2019 dimensions:", ds_2019[var].dims)

    print("2009 units:", ds_2009[var].attrs.get("units"))

    print("2019 units:", ds_2019[var].attrs.get("units"))

```

This check helps Ash answer practical questions:

* Does the same variable exist in both files?

* Is it organised over the same dimensions?

* Are the units the same?

* Is the meaning of the variable described in metadata?

For example, `equivalent_reflectivity_factor` is a radar variable related to precipitation, but it is not the same as rainfall amount. It is usually expressed in `dBZ`, while rainfall amount may be expressed in units such as `mm` or `mm h⁻¹`.

### Step 5: Select one variable for comparison

Ash starts with `equivalent_reflectivity_factor`.

```python

var = "equivalent_reflectivity_factor"

refl_2009 = ds_2009[var]

refl_2019 = ds_2019[var]

```

```python

refl_2009

```

```python

refl_2019

```

Now she checks whether both arrays use the same dimensions.

```python

print(refl_2009.dims)

print(refl_2019.dims)

```

If both use `time_processed_data` and `range`, Ash can compare them more easily.

### Step 6: Create a small subset**

To keep the demo fast, Ash selects only the first few time steps and the first part of the range dimension.

```python

subset_2009 = refl_2009.isel(time_processed_data=slice(0, 20), range=slice(0, 100))

subset_2019 = refl_2019.isel(time_processed_data=slice(0, 20), range=slice(0, 100))

```

This is an important practical benefit of OPeNDAP: Ash can request a subset of the remote data instead of downloading everything manually.

### Step 6b: Check missing values in each subset (Optional)

Before combining the subsets, Ash checks whether the selected parts of the data contain many missing values.

This matters because a visual comparison can be misleading if one subset contains much less valid data than the other.

```python

subset_2009 = subset_2009.load()

subset_2019 = subset_2019.load()

```

```python

def missing_value_summary(data_array, label):

    total_values = data_array.size

    nan_values = int(data_array.isnull().sum().item())

    valid_values = total_values - nan_values

    nan_percentage = 100 * nan_values / total_values

    return {

        "subset": label,

        "total_values": total_values,

        "valid_values": valid_values,

        "nan_values": nan_values,

        "nan_percentage": round(nan_percentage, 2),

    }

```

```python

nan_summary = pd.DataFrame(

    [

        missing_value_summary(subset_2009, "2009 subset"),

        missing_value_summary(subset_2019, "2019 subset"),

    ]

)

nan_summary

```

Ash can also add a simple warning threshold. Here, the threshold is set to 50%, but this is only a teaching choice.

```python

nan_threshold = 50

nan_summary["interpretation"] = nan_summary["nan_percentage"].apply(

    lambda value: "High number of missing values" if value > nan_threshold else "Acceptable for this demo"

)

nan_summary

```

This check helps Ash avoid comparing two subsets blindly. If one year contains many more missing values than the other, the difference in the plots may reflect data availability rather than a real difference in the radar signal.



### Step 7: Add a year coordinate and combine the subsets

Because the two files come from different dates, Ash first converts the selected time dimension into a simple relative index.

This means she compares the first 20 selected time steps from 2009 with the first 20 selected time steps from 2019.

```python

subset_2009 = subset_2009.assign_coords(

    time_processed_data=range(subset_2009.sizes["time_processed_data"])

)

subset_2019 = subset_2019.assign_coords(

    time_processed_data=range(subset_2019.sizes["time_processed_data"])

)

```

```python

subset_2009 = subset_2009.expand_dims(year=[2009])

subset_2019 = subset_2019.expand_dims(year=[2019])

```

```python

combined = xr.concat([subset_2009, subset_2019], dim="year", join="outer")

combined

```

Ash now has one small combined object containing the same radar variable from two different years.

```python

combined.name = "equivalent_reflectivity_factor"

combined_ds = combined.to_dataset()

combined_ds

```

#### Checking for NaN values after merging

```python

nan_comparison = pd.DataFrame(

    [

        {

            "year": 2009,

            "nan_before_combining": int(subset_2009.isnull().sum().item()),

            "nan_after_combining": int(combined_ds[var].sel(year=2009).isnull().sum().item()),

        },

        {

            "year": 2019,

            "nan_before_combining": int(subset_2019.isnull().sum().item()),

            "nan_after_combining": int(combined_ds[var].sel(year=2019).isnull().sum().item()),

        },

    ]

)

nan_comparison["extra_nans_after_combining"] = (

    nan_comparison["nan_after_combining"] - nan_comparison["nan_before_combining"]

)

nan_comparison

```

### Step 8: Plot the two years for comparison

Now Ash can make a visual comparison between the 2009 and 2019 subsets.

First, she plots the selected radar variable as a two-dimensional image, with `range` on one axis and `time_processed_data` on the other.

```python

combined_ds[var].plot(

    x="range",

    y="time_processed_data",

    col="year",

    robust=True,

)

plt.suptitle("Equivalent reflectivity factor comparison: 2009 and 2019", y=1.05)

plt.show()

```

This plot helps Ash visually inspect whether the structure of the radar signal looks similar or different between the two selected files.

However, two-dimensional plots can be difficult to compare in detail. Ash can also reduce each subset to a simple profile by averaging over time.

```python

mean_over_time = combined_ds[var].mean(dim="time_processed_data", skipna=True)

mean_over_time.plot.line(

    x="range",

    hue="year",

)

plt.title("Mean equivalent reflectivity factor over range")

plt.ylabel(combined_ds[var].attrs.get("units", "value"))

plt.show()

```

This plot shows how the average value of the selected radar variable changes across the range dimension for each year.

Ash can also average over range and compare how the signal changes across the selected time steps.

```python

mean_over_range = combined_ds[var].mean(dim="range", skipna=True)

mean_over_range.plot.line(

    x="time_processed_data",

    hue="year",

)

plt.title("Mean equivalent reflectivity factor over selected time steps")

plt.ylabel(combined_ds[var].attrs.get("units", "value"))

plt.show()

```

These plots are not a full scientific analysis. They are a first exploratory comparison that helps Ash understand whether the two datasets can be handled with a shared workflow.



### Step 9: Add useful metadata

```python

combined_ds.attrs["title"] = "Small combined IDRA reflectivity subset for interoperability demo"

combined_ds.attrs["source_datasets"] = "IDRA OPeNDAP files from 2009-04-27 and 2019-01-02"

combined_ds.attrs["purpose"] = "Demonstration of remote access, variable inspection, subsetting, and combination"

combined_ds.attrs["warning"] = (

    "This is a small teaching subset. It is not a complete scientific rainfall or drizzle analysis."

)

```

This step shows learners that combining data is not only a technical operation. Ash also needs to preserve enough metadata to explain where the data came from and what processing decisions were made.

### Step 10: Save the combined subset as Zarr

For repeated analysis, Ash may want to store the small combined subset in a format that is efficient for chunked, cloud-friendly access.

```python

combined_ds.to_zarr("idra_reflectivity_subset.zarr", mode="w")

```

Later, she can reopen it directly:

```python

reopened = xr.open_zarr("idra_reflectivity_subset.zarr")

reopened

```

This creates a small analysis-ready version of the subset. Instead of repeating the same remote access and harmonisation steps every time, Ash can reuse the prepared Zarr version in later notebooks or workflows.

Ash can access both IDRA files through OPeNDAP, inspect their NetCDF structure, select the same radar variable, and create a small combined subset. But meaningful comparison still depends on metadata, units, dimensions, coordinates, provenance, and clear documentation of the processing steps.

This is the practical meaning of interoperability: different datasets become useful together only when software can access them, humans can understand them, and workflows can reuse them reliably.



:::::::::::::::::::::::::::::::::::::::::::::::::: keypoints

- Technical interoperability enables machine-to-machine data exchange through standardized protocols.

- OPeNDAP implements the DAP protocol for remote access to structured scientific datasets.

- Remote datasets can be explored without full download.

- Server-side subsetting reduces bandwidth and supports scalable workflows.

- Data-access protocols can transform repositories from places where data are stored into infrastructures where data can also be accessed programmatically and selectively.

::::::::::