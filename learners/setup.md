---
title: Setup
---

## 1. Software Setup

We will use **JupyterLab** for live coding and exercises.

This course requires:

- A Unix-like terminal
- `uv`, a Python package and project manager
- Python 3.11 or newer
- Several Python libraries, defined in `pyproject.toml`
- `jq` library - optional for API episode

---

### Get Unix Shell Terminal 

For practical exercises, we will often use a Unix-like terminal.

::: group-tab

### Windows

Install one of:

- [Git Bash](https://git-scm.com/downloads)
- [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install)


For this course, Git Bash is usually enough.

WSL is more powerful, but it may require more setup time.


### Mac

Use the default Terminal app.

Terminal can be found under **`/Applications/Utilities`**.

You can also search for "Terminal" through Spotlight.

### Linux

Use the default terminal.

:::


---


###  Install `uv` (Required)

We use `uv` instead of manually creating a virtual environment with `venv` and installing packages from `requirements.txt`.

With `uv`, the main workflow is:

```bash
uv sync
uv run jupyter lab
```

`uv sync` creates and updates the course environment.  
`uv run` runs commands inside that environment.

:::::::::::::::: callout

You do not need to manually activate the virtual environment during the course if you use `uv run`.

::::::::::::::::


Install `uv` using one of the options below.

::: group-tab

### Windows

In the Unix-shell terminal (e.g. Git Bash or WSL), run:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
After installation, close and reopen your terminal.

### Mac
In your terminal, run:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
After installation, close and reopen your terminal.

### Linux

In your terminal, run:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
After installation, close and reopen your terminal.

:::

:::::::::::::::: callout
### Alternative installation methods

You can also install `uv` with package managers such as Homebrew, Winget, Scoop, or `pipx`.

See the official installation instructions:

<https://docs.astral.sh/uv/getting-started/installation/>

:::

---

#### Verify `uv` Installation

Open a terminal and run:

```bash
uv --version
```

Expected output:

```bash
uv 0.x.x
```

The exact version number may be different.

:::::::::::::::: caution

If your terminal says `uv: command not found`, close and reopen the terminal and try again.

If it still does not work, check whether the installation directory was added to your `PATH`.

::::::::::::::::

#### Useful `uv` Commands During the Course

Run Python inside the course environment:

```bash
uv run python
```

Run a Python script:

```bash
uv run python scripts/example.py
```

Run JupyterLab:

```bash
uv run jupyter lab
```

Install a new package and add it to `pyproject.toml`:

```bash
uv add package-name
```

Synchronise the environment after `pyproject.toml` changes:

```bash
uv sync
```

Show the installed dependency tree:

```bash
uv tree
```

---


### Install or Check Python

This course was tested with **Python 3.11**.

`uv` can use an existing Python installation or install Python for you.

To install Python 3.11 with `uv`, run:

```bash
uv python install 3.11
```

Then verify that Python is available:

```bash
uv run python --version
```

Expected output:

```bash
Python 3.11.x
```

A newer Python 3 version may also work, but Python 3.11 is recommended for the course.

:::::::::::::::: caution

Python 2.7 is not supported.

Please use Python 3.11 or newer.

::::::::::::::::

:::::::::::::::: callout

If you already have Python installed, `uv` may use your existing Python version automatically.

::::::::::::::::

---

### Install `jq` (Recommended for for REST API Episodes)

`jq` is a command-line tool for reading and formatting JSON output. 
It is useful when working with REST APIs.

::: group-tab

#### Windows

With WSL:

```bash
sudo apt-get update
sudo apt-get install -y jq
```

With Git Bash:

```bash
winget install jqlang.jq
```

#### Mac

```bash
brew install jq
```

#### Linux

```bash
sudo apt-get update
sudo apt-get install -y jq
```
:::


---

### Verify `jq` Installation

```bash
jq --version
```

Expected output:

```bash
jq-1.x
```

---

## 2. Project Setup

Create a course working directory somewhere convenient, for example your home directory:

```bash
mkdir  ~/Interoperability_climate_sciences
cd ~/Interoperability_climate_sciences
```

This folder will contain the course environment files, notebooks, scripts, and any downloaded data used during the exercises.

## 3. Environment Setup 

### Download the Course Environment File

Make sure you are inside the course folder:

```bash
cd ~/Interoperability_climate_sciences
```

Download the course environment file:

```bash
curl -o pyproject.toml \
https://raw.githubusercontent.com/4TUResearchData-Carpentries/interoperability-climate-sciences/main/learners/files/pyproject.toml
```
The `pyproject.toml` file defines the Python dependencies required for this course.

Verify that the file was downloaded successfully:

```bash
ls
```

Expected output:

```bash 
pyproject.toml
```


- Generate the lockfile before the workshop with:

```bash
uv lock
```

The `uv.lock` file records the resolved package versions and improves reproducibility across learners' machines.


:::::::::::::::: callout

If the download fails, open the [download URL](https://raw.githubusercontent.com/4TUResearchData-Carpentries/interoperability-climate-sciences/main/learners/files/pyproject.toml) in a web browser and save the file as: `pyproject.toml` inside your Interoperability_climate_sciences folder.

:::

---

### Create and Synchronise the Environment

Run:

```bash
uv sync
```

This command will:

- create a local `.venv` folder if it does not exist;
- install all packages listed in `pyproject.toml`;
- create or update the `uv.lock` file.

:::::::::::::::: instructor

Send this step to participants before the lesson.

The first `uv sync` can take some time, depending on the internet connection and operating system.

Recommended pre-workshop instruction:

```bash
cd ~/Interoperability_climate_sciences
curl -o pyproject.toml \
https://raw.githubusercontent.com/4TUResearchData-Carpentries/interoperability-climate-sciences/main/learners/files/pyproject.toml
uv sync
```

If participants cannot complete this before the lesson, keep a 20-30 minute setup buffer at the beginning of the workshop.

::::::::::::::::

:::::::::::::::: callout

The `.venv` folder is the virtual environment created by `uv`.

Learners do not need to activate it manually if they use commands starting with `uv run`.

::::::::::::::::

---

### Verify the Python Environment

Run:

```bash
uv run python -c "import xarray, netCDF4, pydap, zarr, kerchunk, fsspec, h5netcdf, h5py, scipy, pandas, requests, cf_xarray; print('All good')"
```

Expected output:

```bash
All good
```

If this command works, the course Python environment is ready.

---

### Register the Environment in Jupyter

Register the course environment as a Jupyter kernel:

```bash
uv run python -m ipykernel install --user --name nes-course-env --display-name "NES Course (Python)"
```

This makes the environment available inside JupyterLab as:

```text
NES Course (Python)
```

---

### Launch JupyterLab

Launch JupyterLab with:

```bash
uv run jupyter lab
```

In JupyterLab, click on the button **NES Course (Python)** under Notebook.

![](fig/setup-jupyterlab.png){alt="Screenshot from JupyterLab Launcher with title Notebook and two Python icons underneath: one named 'Python 3 (ipykernel)' and the other named 'NES Course (Python)'"}

:::::::::::::::: caution

If you open JupyterLab without `uv run`, you may accidentally use a different Python environment.

Recommended:

```bash
uv run jupyter lab
```

Avoid:

```bash
jupyter lab
```

unless you are sure your terminal is using the correct environment.

::::::::::::::::

---

## 4. Final Setup Check

Before the workshop, make sure the following commands work:

```bash
uv --version
uv run python --version
uv sync
uv run python -c "import xarray, netCDF4, pydap, zarr, kerchunk, fsspec; print('All good')"
uv run jupyter lab
jq --version
```

If all commands work, you are ready for the course.


## 5. Troubleshooting

### `uv: command not found`

Close and reopen your terminal.

Then try:

```bash
uv --version
```

If it still fails, reinstall `uv` or check whether the installation folder was added to your `PATH`.

---

### JupyterLab opens but the course kernel is missing

Run:

```bash
uv run python -m ipykernel install --user --name nes-course-env --display-name "NES Course (Python)"
```

Then restart JupyterLab:

```bash
uv run jupyter lab
```

---

### A package import fails

Run:

```bash
uv sync
```

Then verify again:

```bash
uv run python -c "import xarray, netCDF4, pydap, zarr, kerchunk, fsspec; print('All good')"
```

---

### You are not sure which Python is being used

Run:

```bash
uv run python --version
uv run python -c "import sys; print(sys.executable)"
```

The executable path should point to the `.venv` folder inside your course directory.

Example:

```text
.../Interoperability_climate_sciences/.venv/...
```

---

### Optional Fallback: `venv` and `requirements.txt`

Use this fallback only if `uv` cannot be installed on your machine.

:::::::::::::::: caution

The recommended setup for this course is `uv`.

Use this section only if your institution blocks `uv` installation or if you cannot get `uv` working before the lesson.

::::::::::::::::

Create a virtual environment:

```bash
python -m venv nes-course-env
```

Activate it.

::: group-tab
### Windows 

WSL:
```bash
source nes-course-env/bin/activate
```

Git Bash:
```bash
source nes-course-env/Scripts/activate
```

### Mac 

```bash
source nes-course-env/bin/activate
```

###  Linux

```bash
source nes-course-env/bin/activate
```
:::

Install dependencies from a `requirements.txt` file provided by the instructors:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Register the environment in Jupyter:

```bash
python -m ipykernel install --user --name nes-course-env --display-name "NES Course (Python)"
```

Launch JupyterLab:

```bash
jupyter lab
```

:::::::::::::::: instructor

If you want to provide a fallback `requirements.txt`, generate it from `pyproject.toml` using:

```bash
uv pip compile pyproject.toml -o requirements.txt
```

Then commit the generated `requirements.txt` to the repository as a fallback, not as the main source of truth.

::::::::::::::::

---

