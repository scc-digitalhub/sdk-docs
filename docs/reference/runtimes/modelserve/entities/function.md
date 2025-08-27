# Function

The ModelServe runtime defines Functions of various kinds (`sklearnserve`, `mlflowserve`, `huggingfaceserve`, `kubeai-text`, `kubeai-speech`), each one representing a different ML model flavour.

## Parameters

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

## Function Kinds

The `kind` parameter must be one of the following:

- `sklearnserve`
- `mlflowserve`
- `huggingfaceserve`
- `kubeai-text`
- `kubeai-speech`

## Adapters

Adapters is a list of dictionaries with the following keys:

```python
adapters = [{
    "name": "adapter-name",
    "url": "adapter-url"
}]
```

## Features

Features is a list of strings. It accepts the following values:

- `TextGeneration`
- `TextEmbedding`
- `SpeechToText`

## Engine

The engine is a `KubeaiEngine` object that represents the engine to use for the function. The engine can be one of the following:

- `OLlama`
- `VLLM`
- `FasterWhisper`
- `Infinity`

## Model Path

The model path is the path to the model files. In **remote execution**, the path is a remote s3 path (for example: `s3://my-bucket/path-to-model`). In **local execution**, the path is a local path (for example: `./my-path` or `my-path`). According to the kind of ModelServe function, the path must follow a specific pattern:

- `sklearnserve`: `s3://my-bucket/path-to-model/model.pkl` or `./path-to-model/model.pkl`. The remote path is the partition with the model file, the local path is the model file.
- `mlflowserve`: `s3://my-bucket/path-to-model-files` or `./path-to-model-files`. The remote path is the partition with all the model files, the local path is the folder containing the MLmodel file according to MLFlow specification.

Model path is not required for `kubeai-text`, `kubeai-speech`.

## Model URL

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
