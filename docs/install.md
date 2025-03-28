# Installation

The Digitalhub SDK is distributed as a Python package and is hosted on [PyPI](https://pypi.org/project/digitalhub/) so you can install them with `pip`.

## Installing Digitalhub SDK

The most basic command to install the `digitalhub` package in your Python environment is:

```bash
python -m pip install digitalhub[full]
```

This command will install the SDK with all the dependencies.

There are more options available:

```bash
# Install SDK plus pandas for dataitems handling
python -m pip install digitalhub[pandas]

# Install SDK plus mlflow for mlflow model handling
python -m pip install digitalhub[mlflow]
```

## Installing Digitalhub Runtimes

The Digitalhub runtimes are installed in the same way as the SDK. We have distributed the following runtimes at the moment:

- [digitalhub-runtime-python](./runtimes/python/1-overview.md)
- [digitalhub-runtime-container](./runtimes/container.md)
- [digitalhub-runtime-dbt](./runtimes/dbt.md)
- [digitalhub-runtime-kfp](./runtimes/kfp.md)
- [digitalhub-runtime-modelserve](./runtimes/modelserve.md)

You can install the runtime directly with pip. It will come with all the required SDK dependencies:

```bash
# Install python runtime
python -m pip install digitalhub-runtime-python

# Install container runtime
python -m pip install digitalhub-runtime-container

# Install dbt runtime
python -m pip install digitalhub-runtime-dbt
python -m pip install digitalhub-runtime-dbt[local]

# Install kfp runtime
python -m pip install digitalhub-runtime-kfp

# Install modelserve runtime
python -m pip install digitalhub-runtime-modelserve
```
