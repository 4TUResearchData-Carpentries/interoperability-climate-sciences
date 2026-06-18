---
title: Setup
---

<!-- 
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


--- -->

## Project Setup

Create a working directory for this course:

```bash
cd ~/Desktop
mkdir Interoperability_climate_sciences
cd Interoperability_climate_sciences
```

---

## Software Setup

We will use **JupyterLab** for live coding and exercises.

This course requires:

* A Python 3 environment
* A Unix-like terminal
* Several Python libraries (installed via `requirements.txt`)

Follow the steps below carefully.

---

## 1. Install Python 3 (Required)

Download Python from:

👉 [https://www.python.org/downloads/](https://www.python.org/downloads/)

This course was tested with **Python 3.11**, but any supported version should work:
[https://devguide.python.org/versions/#versions](https://devguide.python.org/versions/#versions)

> ⚠️ Python 2.7 is not supported

---

### Verify Installation

Open a terminal and run:

```bash
python3 --version   # macOS / Linux
python --version    # Windows
```

Expected output (example):

```bash
Python 3.11.4
```

You can also start Python interactively:

```bash
python3   # or python on Windows
```

Exit with:

```bash
exit()
```

or press `CTRL+D`.

---

## 2. Set Up the Python Environment

We will:

1. Create a virtual environment
2. Define dependencies in `requirements.txt`
3. Install all libraries in one step

---

### Step 1 — Create a Virtual Environment

```bash
python3 -m venv nes-course-env
```

Activate it:

* **macOS / Linux**

  ```bash
  source nes-course-env/bin/activate
  ```

* **Windows (PowerShell)**

  ```bash
  nes-course-env\Scripts\Activate.ps1
  ```

You should now see `(nes-course-env)` in your terminal prompt.

---

### Step 2 — Create `requirements.txt`

Make sure you are in your project folder:

```bash
cd ~/Desktop/Interoperability_climate_sciences
```

Create a file named:

```bash
touch requirements.txt
```

Open the file in a text editor and add the following content:

```txt
# Core scientific stack
xarray
netCDF4
pydap
matplotlib
scipy

# Cloud-native data access
zarr
kerchunk
fsspec[http]
h5netcdf
h5py

# Interactive environment
jupyterlab
ipykernel
```

---

### Step 3 — Install Dependencies



Upgrade `pip` and install all packages:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

:::::::::::::::: instructor

You should send  this step prior to the lesson since it can take some time (~ 20 mins ) as part of a pre workshop email for example and recommend to the participants to complete the setup before the lesson if possible. If not , keep in mind to leave a 30 mins buffer to complete all installations. 

:::::::::::::::::


### Step 4 — Verify Installation (Recommended)

```bash
python -c "import xarray, netCDF4, pydap, zarr, kerchunk, fsspec; print('All good')"
```


### Step 5 — Register the Environment in Jupyter

```bash
python -m ipykernel install --user --name nes-course-env --display-name "NES Course (Python)"
```

---

### Step 6 — Launch JupyterLab

```bash
jupyter lab
```

In JupyterLab:

* Create a new notebook 
* Select kernel: **"NES Course (Python)"**

---

## 3. Unix Terminal (Required for API Episodes)

You will need a Unix-like terminal.

### Linux

Use the default terminal.

### macOS

Use the default Terminal app.

### Windows

Install one of:

* Git Bash: [https://git-scm.com/downloads](https://git-scm.com/downloads)
* Windows Subsystem for Linux (WSL): [https://learn.microsoft.com/en-us/windows/wsl/install](https://learn.microsoft.com/en-us/windows/wsl/install)

---

## 4. API Command-Line Tools (Required for REST API Episodes)

### `yq` (Required)

YAML processor for working with metadata.

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

Then install `yq`:

```bash
scoop install yq
```

---

### `jq` (Optional but Recommended)

JSON processor for formatting API output.

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

---

### Verify Installation

```bash
yq --version
jq --version
```

---








