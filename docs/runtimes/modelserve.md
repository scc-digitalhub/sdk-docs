# Modelserve runtime

The **Modelserve runtime** allows you to deploy ML models on Kubernetes or locally.

## Prerequisites

Python version and libraries:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-modelserve`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-modelserve
```

## HOW TO

The modelserve runtime introduces several functions of kind `sklearnserve`, `mlflowserve`, `huggingfaceserve` and `kubeaiserve` that allows you to serve different ML models flavours and a task of kind `serve`.
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

There are different modelserve functions (`sklearnserve`, `mlflowserve`, `huggingfaceserve` and `kubeaiserve`), each one representing a different ML model flavour.

#### Function parameters

A modelserve function has the following `spec` parameters to pass to the `new_function()` method:

| Name | Type | Description | Default | Runtime |
| --- | --- | --- | --- | --- |
| project | str | Project name. Required only if creating from library, otherwise **MUST NOT** be set | | |
| name | str | Name that identifies the object | required | |
| [kind](#function-kinds) | str | Function kind | required | |
| uuid | str | ID of the object in form of UUID4 | None | |
| description | str | Description of the object | None | |
| labels | list[str] | List of labels | None | |
| embedded | bool | Flag to determine if object must be embedded in project | True | |
| [path](#model-path) | str | Path to the model files | None | |
| model_name | str | Name of the model | None | |
| image | str | Docker image where to serve the model | None | |
| [url](#model-url) | str | Model url | None | `kubeaiserve` |
| [adapters](#adapters) | list[str] | Adapters | None | `kubeaiserve` |
| [features](#features) | list[str] | Features | None | `kubeaiserve` |
| [engine](#engine) | KubeaiEngine | Engine | None | `kubeaiserve` |
| processors | int | Number of processors | None | `kubeaiserve` |

##### Function kinds

The `kind` parameter must be one of the following:

- `sklearnserve`
- `mlflowserve`
- `huggingfaceserve`
- `kubeaiserve`

##### Adapters

Adapters is a list of dictionaries with the following keys:

```python
adapters = [{
    "name": "adapter-name",
    "url": "adapter-url"
}]
```

##### Features

Features is a list of strings. It accepts the following values:

- `TextGeneration`
- `TextEmbedding`
- `SpeechToText`

##### Engine

The engine is a `KubeaiEngine` object that represents the engine to use for the function. The engine can be one of the following:

- `OLlama`
- `VLLM`
- `FasterWhisper`
- `Infinity`

##### Model path

The model path is the path to the model files. In **remote execution**, the path is a remote s3 path (for example: `s3://my-bucket/path-to-model`). In **local execution**, the path is a local path (for example: `./my-path` or `my-path`). According to the kind of modelserve function, the path must follow a specific pattern:

- `sklearnserve`: `s3://my-bucket/path-to-model/model.pkl` or `./path-to-model/model.pkl`. The remote path is the partition with the model file, the local path is the model file.
- `mlflowserve`: `s3://my-bucket/path-to-model-files` or `./path-to-model-files`. The remote path is the partition with all the model files, the local path is the folder containing the MLmodel file according to MLFlow specification.

Model path is not required for `kubeaiserve`.

##### Model url

The model url must follow the pattern:

```python
regexp = (
    r"^(store://([^/]+)/model/huggingface/.*)"
    + r"|"
    + r"^pvc?://.*$"
    + r"|"
    + r"^s3?://.*$"
    + r"|"
    + r"^ollama?://.*$"
    + r"|"
    + r"^hf?://.*$"
)
```

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

| Name | Type | Description | Default | Runtime |
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
| [replicas](kubernetes-resources.md#replicas) | int | Number of replicas | None | |
| [service_type](kubernetes-resources.md#service-type) | str | Service type | `NodePort` | |
| [huggingface_task](#huggingface-task) | str | Huggingface task type | None | `huggingfaceserve` |
| [backend](#backend) | str | Backend type | None | `huggingfaceserve` |
| tokenizer_revision | str | Tokenizer revision | None | `huggingfaceserve` |
| max_length | int | Huggingface max sequence length for the tokenizer | None | `huggingfaceserve` |
| disable_lower_case | bool | Do not use lower case for the tokenizer | None | `huggingfaceserve` |
| disable_special_tokens | bool | The sequences will not be encoded with the special tokens relative to their model | None | `huggingfaceserve` |
| [dtype](#dtype) | str | Data type to load the weights in | None | `huggingfaceserve` |
| trust_remote_code | bool | Allow loading of models and tokenizers with custom code | None | `huggingfaceserve` |
| tensor_input_names | list[str] | The tensor input names passed to the model | None | `huggingfaceserve` |
| return_token_type_ids | bool | Return token type ids | None | `huggingfaceserve` |
| return_probabilities | bool | Return all probabilities | None | `huggingfaceserve` |
| disable_log_requests | bool | Disable log requests | None | `huggingfaceserve` |
| max_log_len | int | Max number of prompt characters or prompt | None | `huggingfaceserve` |

##### Task actions

Actions must be one of the following:

- `serve`: to deploy a service

##### Huggingface task

You can specify the task type for the Huggingface model. The task type must be one of the following:

- `sequence_classification`
- `token_classification`
- `fill_mask`
- `text_generation`
- `text2text_generation`
- `text_embedding`

##### Backend

You can specify the backend type for the Huggingface model. The backend type must be one of the following:

- `AUTO`
- `VLLM`
- `HUGGINGFACE`

##### Dtype

You can specify the data type to load the weights in. The data type must be one of the following:

- `AUTO`
- `FLOAT32`
- `FLOAT16`
- `BFLOAT16`
- `FLOAT`
- `HALF`

#### Task example

```python
run = function.run(action="serve")
```

### Run

The `Run` object is, similar to the `Task`, created with the `run()` method.
The run's parameters are passed alongside the task's ones.

#### Run parameters

| Name | Type | Description | Default | Runtime |
| --- | --- | --- | --- | --- |
| local_execution | bool | Flag to determine if the run must be executed locally | False | |
| env | dict | Environment variables | None | `kubeaiserve` |
| args | list[str] | Arguments | None | `kubeaiserve` |
| cache_profile | str | Cache profile | None | `kubeaiserve` |
| [files](#files) | list[KubeaiFile] | Files | None | `kubeaiserve` |
| [scaling](#scaling) | Scaling | Scaling parameters | None | `kubeaiserve` |

##### Files

Files is a list of dict with the following keys:

```python
files = [
    {
        "path": "file-path"
        "content": "file-content"
    }
]
```

###### Scaling

Scaling is a `Scaling` object that represents the scaling parameters for the run. Its structure is as follows:

```python
scaling = {
    "replicas": int,
    "min_replicas": int,
    "max_replicas": int,
    "autoscaling_disabled": bool,
    "target_request": int,
    "scale_down_delay_seconds": int,
    "load_balancing": {
        "strategy": str,  # "LeastLoad" or "PrefixHash"
        "prefix_hash": {
            "mean_load_factor": int,
            "replication": int,
            "prefix_char_length": int
        }
    }
}
```

#### Run example

```python
run = function.run(action="serve")
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
