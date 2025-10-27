# ModelServe kubeai-text Serve

The `serve` action deploys text generation models via KubeAI as services on Kubernetes. A `Task` is created by calling `run()` on the Function; task parameters are passed through that call.

## Overview

The kubeai-text function kind supports deploying text generation and processing models via KubeAI. It supports various features including text generation, text embedding, and can work with different engines like Ollama, VLLM, etc.

## Quick example

```python
function = dh.new_function(
    name="my-kubeai-text-service",
    kind="kubeai-text",
    url="hf://microsoft/DialoGPT-medium",
    features=["TextGeneration"],
    engine="VLLM"
)

run = function.run(
    action="serve",
    replicas=1
)
```

## Parameters

### Function Parameters

Must be specified when creating the function.

| Name | Type | Description |
| --- | --- | --- |
| project | str | Project name. Required only when creating from the library; otherwise **MUST NOT** be set. |
| name | str | Name that identifies the object. **Required.** |
| kind | str | Function kind. Must be `kubeai-text`. **Required.** |
| uuid | str | Object ID in UUID4 format. |
| description | str | Description of the object. |
| labels | list[str] | List of labels. |
| embedded | bool | Whether the object should be embedded in the project. |
| model_name | str | Name of the model. |
| image | str | Docker image where to serve the model. |
| [url](#model-url) | str | Model url. **Required.** |
| [adapters](#adapters) | list[str] | Adapters. |
| [features](#features) | list[str] | Features. **Required.** |
| [engine](#engine) | KubeaiEngine | Engine. **Required.** |

#### Adapters

Adapters is a list of dictionaries with the following keys:

```python
adapters = [{
    "name": "adapter-name",
    "url": "adapter-url"
}]
```

#### Features

Features is a list of strings. It accepts the following values:

- `TextGeneration`
- `TextEmbedding`
- `SpeechToText`

#### Engine

The engine is a `KubeaiEngine` object that represents the engine to use for the function. The engine can be one of the following:

- `OLlama`
- `VLLM`
- `FasterWhisper`
- `Infinity`

#### Model URL

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

### Task Parameters

Can only be specified when calling `function.run()`.

#### Shared Parameters

| Name | Type | Description |
| --- | --- | --- |
| action | str | Task action. **Required. Must be `serve`** |
| [node_selector](../../../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. |
| [volumes](../../../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. |
| [resources](../../../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. |
| [affinity](../../../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. |
| [tolerations](../../../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. |
| [envs](../../../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. |
| [secrets](../../../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. |
| [profile](../../../configuration/kubernetes/overview.md#profile) | str | Profile template. |
| [replicas](../../../configuration/kubernetes/overview.md#replicas) | int | Number of replicas. |
| [service_type](../../../configuration/kubernetes/overview.md#service-port-type) | str | Service type. |
| service_name | str | Service name. |

### Run Parameters

Can only be specified when calling `function.run()`.

#### Run Function Kind-Specific Parameters

##### KubeAI Text

| Name | Type | Description |
| --- | --- | --- |
| env | dict | Environment variables. |
| args | list[str] | Arguments. |
| cache_profile | str | Cache profile. |
| [files](#files) | list[KubeaiFile] | Files. |
| [scaling](#scaling) | Scaling | Scaling parameters. |
| processors | int | Number of processors. |

#### Files

Files is a list of dict with the following keys:

```python
files = [
    {
        "path": "file-path"
        "content": "file-content"
    }
]
```

#### Scaling

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

## Entity methods

### Run methods

Once the run is created, you can access its attributes and methods through the `run` object.

::: digitalhub_runtime_modelserve.entities.run.modelserve_run.entity.RunModelserveRun.invoke
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true
