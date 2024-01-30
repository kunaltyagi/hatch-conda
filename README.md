# hatch-conda

| | |
| --- | --- |
| CI/CD | [![CI - Test](https://github.com/OldGrumpyViking/hatch-conda/actions/workflows/test.yml/badge.svg)](https://github.com/OldGrumpyViking/hatch-conda/actions/workflows/test.yml) [![CD - Build](https://github.com/OldGrumpyViking/hatch-conda/actions/workflows/build.yml/badge.svg)](https://github.com/OldGrumpyViking/hatch-conda/actions/workflows/build.yml) |
| Package | [![PyPI - Version](https://img.shields.io/pypi/v/hatch-conda.svg?logo=pypi&label=PyPI&logoColor=gold)](https://pypi.org/project/hatch-conda/) [![PyPI - Python Version](https://img.shields.io/pypi/pyversions/hatch-conda.svg?logo=python&label=Python&logoColor=gold)](https://pypi.org/project/hatch-conda/) |
| Meta | [![code style - black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black) [![types - Mypy](https://img.shields.io/badge/types-Mypy-blue.svg)](https://github.com/ambv/black) [![imports - isort](https://img.shields.io/badge/imports-isort-ef8336.svg)](https://github.com/pycqa/isort) [![License - MIT](https://img.shields.io/badge/license-MIT-9400d3.svg)](https://spdx.org/licenses/) |

-----

This provides a plugin for [Hatch](https://github.com/pypa/hatch) that allows the use of conda [environments](https://hatch.pypa.io/latest/environment/).

This project is a copied and modified version of the [hatch-containers](https://github.com/ofek/hatch-containers) plugin by Ofek Lev.

**Table of Contents**

- [Installation](#installation)
- [Configuration](#configuration)
  - [Python](#python)
  - [Command](#command)
  - [Conda-forge](#conda-forge)
  - [Environment file](#environment-file)
- [Notes](#notes)
- [License](#license)

## Installation

```console
pip install hatch-conda
```

## Configuration

### Environment
The [environment plugin](https://hatch.pypa.io/latest/plugins/environment/) name is `conda`. It allows using a conda environment for managing dependencies

- ***pyproject.toml***

    ```toml
    [tool.hatch.envs.<ENV_NAME>]
    type = "conda"
    ```

- ***hatch.toml***

    ```toml
    [envs.<ENV_NAME>]
    type = "conda"
    ```

### Environment Collector
The [environment collector plugin](https://hatch.pypa.io/latest/plugins/environment-collector/) name is `conda`. It allows using the conda file to keep track of python dependencies, and prevents splitting dependencies between the conda file and `pyproject.toml`. It doesn't need conda to be installed and can be used without the [environment plugin](#environment)

- ***pyproject.toml***

    ```toml
    [tool.hatch.env]
    requires = ["hatch-conda"]
    [tool.hatch.env.collectors.conda.<ENV_NAME>]
    environment-file = "file.yaml"
    ```

- ***hatch.toml***

    ```toml
    [env]
    requires = ["hatch-conda"]
    [env.collectors.conda.<ENV_NAME>]
    environment-file = "file.yaml"
    ```

### Build Hook
The [build hook plugin](https://hatch.pypa.io/latest/plugins/build-hook/reference/) name is `conda`. It allows using the conda file to keep track of python dependencies, and prevents splitting dependencies between the conda file and `pyproject.toml`. It doesn't need conda to be installed and can be used without other plugins. Please note that non-pip dependencies are not included in the dependency details for targets like wheel.

- ***pyproject.toml***

    ```toml
    [tool.hatch.env]
    requires = ["hatch-conda"]
    [tool.hatch.build.targets.wheel.hooks.conda]
    environment-file = "file.yaml"
    ```

- ***hatch.toml***

    ```toml
    [env]
    requires = ["hatch-conda"]
    [build.targets.wheel.hooks.conda]
    environment-file = "file.yaml"
    ```

### Python

If the [Python version](https://hatch.pypa.io/latest/config/environment/#python-version) is set to a multi-character integer like `310` then it will be interpreted as its `<MAJOR>.<MINOR>` form e.g. `3.10`.

If not set, then the `<MAJOR>.<MINOR>` version of the first `python` found along your `PATH` will be used, defaulting to the Python executable Hatch is running on.

### Command

The `command` option specifies the command that will be used to setup the environment. The possible options are `conda`, `mamba` and `micromamba`.

Default:

```toml
[envs.<ENV_NAME>]
command = "conda"
```

### Conda-forge

Indicates if the conda-forge index should be used.

Default:

```toml
[envs.<ENV_NAME>]
conda-forge = true
```

### Environment file
By default packages will be installed using pip. However, to install packages using conda, conda-forge, or any other channel, you can specify a conda environment file:

```toml
[envs.<ENV_NAME>]
environment-file = "environment.yml"
```

When using an environment file, the channel and python version specified in the environment file will be used. After installing the environment, any extra packages specified in the dependencies will be installed, as well as the local package.

### Prefix environments

It's possible to identify environment py prefix (the path in the filesytem) rather than by name.

```toml
[envs.<ENV_NAME>]
prefix = .venv/
```

Will create a conda environment in the `.venv/` folder. This is useful because a text editor might check for this location to use the virtual environment.

## Notes

- There must be a `conda`, `mamba`, or `micromamba` executable along your `PATH`.
- The `env-exclude` [environment variable filter](https://hatch.pypa.io/latest/config/environment/#filters) has no effect.

## License

`hatch-conda` is distributed under the terms of the [MIT](https://spdx.org/licenses/MIT.html) license.
