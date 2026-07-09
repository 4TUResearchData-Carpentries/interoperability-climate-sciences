---
title: Setup
---

## Project Setup

Create a working directory for this course:

```bash
cd ~/Desktop
mkdir Interoperability_climate_sciences
cd Interoperability_climate_sciences
```

This folder will contain the course environment files, notebooks, scripts, and any downloaded data used during the exercises.

---

## Software Setup

We will use **JupyterLab** for live coding and exercises.

This course requires:

- `uv`, a Python package and project manager
- Python 3.11 or newer
- A Unix-like terminal
- Several Python libraries, defined in `pyproject.toml`

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

---

## 1. Install `uv` (Required)

Install `uv` using one of the options below.

### macOS / Linux

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

After installation, close and reopen your terminal.

### Windows PowerShell

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

After installation, close and reopen PowerShell.

### Alternative installation methods

You can also install `uv` with package managers such as Homebrew, Winget, Scoop, or `pipx`.

See the official installation instructions:

<https://docs.astral.sh/uv/getting-started/installation/>

---

### Verify `uv` Installation

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

---

## 2. Install or Check Python

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

## 3. Create the Course Environment File

Make sure you are inside the course folder:

```bash
cd ~/Desktop/Interoperability_climate_sciences
```

Create a file named:

```bash
pyproject.toml
```

The `pyproject.toml` file defines the direct dependencies of the course. Open the file in a text editor and add the following content:

```toml
[project]
name = "interoperability-climate-sciences"
version = "0.1.0"
description = "Course environment for interoperability in climate and atmospheric sciences"
requires-python = ">=3.11"
dependencies = [
    # Core scientific stack
    "xarray",
    "netCDF4",
    "pydap",
    "matplotlib",
    "scipy",
    "pandas",

    # Cloud-native and remote data access
    "zarr",
    "kerchunk",
    "fsspec[http]",
    "h5netcdf",
    "h5py",

    # Metadata and conventions
    "cf-xarray",

    # API access
    "requests",

    # Interactive environment
    "jupyterlab",
    "ipykernel",
]
```

Save the file.

- Generate the lockfile before the workshop with:

```bash
uv lock
```

The `uv.lock` file records the resolved package versions and improves reproducibility across learners' machines.

---

## 4. Create and Synchronise the Environment

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
cd ~/Desktop/Interoperability_climate_sciences
touch pyproject.toml
uv sync
```

If participants cannot complete this before the lesson, keep a 20-30 minute setup buffer at the beginning of the workshop.

::::::::::::::::

:::::::::::::::: callout

The `.venv` folder is the virtual environment created by `uv`.

Learners do not need to activate it manually if they use commands starting with `uv run`.

::::::::::::::::

---

## 5. Verify the Python Environment

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

## 6. Register the Environment in Jupyter

Register the course environment as a Jupyter kernel:

```bash
uv run python -m ipykernel install --user --name nes-course-env --display-name "NES Course (Python)"
```

This makes the environment available inside JupyterLab as:

```text
NES Course (Python)
```

---

## 7. Launch JupyterLab

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

## 8. Useful `uv` Commands During the Course

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

## 9. Unix Terminal (Required for API Episodes)

You will need a Unix-like terminal for the API episodes.

### Linux

Use the default terminal.

### macOS

Use the default Terminal app.

Terminal can be found under:

```text
/Applications/Utilities
```

You can also search for "Terminal" through Spotlight.

### Windows

Install one of:

- Git Bash: <https://git-scm.com/downloads>
- Windows Subsystem for Linux WSL: <https://learn.microsoft.com/en-us/windows/wsl/install>

:::::::::::::::: callout

For this course, Git Bash is usually enough.

WSL is more powerful, but it may require more setup time.

::::::::::::::::

---

## 10. API Command-Line Tools (Required for REST API Episodes)

### `jq` Optional but Recommended

`jq` is a command-line tool for reading and formatting JSON output.

It is useful when working with REST APIs.

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

Using Scoop:

```powershell
scoop install main/jq
```

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

## 11. Optional Fallback: `venv` and `requirements.txt`

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

### macOS / Linux

```bash
source nes-course-env/bin/activate
```

### Windows PowerShell

```powershell
nes-course-env\Scripts\Activate.ps1
```

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

## 12. Troubleshooting

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

## 13. Final Setup Check

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
