# Entities

This page lists the entities used by the Python runtime and documents the parameters required or allowed when creating them.

## Function

The Python runtime introduces a Function of kind `python`.

### Function parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only when creating from the library; otherwise **MUST NOT** be set. | |
| name | str | Name that identifies the object. | required |
| [kind](#function-kinds) | str | Function kind. | required |
| uuid | str | Object ID in UUID4 format. | None |
| description | str | Description of the object. | None |
| labels | list[str] | List of labels. | None |
| embedded | bool | Whether the object should be embedded in the project. | True |
| [code_src](../../configuration/code_src/overview.md#code-source-uri) | str | URI pointing to the source code. | None |
| [code](../../configuration/code_src/overview.md#plain-text-source) | str | Source code provided as plain text. | None |
| base64 | str | Source code encoded as base64. | None |
| [handler](../../configuration/code_src/overview.md#handler) | str | Function entrypoint. | None |
| [init_function](#init-function) | str | Init function name for remote (Nuclio) execution. | None |
| [python_version](#python-versions) | str | Python version to use. | required |
| lang | str | Source code language (informational). | None |
| image | str | Container image used to execute the function. | None |
| [base_image](#base-image) | str | Base image (name:tag) used to build the execution image. | None |
| [requirements](#requirements) | list | List of pip requirements to install into the execution image. | None |

#### Function kinds

The `kind` parameter must be:

- `python`

#### Python versions

The Python runtime supports versions 3.9, 3.10 and 3.11, expressed as:

- `PYTHON3_9`
- `PYTHON3_10`
- `PYTHON3_11`

#### Init function

The init function is the entrypoint used by the Nuclio init wrapper. Specify the init function name via the `init_function` parameter.

#### Base image

The base image is the image (name:tag) used as the foundation when building the execution image for the function.

!!! warning
    Deploying jobs built from certain base images may be restricted by cluster security policies. Confirm allowed base images with your cluster administrator.

#### Requirements

Requirements are a list of `str` representing packages to be installed by `pip` in the image where the function will be executed.

```python
requirements = ["numpy", 'pandas>1, <3', "scikit-learn==1.2.0"]
```

### Function example

```python
# From project ...

function = project.new_function(name="python-function",
                                kind="python",
                                code_src="main.py",
                                handler="function",
                                python_version="PYTHON3_10")

# .. or from sdk

function = dh.new_function(project="my-project",
                           name="python-function",
                           kind="python",
                           code_src="main.py",
                           handler="function",
                           python_version="PYTHON3_10")
```

## Task

A set of tasks of kinds `job`, `serve` and `build` allow you to run a Python function execution, serve a function as a service, or build the Docker image used to execute the function.
A `Task` is created with the `run()` method, so it's not managed directly by the user. The parameters for the task creation are passed directly to the `run()` method, and may vary depending on the kind of task.

### Task parameters

| Name | Type | Description | Default | Kind specific |
| --- | --- | --- | --- | --- |
| [action](#task-actions) | str | Task action. | required | |
| [node_selector](../../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. | None | |
| [volumes](../../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. | None | |
| [resources](../../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. | None | |
| [affinity](../../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. | None | |
| [tolerations](../../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. | None | |
| [envs](../../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. | None | |
| [secrets](../../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. | None | |
| [profile](../../configuration/kubernetes/overview.md#profile) | str | Profile template. | None | |
| [replicas](../../configuration/kubernetes/overview.md#replicas) | int | Number of replicas. | None | `serve` |
| [service_type](../../configuration/kubernetes/overview.md#service-port-type) | str | Service type. | `NodePort` | `serve` |
| [instructions](#instructions) | list[str] | Build instructions executed as RUN lines in the generated Dockerfile. | None | `build` |

#### Task actions

Actions must be one of the following:

- `job`
- `build`
- `serve`

##### Serving

Use the `serve` action to deploy a function as a service on Kubernetes.

!!! warning "Service responsiveness"
    It may take some time for the service to become ready and for the platform to notify the client.

After the service is ready, call the inference endpoint with `run.invoke()`.
`run.invoke()` accepts the same keyword arguments as `requests.request`; by default the `url` is taken from the `run` object but you may override it with an explicit `url` parameter.

```python
run = function.run("serve", ...)

json = {
    "some-func-param": data
}

run.invoke(json=json)
```

#### Instructions

List of `str` representing the instructions to be executed as RUN instructions in Dockerfile.

```python
instructions = ["apt-get install -y git"]
```

### Task example

```python
run = function.run(
    action="build",
    instructions=["apt-get install -y git"]
)
```

## Run

The `Run` object is, similar to the `Task`, created with the `run()` method.
The run's parameters are passed alongside the task's ones.

### Run parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| local_execution | bool | Execute the run locally instead of remotely. | False |
| inputs | dict | Mapping of function argument names to entity keys. | None |
| parameters | dict | Extra parameters passed to the function. | None |
| init_parameters | dict | Parameters supplied to the init function. | None |

### Run example

```python
run = function.run(
    action="job",
    inputs={
        "dataitem": dataitem.key
    }
)
```

### Run methods

Once the run is created, you can access some of its attributes and methods through the `run` object.

::: digitalhub_runtime_python.entities.run.python_run.entity.RunPythonRun.output
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_python.entities.run.python_run.entity.RunPythonRun.outputs
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_python.entities.run.python_run.entity.RunPythonRun.result
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_python.entities.run.python_run.entity.RunPythonRun.results
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_python.entities.run.python_run.entity.RunPythonRun.invoke
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true
