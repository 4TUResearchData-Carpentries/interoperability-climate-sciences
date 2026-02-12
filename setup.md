---
title: Setup
---

FIXME: Setup instructions live in this document. Please specify the tools and
the data sets the Learner needs to have installed.

## Data Sets

- **Make sure we the instructors create a  toy dataset from existing ones in 4TU.researchdata also hosted in OpenDAP server and publish it again in 4TU.researchdata and OpenDAP server.**

- We are considering :

       - IDRA datasets - https://data.4tu.nl/articles/_/12696887/1 CCO license

       - https://ruisdael-observatory.nl/data/ NOT available yet in 4TU.ResearchData

We want to modify one real dataset , such as it gets less complex and “more incomplete” for the part of reading and manipulating the dataset.

Why: 
The IDRA dataset has a license to allows us to reuse and also it has bigger size that we want to showcase how we can retrieve data stored in the web via a protocol that do not require the download of the data.

Implications: 

- We need to decide on which steps to take to modify the real good dataset.

- We need to create the toy data and stored it back to 4tu to be able to showcase the web retrieval.

Below is a **restructured and streamlined version** of your Software Setup section.
The goal is to:

* Separate **core setup** from **episode-specific requirements**
* Reduce redundancy
* Make the workflow explicit
* Encourage reproducible environments (venv-first approach)
* Clarify what is *required* vs *optional*

You can copy this as raw Markdown.

---

# Software Setup

This course requires a working Python 3 environment, a Unix-like terminal, and several Python libraries used throughout the episodes.

We strongly recommend setting up a **dedicated virtual environment** for this course.



## 1. Python 3 Installation

Download the latest Python 3 version from:

👉 [https://www.python.org/downloads/](https://www.python.org/downloads/)

The course has been tested with **Python 3.11**, but any currently supported version should work:
[https://devguide.python.org/versions/#versions](https://devguide.python.org/versions/#versions)

> ⚠️ Python 2.7 is not supported.



### Verify Your Installation

Open a terminal and run:

```bash
python3 --version   # macOS / Linux
python --version    # Windows
```

Expected output (example):

```bash
Python 3.11.4
```

To confirm you are using the standard Python distribution:

```bash
python3   # or python on Windows
```

You should see something like:

```bash
Python 3.11.4 (main, Jun 20 2023, ...)
>>> 
```

Exit with:

```bash
exit()
```

or `CTRL+D`.


## 2. Create a Virtual Environment (Recommended)

We use `venv` for environment isolation and `pip` for package management.

Create a virtual environment:

```bash
python3 -m venv nes-course-env
```

Activate it:

* macOS / Linux:

  ```bash
  source nes-course-env/bin/activate
  ```

* Windows (PowerShell):

  ```bash
  nes-course-env\Scripts\Activate.ps1
  ```

You should now see the environment name in your prompt.

Upgrade pip:

```bash
pip install --upgrade pip
```


## 3. Core Python Libraries (Required for Most Episodes)

Install the core scientific stack:

```bash
pip install xarray netCDF4
```

These are used for:

* Reading NetCDF datasets
* Data analysis
* Structural interoperability exercises



## 4. Additional Libraries for Cloud-Native Layouts (Episode on Zarr & Kerchunk)

For the cloud-native layouts episode, you will also need:

* `zarr`
* `kerchunk`
* `fsspec`
* `netCDF4` (or `h5netcdf`)

Install all required libraries with:

```bash
pip install xarray zarr kerchunk fsspec netCDF4
```

### What is `fsspec`?

`fsspec` (Filesystem Spec) is a Python library that provides a unified interface to multiple storage backends:

* Local filesystem
* HTTP / HTTPS
* S3
* Google Cloud Storage
* Azure Blob
* SSH
* In-memory storage

It enables libraries such as:

* `xarray`
* `zarr`
* `kerchunk`
* `dask`

to access remote data as if it were a local filesystem.

`fsspec` is often installed automatically as a dependency of `kerchunk`.


### What is `kerchunk`?

`kerchunk` enables **cloud-native access to NetCDF and HDF5 files without rewriting them to Zarr**.

It works by:

1. Scanning a NetCDF/HDF5 file
2. Extracting internal chunk metadata
3. Creating a JSON reference description
4. Allowing access via `fsspec` + `xarray` as if it were a Zarr dataset

This allows:

* Lazy loading
* Parallel access
* Cloud-optimized workflows
* Avoiding expensive data conversion

Kerchunk is especially relevant when:

* You cannot rewrite original NetCDF files
* Data is stored in object storage
* You want to improve interoperability in cloud workflows



## 5. Unix Terminal (Required for API Episodes)

You will need a Unix-like terminal.

### Linux

Native terminal is sufficient.

### macOS

Use the default Terminal app.

### Windows

Install one of:

* Git Bash: [https://git-scm.com/downloads](https://git-scm.com/downloads)
* Windows Subsystem for Linux (WSL): [https://learn.microsoft.com/en-us/windows/wsl/install](https://learn.microsoft.com/en-us/windows/wsl/install)



## 6. API Command-Line Tools (Required for REST API Episodes)

### `yq` (Required)

YAML processor used to manipulate metadata files.

#### Linux

```bash
sudo apt-get update
sudo apt install yq
```

#### macOS

```bash
brew install yq
```

#### Windows (PowerShell)

Install Scoop:

```bash
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

Then:

```bash
scoop install yq
```



### `jq` (Optional but Recommended)

JSON processor for nicely formatted API outputs.

#### Linux

```bash
sudo apt-get update
sudo apt-get install -y jq
```

#### macOS

```bash
brew install jq
```

#### Windows

```bash
scoop install main/jq
```

Verify installation:

```bash
yq --version
jq --version
```



## 7. Recommended IDE

You may use any editor, but we recommend:

* Visual Studio Code (VS Code)
  [https://code.visualstudio.com/](https://code.visualstudio.com/)

Install the Python extension for best experience.


# Summary of Requirements

### Minimum Required

* Python 3 (3.11 recommended)
* `venv`
* `pip`
* `xarray`
* `netCDF4`
* Unix-like terminal
* `yq`

### Required for Cloud-Native Episode

* `zarr`
* `kerchunk`
* `fsspec`

### Optional but Recommended

* `jq`
* VS Code


