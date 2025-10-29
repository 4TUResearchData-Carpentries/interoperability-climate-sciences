---
title: Setup
---

FIXME: Setup instructions live in this document. Please specify the tools and
the data sets the Learner needs to have installed.

## Data Sets

- **Make sure we the instructors create a  toy dataset from existing ones in 4TU.researchdata also hosted in OpenDAP server and publish it again in 4TU.researchdata and OpenDAP server.**

## Software Setup

- [Python 3 distribution](#python-3-distribution)
- Install libraries needed or live in the workshop
  

## Python 3 Distribution

To download the latest Python 3 distribution for your operating system,
please head to [Python.org](https://www.python.org/downloads/).

If you are on Linux,
it is likely that the system Python 3 already installed will satisfy the requirements
of this course (the material has been tested using the standard Python distribution version 3.11
but any [supported version](https://devguide.python.org/versions/#versions) should work).

The course uses `venv` for virtual environment management and `pip` for package management.
The material has not been extensively tested with other Python distributions and package managers,
but most sections are expected to work with some modifications.
For example, package installation and virtual environments would need to be managed differently, but Python script
invocations should remain the same regardless of the Python distribution used.

:::::::::::::::::::::::::::::::::::::::::  callout

## Recommended Python Version

We recommend using the latest Python version but any [supported version](https://devguide.python.org/versions/#versions)
should work.
Specifically, we recommend upgrading from Python 2.7 wherever possible;
continuing to use it will likely result in difficulty finding supported dependencies or syntax errors.


::::::::::::::::::::::::::::::::::::::::::::::::::

You can
test your Python installation from the command line with:

```bash
$ python3 --version # on Mac/Linux
$ python --version # on Windows — Windows installation comes with a python.exe file rather than a python3.exe file 
```

If you are using Windows and invoking `python` command causes your Git Bash terminal to hang with no error message or output, you may
need to create an alias for the python executable `python.exe`, as explained in the [troubleshooting section](learners/common-issues.md#python-hangs-in-git-bash).

If all is well with your installation, you should see something like:

```output
Python 3.11.4
```

To make sure you are using the standard Python distribution and not some other distribution you may have on your system,
type the following in your shell:

```bash
$ python3 # python on Windows
```

This should enter you into a Python console and you should see something like:

```bash
Python 3.11.4 (main, Jun 20 2023, 17:23:00) [Clang 14.0.3 (clang-1403.0.22.14.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information. 
>>> 
```

Press `CONTROL-D` or type `exit()` to exit the Python console.

### `venv` and `pip`

If you are using a Python 3 distribution from [Python.org](https://www.python.org/),
`venv` and `pip` will be automatically installed for you. If not, please make sure you have these
two tools (that correspond to your Python distribution) installed on your machine.

## IDE


### VS Code

Alternatively, you can use Microsoft's [Visual Studio Code (VS Code)](https://code.visualstudio.com/).

