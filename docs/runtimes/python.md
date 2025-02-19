# Python

The **python runtime** allows you to run generic python function.
The runtime introduces a function of kind `python` and three task of kind `job`, `serve` and `build`.

## Prerequisites

Python version and libraries:

- `python >= 3.9`
- `digitalhub-runtime-python`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-python
```

## HOW TO

With the python runtime you can use the function's `run()` method to execute a python function you have defined.
The python runtime execution workflow follows roughly these steps:

1. Define somewhere a [python function](./python.md#python-function-definition).
2. Create a `Function` object in the platform and execute the function's `run()` method.
3. The runtime collects the [inputs](./python.md#run) specified in the function as SDK objects (`Dataitem`, `Artifact`, `Model`).
4. It fetches the function [source code](../objects/code_source.md) and import the function handler.
5. It [composes](./python.md#parameters-composition) the parameters for the handler function.
6. It executes the function and map the outputs as SDK objects or as simple results.

### Python function definition

You can declare a generic python function as usual with the `def` keyword.
There are some restriction that must be applied when defining the function:

1. The argument `project` is reserved. The runtime overrides the function parameters and assign to the `project` argument a `Project` object, used as SDK context. With the `Project` object you can manipulate entities like `Artifact`, `Dataitem`, etc. If you provide a `project` argument into the function and use it as a non `Project` object, you will probably get an error. If you define the `project` argument into your functions signature, you can use the `project` variable as `Project` object.
2. The arguments `context` and `events` are reserved in remote execution. These arguments are reserved for `nuclio` `context` and `events` function parameters. If you define these arguments into your functions signature, you can use the `context` and `events` variables as `nuclio` `context` and `events` objects.
3. If some arguments of the function refer to some SDK objects, they must be mapped inside the run's `inputs` parameter. Other arguments of the function can be mapped inside the run's `parameter` parameter. More on that on the [Parameters composition](./python.md#inputs) section.
4. You may or may not decorate your function with the `@handler` decorator you can import from the `digitalhub_runtime_python` package. If you decorate your function and return something, you need to map the outputs in the decorator to collect named outputs/results. More on that on the [Parameters composition](./python.md#outputs) section.

#### Function definition example

```python
from digitalhub_runtime_python import handler

# 1. Simple function that returns a string

def func1():
   return "hello world"

# 2. Decorated function that returns a string

# If you decorate your function and return something, you need to map the outputs
# in the decorator
@handler(outputs=["result"])
def func2():
   return "hello world"


# 3. Function with project argument
def func3(project):
   # allowed use of project variable
   project.log_artifact(name="example",
                        kind="artifact",
                        source_path="/path/to/file")

   # not allowed use of project variable
   project.some_method_not_from_sdk() # Probably there will be an error


# 4. Function with context and events arguments
def func4(context, events):
   # allowed use of context and events variables in remote execution
   context.logger.info("Some log")

# 5. Function with mixed input arguments
def func5(di: Dataitem, param1: str):
   # di refers to a Dataitem object, so it must be mapped into runs inputs
   # param1 is a string, it must be mapped into runs parameters

# 6. Init function for remote execution
def init(context, param1, param2):
    # The init function is a function executed by the nuclio wrapper
    # before the execution of the user handler.
    # The context param is overridden by the runtime with the nuclio context,
    # the param1 and param2 are passed by the user at runtime into runs init_parameters
```

### Parameters composition

#### Inputs

To properly pass the parameters you need to your function, you must map them in the `function.run()` method.
Ther are some rules you need to follow:

- If you expect one of your arguments to be a `Dataitem`/`Artifact`/`Model` object, you need to explicit the reference to the object into the run's `inputs` parameter using the argument name as key and the object key as value.

```python

# Define your function and declare di argument as Dataitem
def func(di: Dataitem):
   # do something with di


# Create a dataitem
sdk_dataitem = sdk.new_dataitem(...)

# Reference the di argument as key and the dataitem key as value
sdk_function.run(inputs={"di": sdk_dataitem.key})
```

- Other function arguments must be mapped inside the run's `parameters` parameter.

```python

# Define your function and declare di argument as Dataitem
def func(di: Dataitem, param1: str):
   # do something with di


# Create a dataitem
sdk_dataitem = sdk.new_dataitem(...)

# Reference the di argument as key and the dataitem key as value
sdk_function.run(inputs={"di": sdk_dataitem.key},
                 parameters={"param1": "some value"})
```

#### Outputs

If the function return something, it is possible to collect two kinds of outputs from the `Run` object:

- SDK **`outputs`**, represented as `Dataitems` (if the rerurn value are Dataframe, eg. `pandas.DataFrame`) or `Artifacts` (if the return value are "non primitive" python object, like user defined class)
- Function **`results`** consisting of python "primitives" (str, int, float, etc.).

To collect outputs and results with named keys, you need to map them in the `handler` decorator.

```python
from digitalhub_runtime_python import handler

@handler(outputs=["data", "string"])
def func(di: Dataitem, param1: str):
   # do something with di
   return pd.DataFrame, "some value"


sdk_function.run(inputs={"di": sdk_dataitem.key},
                 parameters={"param1": "some value"})
```

In this example, the `Run` object will collect an output and a result. The output is a `Dataitem` object and the result is a `str`. To access the output from the run you can call `run.output("data")`, to collect the result you can call `run.result("string")`.

#### Serving

You can run a using `serve` action. This action deploys a service on Kubernetes.

!!! warning "Service responsiveness"
    It takes a while for the service to be ready and notified to the client. You can use the `refresh()` method and access the `status` attribute of the run object. When the service is ready, you can see a `service` attribute in the `status`.

```python
run.refresh()
run.status
```

Once the service is ready, you can use the `run.invoke()` method to call the inference server.
The `invoke` method accept [`requests.request`](https://requests.readthedocs.io/en/latest/user/quickstart/#) parameters as kwargs. The `url` parameter is by default collected from the `run` object. In case you need to override it, you can use the `url` parameter.

```python
json = {
    "some-func-param": data
}

run.invoke(method="POST", json=json)
```

### Function

The python runtime introduces a function of kind `python`.

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
| [init_function](#init-function) | str | Init function for remote nuclio execution | None |
| [python_version](#python-versions) | str | Python version to use | required |
| lang | str | Source code language (hint)| None |
| image | str | Image where the function will be executed | None |
| [base_image](#base-image) | str | Base image used to build the image where the function will be executed | None |
| [requirements](#requirements) | list | Requirements list to be installed in the image where the function will be executed | None |

##### Function kinds

The `kind` parameter must be:

- `python`

##### Python versions

The python runtime supports Python versions 3.9, 3.10 and 3.11, expressed respectively as:

- `PYTHON3_9`
- `PYTHON3_10`
- `PYTHON3_11`

##### Init function

The init function is the entrypoint of the nuclio init function. The user must pass the name of the init function in the `init_function` parameter.
The init function must be defined in the source code and should follow the [example 6](#function-definition-example).

##### Base image

The base image is a string that represents the image (name:tag) used to build the image where the function will be executed.

!!! warning
      It is possible that the platform where you deploy a job after a `build` action with a root image will not work because of security policy. Please check with the cluster administrator what policy are in place.

##### Requirements

Requirements are a list of `str` representing packages to be installed by `pip` in the image where the function will be executed.

```python
requirements = ["numpy", 'pandas>1, <3', "scikit-learn==1.2.0"]
```

#### Function example

```python
# From project ...

function = project.new_function(name="python-function",
                                kind="python",
                                code_src="main.py",
                                handler="function",
                                python_version="PYTHON3_9")

# .. or from sdk

function = dh.new_function(project="my-project",
                           name="python-function",
                           kind="python",
                           code_src="main.py",
                           handler="function",
                           python_version="PYTHON3_9")
```

### Task

The python runtime introduces three tasks of kind `job`, `serve` and `build` that allows you to run a python function execution, serving a function as a service or build a docker image where the function is executed.
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
| [replicas](kubernetes-resources.md#replicas) | int | Number of replicas | None | `serve` |
| [service_type](kubernetes-resources.md#service-type) | str | Service type | `NodePort` | `serve` |
| [instructions](#instructions) | list[str] | Build instructions to be executed as RUN instructions in Dockerfile | None | `build` |

##### Task actions

Actions must be one of the following:

- `job`
- `build`
- `serve`

##### Instructions

List of `str` representing the instructions to be executed as RUN instructions in Dockerfile.

```python
instructions = ["apt-get install -y git"]
```

#### Task example

```python
run = function.run(
    action="build",
    instructions=["apt-get install -y git"]
)
```

### Run

The `Run` object is, similar to the `Task`, created with the `run()` method.
The run's parameters are passed alongside the task's ones.

#### Run parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| loacal_execution | bool | Flag to indicate if the run will be executed locally | False |
| inputs | dict | Input entity key. | None |
| parameters | dict | Extra parameters for a function. | None |
| init_parameters | dict | Parameters for init function. | None |

#### Run example

```python
run = function.run(
    action="job",
    inputs={
        "dataitem": dataitem.key
    }
)
```

#### Run methods

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
