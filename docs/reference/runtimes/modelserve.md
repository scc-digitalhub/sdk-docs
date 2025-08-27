
# ModelServe runtime

The ModelServe runtime allows you to deploy ML models on Kubernetes or locally.

## Prerequisites

Supported Python version and required package:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-modelserve`

Install from PyPI:

```bash
python -m pip install digitalhub-runtime-modelserve
```

## Overview

The ModelServe runtime provides several serve functions (`sklearnserve`, `mlflowserve`, `huggingfaceserve`, `kubeai-text`, `kubeai-speech`) and a `serve` task action. Typical usage:

1. Create a Function for the model and call its `run()` method.
2. The runtime collects, loads and exposes the model as a service.
3. Call the run's `invoke()` method to send inference requests (the method accepts the same keyword arguments as `requests.request`).
4. Stop the service with `run.stop()` when finished.

The ModelServe runtime deploys an [mlserver](https://mlserver.readthedocs.io/en/latest/) inference server on Kubernetes (Deployment + Service).

!!! warning "Service responsiveness"
    It may take some time for the service to become ready. Use `run.refresh()` and inspect `run.status`. When ready, the `status` will include a `service` attribute.

```python
run.refresh()
run.status
```

After the service is ready, call the inference endpoint with `run.invoke()`. By default the `url` is taken from the `run` object; override it with an explicit `url` parameter if needed.

!!! note
    If you set `model_name` in the function spec and run remotely, pass `model_name` to `invoke()` so the runtime can target the model with the MLServer V2 endpoint ("http://{url-from-k8s}/v2/models/{model_name}/infer").

```python
data = [[...]]  # some array
json = {
    "inputs": [
        {
            "name": "input-0",
            "shape": [x, y],
            "datatype": "FP32",
            "data": data
        }
    ]
}

run.invoke(json=json)
```

### Function

The ModelServe runtime defines Functions of various kinds (`sklearnserve`, `mlflowserve`, `huggingfaceserve`, `kubeai-text`, `kubeai-speech`), each one representing a different ML model flavour.

#### Function parameters

| Name | Type | Description |
| --- | --- | --- |
| project | str | Project name. Required only when creating from the library; otherwise **MUST NOT** be set. |
| name | str | Name that identifies the object. **Required.** |
| [kind](#function-kinds) | str | Function kind. **Required.** |
| uuid | str | Object ID in UUID4 format. |
| description | str | Description of the object. |
| labels | list[str] | List of labels. |
| embedded | bool | Whether the object should be embedded in the project. |
| [path](#model-path) | str | Path to the model files. |
| model_name | str | Name of the model. |
| image | str | Docker image where to serve the model. |
| [url](#model-url) | str | Model url. (For `kubeai-text`, `kubeai-speech`) |
| [adapters](#adapters) | list[str] | Adapters. (For `kubeai-text`, `kubeai-speech`) |
| [features](#features) | list[str] | Features. (For `kubeai-text`) |
| [engine](#engine) | KubeaiEngine | Engine. (For `kubeai-text`) |

##### Function kinds

The `kind` parameter must be one of the following:

- `sklearnserve`
- `mlflowserve`
- `huggingfaceserve`
- `kubeai-text`
- `kubeai-speech`

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

The model path is the path to the model files. In **remote execution**, the path is a remote s3 path (for example: `s3://my-bucket/path-to-model`). In **local execution**, the path is a local path (for example: `./my-path` or `my-path`). According to the kind of ModelServe function, the path must follow a specific pattern:

- `sklearnserve`: `s3://my-bucket/path-to-model/model.pkl` or `./path-to-model/model.pkl`. The remote path is the partition with the model file, the local path is the model file.
- `mlflowserve`: `s3://my-bucket/path-to-model-files` or `./path-to-model-files`. The remote path is the partition with all the model files, the local path is the folder containing the MLmodel file according to MLFlow specification.

Model path is not required for `kubeai-text`, `kubeai-speech`.

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

### Task

The ModelServe runtime supports a `serve` task action to deploy ML models on Kubernetes or locally. A `Task` is created by calling `run()` on the Function; task parameters are passed through that call and may vary by action.

#### Task parameters (shared)

| Name | Type | Description |
| --- | --- | --- |
| [action](#task-actions) | str | Task action. One of: `serve`. **Required.** |
| [node_selector](../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. |
| [volumes](../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. |
| [resources](../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. |
| [affinity](../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. |
| [tolerations](../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. |
| [envs](../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. |
| [secrets](../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. |
| [profile](../configuration/kubernetes/overview.md#profile) | str | Profile template. |
| [replicas](../configuration/kubernetes/overview.md#replicas) | int | Number of replicas. |
| [service_type](../configuration/kubernetes/overview.md#service-port-type) | str | Service type. |

#### Function kind-specific task parameters

- `huggingfaceserve`

| Name | Type | Description |
| --- | --- | --- |
| [huggingface_task](#huggingface-task) | str | Huggingface task type. |
| [backend](#backend) | str | Backend type. |
| tokenizer_revision | str | Tokenizer revision. |
| max_length | int | Huggingface max sequence length for the tokenizer. |
| disable_lower_case | bool | Do not use lower case for the tokenizer. |
| disable_special_tokens | bool | The sequences will not be encoded with the special tokens relative to their model. |
| [dtype](#dtype) | str | Data type to load the weights in. |
| trust_remote_code | bool | Allow loading of models and tokenizers with custom code. |
| tensor_input_names | list[str] | The tensor input names passed to the model. |
| return_token_type_ids | bool | Return token type ids. |
| return_probabilities | bool | Return all probabilities. |
| disable_log_requests | bool | Disable log requests. |
| max_log_len | int | Max number of prompt characters or prompt. |

##### Task actions

Supported actions:

- `serve` — deploy a service

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

### Run

The `Run` object is created by calling `run()` on a Function. Run-level parameters are provided alongside task parameters.

#### Run parameters (shared)

No shared specific parameters for run of this runtime.

#### Function kind-specific run parameters

- `kubeai-text`, `kubeai-speech`

| Name | Type | Description |
| --- | --- | --- |
| env | dict | Environment variables. |
| args | list[str] | Arguments. |
| cache_profile | str | Cache profile. |
| [files](#files) | list[KubeaiFile] | Files. |
| [scaling](#scaling) | Scaling | Scaling parameters. |
| processors | int | Number of processors. |

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

##### Scaling

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

#### Run methods

::: digitalhub_runtime_modelserve.entities.run.modelserve_run.entity.RunModelserveRun.invoke
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

## Examples

```python
import digitalhub as dh

project = dh.get_or_create_project("my_project")

# Example model mlflow
function = project.new_function(name="mlflow-serve-function",
                                kind="mlflowserve",
                                path=model.spec.path + "model")

# Example model sklearn
function = project.new_function(name="sklearn-serve-function",
                                kind="sklearnserve",
                                path=model.spec.path)

# Example KubeAI text model
function = project.new_function(
    name="kubeai-text-function",
    kind="kubeai-text",
    url="hf://mistralai/Mistral-7B-v0.1",
    features=["TextGeneration"],
    engine="VLLM"
)

# Example KubeAI speech model
function = project.new_function(
    name="kubeai-speech-function",
    kind="kubeai-speech",
    url="hf://openai/whisper-large-v3",
    features=["SpeechToText"],
    engine="FasterWhisper"
)

run = function.run(action="serve")
```
