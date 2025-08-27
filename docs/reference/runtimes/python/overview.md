# Python runtime

The Python runtime enables you to run user-defined Python handlers. It registers a `Function` kind `python` and supports three actions: `job`, `serve`, and `build`.

- `job` — run a Python handler as a one-off job.
- `serve` — run a Python handler as a long-lived service.
- `build` — construct a Docker image that bundles all required dependencies so the runtime can reuse the image and avoid re-installing dependencies at execution time.

## Prerequisites

Supported Python versions and required package:

- Python >= 3.9, < 3.13
- `digitalhub-runtime-python`

Install from PyPI:

```bash
python -m pip install digitalhub-runtime-python
```

## Overview

Use a `Function` object's `run()` method to invoke your handler. The Python runtime generally follows these steps:

1. You write a Python handler function (see [Function definition](define-function.md)).
2. You create a `Function` resource on the platform and call its `run()` method.
3. The runtime collects the function's declared inputs (objects such as `Dataitem`, `Artifact`, or `Model`).
4. It fetches the function [source code](../../configuration/code_src/overview.md) and imports the handler.
5. It composes the handler arguments according to the function declaration and reserved parameters (see [reserved arguments](define-function.md#reserved-arguments)).
6. It executes the handler and maps the outputs back into SDK objects or returns them as simple results (see [handler and outputs](define-function.md#handler-and-outputs)).

## Quick how-to

To execute a Python handler on the platform:

1. Implement the handler as described in [function definition](define-function.md).
2. Create a `Function` resource that references your handler and declares inputs/outputs.
3. Invoke the `Function.run()` method to execute the handler.

For details on handler signatures, inputs/outputs, and examples, see [function definition](define-function.md).

Here you can find [examples](examples.md).
