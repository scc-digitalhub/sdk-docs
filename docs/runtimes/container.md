# Container runtime

The **Container runtime** allows you to create deployments, jobs and services on Kubernetes.

## Prerequisites

Python version and libraries:

- `python >= 3.9`
- `digitalhub-runtime-container`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-container
```

## HOW TO

With the Container runtime you can launch pods and services on Kubernetes. It is built having **remote online execution** capabilities.

### Function

The Container runtime introduces a function of kind `container` that allows you to deploy deployments, jobs and services on Kubernetes.

#### Function parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only if creating from library, otherwise **MUST NOT** be set | |
| name | str | Name that identifies the object | required |
| [kind](#function-kinds) | str | Function kind | required |
| uuid | str | ID of the object in form of UUID4 | None |
| description | str | Description of the object | None |
| labels | list[str] | List of labels | None |
| embedded | bool | Flag to determine if object must be embedded in project | True |
| [code_src](../objects/code_source.md#code-source-uri) | str | URI pointer to source code | None |
| [code](../objects/code_source.md#plain-text-source) | str | Source code (plain text)| None |
| [base64](../objects/code_source.md#base64-encoded-source) | str | Source code (base64 encoded)| None |
| [handler](../objects/code_source.md#handler) | str | Function entrypoint | None |
| lang | str | Source code language (hint)| None |
| image | str | The image to use | None |
| base_image | str | The base container image | None (required if task is `build`) |
| command | str | The command to run inside the container | None |
| args | list[str] | The arguments to pass to the command | None |

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
    image="hello-world:latest"
)
```

### Task

The container runtime introduces four tasks of kind `job`, `serve`, `build` and `deploy` that allows you to run a Kubernetes job, create a service or a deployment and build an image.
A `Task` is created with the `run()` method, so it's not managed directly by the user. The parameters for the task creation are passed directly to the `run()` method, and may vary depending on the kind of task.

#### Task parameters

| Name | Type | Description | Default | Kind specific |
| --- | --- | --- | --- | --- |
| [action](#task-actions) | str | Task action | required | |
| [node_selector](kubernetes-resources.md#node-selector) | list[dict] | Node selector | None | |
| [volumes](kubernetes-resources.md#volumes) | list[dict] | List of volumes | None | |
| [resources](kubernetes-resources.md#resources) | dict | Resources restrictions | None | |
| [affinity](kubernetes-resources.md#affinity) | dict | Affinity | None | |
| [tolerations](kubernetes-resources.md#tolerations) | list[dict] | Tolerations | None | |
| [envs](kubernetes-resources.md#envs) | list[dict] | Env variables | None | |
| [secrets](kubernetes-resources.md#secrets) | list[str] | List of secret names | None | |
| [profile](kubernetes-resources.md#profile) | str | Profile template | None | |
| [backoff_limit](kubernetes-resources.md#backoff-limit) | int | Backoff limit | None | `job` |
| [schedule](kubernetes-resources.md#schedule) | str | Schedule for the job | None | `job` |
| [replicas](kubernetes-resources.md#replicas) | int | Number of replicas | None | `deploy`, `serve` |
| [service_port](kubernetes-resources.md#service-port) | list[dict] | Service port where to expose the service | `NodePort` | `serve` |
| [service_type](kubernetes-resources.md#service-type) | str | Service type | `NodePort` | `serve` |
| instructions | list[str] | Build instructions to be executed as RUN instructions in Dockerfile | None | `build` |

##### Task actions

Actions must be one of the following:

- `job`
- `build`
- `serve`
- `deploy`

#### Task example

```python
run = function.run(
    action="job",
    instructions=["apt install git -y"],
)
```

### Run

The `Run` object is, similar to the `Task`, created with the `run()` method.
The run's parameters are passed alongside the task's ones.

#### Run parameters

There are no parameters for the `run` spec.

#### Run example

```python
run = function.run(action="job")
```
