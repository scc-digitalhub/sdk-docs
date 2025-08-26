# Container runtime

The Container runtime enables launching pods, jobs and services on Kubernetes. It is designed for remote, online execution.

## Prerequisites

Supported Python version and required package:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-container`

Install the runtime from PyPI:

```bash
python -m pip install digitalhub-runtime-container
```

## Overview

Use the Container runtime to run containers on Kubernetes. It exposes a Function of kind `container` and several task actions to run jobs, serve services, build images, or deploy workloads.

### Function

The Container runtime introduces a Function of kind `container` that describes how to execute a container-based workload.

#### Function parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only when creating from the library; otherwise **MUST NOT** be set. | |
| name | str | Name that identifies the object. | required |
| [kind](#function-kinds) | str | Function kind. | required |
| uuid | str | Object ID in UUID4 format. | None |
| description | str | Description of the object. | None |
| labels | list[str] | List of labels. | None |
| embedded | bool | Whether the object should be embedded in the project. | True |
| [code_src](../configuration/code_src/overview.md#code-source-uri) | str | URI pointing to the source code. | None |
| [code](../configuration/code_src/overview.md#plain-text-source) | str | Source code provided as plain text. | None |
| base64 | str | Source code encoded as base64. | None |
| [handler](../configuration/code_src/overview.md#handler) | str | Function entrypoint. | None |
| lang | str | Source code language (informational). | None |
| image | str | Container image to use for execution (name:tag). | None |
| base_image | str | Base image used when building the execution image. | None (required if task is `build`) |
| image_pull_policy | str | Kubernetes image pull policy. | None |
| command | str | Command to run inside the container. | None |

##### Function kinds

The `kind` parameter must be:

- `container`

#### Function example

```python
import digitalhub as dh

project = dh.get_or_create_project('my_project')
function = dh.new_function(
    kind='container',
    name='my_function',
    image='hello-world:latest'
)
```

### Task

The Container runtime supports four task actions: `job`, `serve`, `build`, and `deploy`. A `Task` is created via the `run()` method; task parameters are passed through that call and may vary by action.

#### Task parameters

| Name | Type | Description | Default | Kind specific |
| --- | --- | --- | --- | --- |
| [action](#task-actions) | str | Task action. | required | |
| [node_selector](../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. | None | |
| [volumes](../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. | None | |
| [resources](../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. | None | |
| [affinity](../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. | None | |
| [tolerations](../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. | None | |
| [envs](../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. | None | |
| [secrets](../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. | None | |
| [profile](../configuration/kubernetes/overview.md#profile) | str | Profile template. | None | |
| [fs_group](../configuration/kubernetes/overview.md#security-context) | int | File system group ID. | None | `deploy`, `job` |
| [run_as_user](../configuration/kubernetes/overview.md#security-context) | int | User ID to run the container. | None | `deploy`, `job`, `serve` |
| [run_as_group](../configuration/kubernetes/overview.md#security-context) | int | Group ID to run the container. | None | `deploy`, `job`, `serve` |
| [replicas](../configuration/kubernetes/overview.md#replicas) | int | Number of replicas. | None | `deploy`, `serve` |
| [service_ports](../configuration/kubernetes/overview.md#service-port-type) | list[dict] | Ports to expose for the service. | `NodePort` | `serve` |
| [service_type](../configuration/kubernetes/overview.md#service-port-type) | str | Service type. | `NodePort` | `serve` |
| [instructions](#instructions) | list[str] | Build instructions executed as RUN lines in the generated Dockerfile. | None | `build` |

##### Instructions

Instructions are executed as `RUN` instructions in the generated Dockerfile. Example:

```python
instructions = ["apt-get install -y git"]
```

##### Task actions

Supported actions:

- `job`
- `build`
- `serve`
- `deploy`

#### Task example

```python
run = function.run(
    action='job',
    instructions=["apt install git -y"],
)
```

### Run

The `Run` object is created by calling `run()` on a `Function`. Run-level parameters are provided alongside task parameters.

#### Run parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| args | list[str] | Command-line arguments to pass to the container command. | None |

#### Run example

```python
run = function.run(action='job')
```

#### Run methods

There are no runtime-specific helper methods for container runs.
