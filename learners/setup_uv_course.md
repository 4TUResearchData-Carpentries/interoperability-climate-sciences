---
title: Setup
---

## Software setup

We will use **JupyterLab** for the hands-on exercises. The course uses **`uv`** to manage Python and install all required libraries, so you do not need to install Python or the packages separately.

### 1. Open a terminal

- **Windows:** Use [Git Bash](https://git-scm.com/downloads) for the course exercises. If it is not installed, install it first.
- **macOS:** Open the Terminal app.
- **Linux:** Open your usual terminal.

### 2. Install `uv`

If you already have `uv` installed, skip to the verification command below.

**Windows:** Open **PowerShell** and run:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

After installation, close PowerShell and open **Git Bash**.

**macOS and Linux:** Run this command in your terminal:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Close and reopen the terminal after installation. Then check that `uv` is available:

```bash
uv --version
```

If the command prints a version number, `uv` is ready. For other installation methods, see the [official `uv` instructions](https://docs.astral.sh/uv/getting-started/installation/).

### 3. Create the course folder and get the environment file

In **Git Bash** (Windows) or your terminal (macOS/Linux), run:

```bash
mkdir -p ~/Interoperability_climate_sciences
cd ~/Interoperability_climate_sciences
curl -fL -o pyproject.toml https://raw.githubusercontent.com/4TUResearchData-Carpentries/interoperability-climate-sciences/main/learners/files/pyproject.toml
```

The downloaded **`pyproject.toml`** lists the Python libraries required for the course.

### 4. Set up the course environment

In the same folder, run:

```bash
uv sync --python 3.11
```

`uv` will obtain Python 3.11 if needed, create the local **`.venv`** environment, and install the course libraries. It also creates a **`uv.lock`** file to record the resolved package versions. You do **not** need to activate the environment manually.

### 5. Start JupyterLab

Run:

```bash
uv run jupyter lab
```

JupyterLab will open in your browser. Create a notebook using the **Python 3 (ipykernel)** kernel to start working on the exercises.

**Your setup is complete.** For later course sessions, return to the same folder and run `uv run jupyter lab` again.
