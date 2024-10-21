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

For installation of thw optional dependencies, you can use the following command:

```bash
python -m pip install digitalhub-runtime-modelserve[sklearn]
python -m pip install digitalhub-runtime-modelserve[mlflow]
python -m pip install digitalhub-runtime-modelserve[sklearn,mlflow]
```

## HOW TO

The modelserve runtime introduces several functions of kind `sklearnserve`, `mlflowserve`, `huggingfaceserve` that allows you to serve different ML models flavours and a task of kind `serve`.
The usage of the runtime is similar to the others:

1. Create a `Function` object of the desired model and execute it's `run()` method.
2. The runtime collects (if in remote execution), loads and exposes the model as a service.
3. With the run's `invoke()` method you can call the v2 inference API specifying the json payload you want (passed as keyword arguments).
4. You can stop the service with the run's `stop()` method.

The *modelserve* runtime launches a [mlserver](https://mlserver.readthedocs.io/en/latest/) inference server is deployed on Kubernetes as deployment and exposed as a service.

!!! warning "Service responsiveness"
    It takes a while for the service to be ready and notified to the client. You can use the `refresh()` method and access the `status` attribute of the run object. When the service is ready, you can see a `service` attribute in the `status`.

```python
run.refresh()
run.status
```

Once the service is ready, you can use the `run.invoke()` method to call the inference server.
The `invoke` method accept [`requests.request`](https://requests.readthedocs.io/en/latest/user/quickstart/#) parameters as kwargs. The `url` parameter is by default collected from the `run` object. In case you need to override it, you can use the `url` parameter.

!!! note
    In case you passed `model_name` in the function spec, and you execute the run in remote execution, you need to pass the `model_name` to the invoke method. This is because the `model_name` is used to identify the model in the inference server. `"http://{url-from-k8s}/v2/models/{model_name}/infer"`.

```python
data = [[...]] #some array
json = {
    "inputs": [
        {
        "name": "input-0",
        "shape": [x, y],
        "datatype": "FP32",
        "data": data #data-array goes here
        }
    ]
}

run.invoke(json=json)
```

### Function

There are different modelserve functions (`sklearnserve`, `mlflowserve` and `huggingfaceserve`), each one representing a different ML model flavour.

#### Function parameters

A modelserve function has the following `spec` parameters to pass to the `new_function()` method:

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only if creating from library, otherwise **MUST NOT** be set | |
| name | str | Name that identifies the object | required |
| [kind](#function-kinds) | str | Function kind | required |
| uuid | str | ID of the object in form of UUID4 | None |
| description | str | Description of the object | None |
| labels | list[str] | List of labels | None |
| embedded | bool | Flag to determine if object must be embedded in project | True |
| [path](#model-path) | str | Path to the model files | None |
| model_name | str | Name of the model | None |
| image | str | Docker image where to serve the model | None |

##### Function kinds

The `kind` parameter must be one of the following:

- `sklearnserve`
- `mlflowserve`
- `huggingfaceserve`

##### Model path

The model path is the path to the model files. In **remote execution**, the path is a remote s3 path (for example: `s3://my-bucket/path-to-model`). In **local execution**, the path is a local path (for example: `./my-path` or `my-path`). According to the kind of modelserve function, the path must follow a specific pattern:

- `sklearnserve`: `s3://my-bucket/path-to-model/model.pkl` or `./path-to-model/model.pkl`. The remote path is the partition with the model file, the local path is the model file.
- `mlflowserve`: `s3://my-bucket/path-to-model-files` or `./path-to-model-files`. The remote path is the partition with all the model files, the local path is the folder containing the MLmodel file according to MLFlow specification.

#### Function example

```python
# Example remote model mlflow

function = project.new_function(name="mlflow-serve-function",
                                kind="mlflowserve",
                                path=model.spec.path + "model")

# Example local model mlflow

function = project.new_function(name="mlflow-serve-function",
                                kind="mlflowserve",
                                path="./my-path/model")

# Example remote model sklearn

function = project.new_function(name="sklearn-serve-function",
                                kind="sklearnserve",
                                path=model.spec.path)

# Example local model sklearn

function = project.new_function(name="sklearn-serve-function",
                                kind="sklearnserve",
                                path="./my-path/model.pkl")
```

### Task

The modelserve runtime introduces one tasks of kind `serve` that allows you to deploy ML models on Kubernetes or locally.
A `Task` is created with the `run()` method, so it's not managed directly by the user. The parameters for the task creation are passed directly to the `run()` method, and may vary depending on the kind of task.

#### Task parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| [action](#task-actions) | str | Task action | required |
| [node_selector](kubernetes-resources.md#node-selector) | list[dict] | Node selector | None |
| [volumes](kubernetes-resources.md#volumes) | list[dict] | List of volumes | None |
| [resources](kubernetes-resources.md#resources) | dict | Resources restrictions | None |
| [affinity](kubernetes-resources.md#affinity) | dict | Affinity | None |
| [tolerations](kubernetes-resources.md#tolerations) | list[dict] | Tolerations | None |
| [envs](kubernetes-resources.md#envs) | list[dict] | Env variables | None |
| [secrets](kubernetes-resources.md#secrets) | list[str] | List of secret names | None |
| [profile](kubernetes-resources.md#profile) | str | Profile template | None |
| [replicas](kubernetes-resources.md#replicas) | int | Number of replicas | None |
| [service_type](kubernetes-resources.md#service-type) | str | Service type | `NodePort` |

##### Task actions

Actions must be one of the following:

- `serve`: to deploy a service

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

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| local_execution | bool | Flag to determine if the run must be executed locally | False |

#### Run example

```python
run = function.run(
    action="serve",
    local_execution=True,
)
```

#### Run methods

Once the run is created, you can access some of its attributes and methods through the `run` object.

::: digitalhub_runtime_modelserve.entities.run.modelserve_run.entity.RunModelserveRun.invoke
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true
