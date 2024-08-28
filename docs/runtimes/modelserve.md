# Modelserve runtime

The **Modelserve runtime** allows you to deploy ML models on Kubernetes or locally.

## Prerequisites

Python version and libraries:

- `python >= 3.9`
- `digitalhub-runtime-modelserve`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-modelserve
```

The package comes with optional dependencies:

- `sklearn`
- `mlflow`
- `huggingface`

For installation of thw optional dependencies, you can use the following command:

```bash
python -m pip install digitalhub-runtime-modelserve[sklearn]
python -m pip install digitalhub-runtime-modelserve[mlflow]
python -m pip install digitalhub-runtime-modelserve[huggingface]
python -m pip install digitalhub-runtime-modelserve[sklearn,mlflow,huggingface]
```

## HOW TO

The modelserve runtime introduces several functions of kind `sklearnserve`, `mlflowserve`, `huggingfaceserve` that allows you to serve different ML models flavours and a task of kind `serve`.
The usage of the runtime is similar to the others:

1. Create a `Function` object of the desired model and execute the it's `run()` method.
2. The runtime collects, loads and exposes the model.
3. With the run's `invoke()` method you can call the model with the parameters you want (passed as keyword arguments).

### Function

There are three modelserve functions: `sklearnserve`, `mlflowserve` and `huggingfaceserve`.

#### Function parameters

A modelserve function has the following `spec` parameters to pass to the `new_function()` method:

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name | required (if creating from library) |
| name | str | Name that identifies the object | required |
| kind | str | Kind of the object. Must be one of: <li>`sklearnserve`</li><li>`mlflowserve`</li><li>`huggingfaceserve`</li>  | required |
| uuid | str | ID of the object in form of UUID | None |
| description | str | Description of the object | None |
| labels | list[str] | List of labels | None |
| embedded | bool | Flag to determine if object must be embedded in project | True |
| path | str | Path to the model files | None |
| model_name | str | Name of the model | None |
| image | str | Docker image where to serve the model | None |

#### Function example

```python
# From project ...

function = project.new_function(name="mlflow-serve-function",
                                kind="mlflowserve",
                                )

# .. or from sdk

function = dh.new_function(project="my-project",
                           name="mlflow-serve-function",
                           kind="mlflowserve",
                           )
```

### Task

The python runtime introduces three tasks of kind `job`, `serve` and `build` that allows you to run a python function execution, serving a function as a service or build a docker image where the function is executed.
A `Task` is created with the `run()` method, so it's not managed directly by the user. The parameters for the task creation are passed directly to the `run()` method, and may vary depending on the kind of task.

#### Task parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| action | str | Task action. Must be: `serve` | required |
| [node_selector](kubernetes-resources.md#node_selector) | list[dict] | Node selector | None |
| [volumes](kubernetes-resources.md#volumes) | list[dict] | List of volumes | None |
| [resources](kubernetes-resources.md#resources) | dict | Resources restrictions | None |
| [affinity](kubernetes-resources.md#affinity) | dict | Affinity | None |
| [tolerations](kubernetes-resources.md#tolerations) | list[dict] | Tolerations | None |
| [envs](kubernetes-resources.md#envs) | list[dict] | Env variables | None |
| [secrets](kubernetes-resources.md#secrets) | list[str] | List of secret names | None |
| [profile](kubernetes-resources.md#profile) | str | Profile template | None |
| replicas | int | Number of replicas | None |
| service_type| str | Service type. Must be one of: <li>`ClusterIP`</li><li>`LoadBalancer`</li><li>`NodePort`</li> | `NodePort` |

#### Task example

```python
run = function.run(
    action="serve",
    backoff_limit=1,
)
```

### Run

The `Run` object is, similar to the `Task`, created with the `run()` method.
The run's parameters are passed alongside the task's ones.

#### Run parameters

There are no parameters for the `run` spec.

#### Run example

```python
run = function.run(
    action="serve"
)
```
