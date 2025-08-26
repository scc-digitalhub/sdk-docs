# Python runtime

The **python runtime** allows you to run generic python function.
The runtime introduces a function of kind `python` and three task of kind `job`, `serve` and `build`.
With `job` action you can run a python function as a job.
With `serve` action you can run a python function as a service.
With `build` action you build a docker image with all the dependencies and reuse that container to run the function avoiding the overhead of dependencies installation during the runtime.

## Prerequisites

Python version and libraries:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-python`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-python
```

## Overwiew

With the python runtime you can use the function's `run()` method to execute a python function you have defined.
The python runtime execution workflow follows roughly these steps:

1. Define somewhere a [python function](2-function.md).
2. Create a `Function` object in the platform and execute the function's `run()` method.
3. The runtime collects the [inputs](2-function.md#inputs-and-parameters) specified in the function as SDK objects (`Dataitem`, `Artifact`, `Model`).
4. It fetches the function [source code](../../configuration/code_src/overview.md) and import the function handler.
5. It [composes](2-function.md#reserved-arguments) the parameters for the handler function.
6. It executes the function and map the [outputs](2-function.md#handler-and-outputs) as SDK objects or as simple results.

## User HOWTO

As delined in the description of the runtime workflow, to execute a python function the user should proceed with the following three steps:

1. Write the function to execute.
2. Create a `Function` object.
3. Execute the function's `run()` method.

You can jump now on [function definition](2-function.md).
