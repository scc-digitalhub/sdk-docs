# Enitities

Here follows the list of entities used in the python runtime and the parameters required/allowed to create them.

## Function

The python runtime introduces a function of kind `python`.

### Function parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only if creating from library, otherwise **MUST NOT** be set | |
| name | str | Name that identifies the object | required |
| [kind](#function-kinds) | str | Function kind | required |
| uuid | str | ID of the object in form of UUID4 | None |
| description | str | Description of the object | None |
| labels | list[str] | List of labels | None |
| embedded | bool | Flag to determine if object must be embedded in project | True |
| [code_src](../../objects/code_source.md#code-source-uri) | str | URI pointer to source code | None |
| [code](../../objects/code_source.md#plain-text-source) | str | Source code (plain text)| None |
| [base64](../../objects/code_source.md#base64-encoded-source) | str | Source code (base64 encoded)| None |
| [handler](../../objects/code_source.md#handler) | str | Function entrypoint | None |
| [init_function](#init-function) | str | Init function for remote nuclio execution | None |
| [python_version](#python-versions) | str | Python version to use | required |
| lang | str | Source code language (hint)| None |
| image | str | Image where the function will be executed | None |
| [base_image](#base-image) | str | Base image used to build the image where the function will be executed | None |
| [requirements](#requirements) | list | Requirements list to be installed in the image where the function will be executed | None |

#### Function kinds

The `kind` parameter must be:

- `python`

#### Python versions

The python runtime supports Python versions 3.9, 3.10 and 3.11, expressed respectively as:

- `PYTHON3_9`
- `PYTHON3_10`
- `PYTHON3_11`

#### Init function

The init function is the entrypoint of the nuclio init function. The user must pass the name of the init function in the `init_function` parameter.

#### Base image

The base image is a string that represents the image (name:tag) used to build the image where the function will be executed.

!!! warning
      It is possible that the platform where you deploy a job after a `build` action with a root image will not work because of security policy. Please check with the cluster administrator what policy are in place.

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

The python runtime introduces three tasks of kind `job`, `serve` and `build` that allows you to run a python function execution, serving a function as a service or build a docker image where the function is executed.
A `Task` is created with the `run()` method, so it's not managed directly by the user. The parameters for the task creation are passed directly to the `run()` method, and may vary depending on the kind of task.

### Task parameters

| Name | Type | Description | Default | Kind specific |
| --- | --- | --- | --- | --- |
| [action](#task-actions) | str | Task action | required | |
| [node_selector](../kubernetes-resources.md#node-selector) | list[dict] | Node selector | None | |
| [volumes](../kubernetes-resources.md#volumes) | list[dict] | List of volumes | None | |
| [resources](../kubernetes-resources.md#resources) | dict | Resources restrictions | None | |
| [affinity](../kubernetes-resources.md#affinity) | dict | Affinity | None | |
| [tolerations](../kubernetes-resources.md#tolerations) | list[dict] | Tolerations | None | |
| [envs](../kubernetes-resources.md#envs) | list[dict] | Env variables | None | |
| [secrets](../kubernetes-resources.md#secrets) | list[str] | List of secret names | None | |
| [profile](../kubernetes-resources.md#profile) | str | Profile template | None | |
| [replicas](../kubernetes-resources.md#replicas) | int | Number of replicas | None | `serve` |
| [service_type](../kubernetes-resources.md#service-type) | str | Service type | `NodePort` | `serve` |
| [instructions](#instructions) | list[str] | Build instructions to be executed as RUN instructions in Dockerfile | None | `build` |

#### Task actions

Actions must be one of the following:

- `job`
- `build`
- `serve`

##### Serving

You can run a function using `serve` action. This action deploys a service on Kubernetes.

!!! warning "Service responsiveness"
    It takes a while for the service to be ready and notified to the client.

Once the service is ready, you can use the `run.invoke()` method to call the inference server.
The `invoke` method accept [`requests.request`](https://requests.readthedocs.io/en/latest/user/quickstart/#) parameters as kwargs. The `url` parameter is by default collected from the `run` object. In case you need to override it, you can use the `url` parameter.

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
| loacal_execution | bool | Flag to indicate if the run will be executed locally | False |
| inputs | dict | Input entity key. | None |
| parameters | dict | Extra parameters for a function. | None |
| init_parameters | dict | Parameters for init function. | None |

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
