# Installation

The Digitalhub SDK is distributed as a Python package and is hosted on [PyPI](https://pypi.org/project/digitalhub/) so you can install them with `pip`.

## Installing Digitalhub SDK

The most basic command to install the `digitalhub` package in your Python environment is:

```shell
python -m pip install digitalhub[full]
```

This command will install the SDK with all the various layer packages (core, data, ml).

There are more options available:

```shell
python -m pip install digitalhub[ml]
python -m pip install digitalhub[data]
python -m pip install digitalhub
```

Everyone of them will install the digitalhub up to a certain layer. Please note that the layer are pyramidal and proceed as follows:

- `core` -> `data` -> `ml`

## Installing Digitalhub Runtimes

The Digitalhub runtimes are installed in the same way as the SDK. We have distributed the following runtimes at the moment:

- digitalhub-runtime-python
- digitalhub-runtime-container
- digitalhub-runtime-dbt
- digitalhub-runtime-kfp

You can install the runtime directly with pip. It will come with all the required SDK dependencies:

```shell
# Install python runtime + core + data + ml
python -m pip install digitalhub-runtime-python

# Install container runtime + core
python -m pip install digitalhub-runtime-container

# Install dbt runtime + data (local option for local execution)
python -m pip install digitalhub-runtime-dbt
python -m pip install digitalhub-runtime-dbt[local]

# Install kfp runtime + core + data + ml
python -m pip install digitalhub-runtime-kfp

# Install modelserve runtime + core + data + ml
python -m pip install digitalhub-runtime-modelserve
```
