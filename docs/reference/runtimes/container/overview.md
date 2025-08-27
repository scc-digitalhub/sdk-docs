# Container Runtime

The Container runtime enables launching pods, jobs and services on Kubernetes. It is designed for remote, online execution.

## Prerequisites

Supported Python version and required package:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-container`

Install the runtime from PyPI:

```bash
python -m pip install digitalhub-runtime-container
```

## Usage overview

Use the Container runtime to run containers on Kubernetes. It exposes a Function of kind `container` and several task actions to run jobs, serve services, build images, or deploy workloads.

- **`job`**: Execute a container as a one-off job
- **`serve`**: Deploy a container as a long-lived service
- **`build`**: Create a Docker image with custom instructions
- **`deploy`**: Deploy an application workload

To execute a container workload on the platform:

1. Create a `Function` resource that references your container image and declares inputs/outputs and call `function.run()` to [execute](execution.md) the workload.

See [Examples](examples.md) for code samples.
