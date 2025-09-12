# ModelServe sklearnserve Serve

The `serve` action deploys scikit-learn ML models as services on Kubernetes. A `Task` is created by calling `run()` on the Function; task parameters are passed through that call.

## Overview

The sklearnserve function kind supports deploying scikit-learn models as REST API services. The model must be saved in pickle format (.pkl).

## Quick example

```python
function = dh.new_function(
    name="my-sklearn-service",
    kind="sklearnserve",
    path="s3://my-bucket/path-to-model/model.pkl"
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
| kind | str | Function kind. Must be `sklearnserve`. **Required.** |
| uuid | str | Object ID in UUID4 format. |
| description | str | Description of the object. |
| labels | list[str] | List of labels. |
| embedded | bool | Whether the object should be embedded in the project. |
| [path](#model-path) | str | Path to the model files. **Required.** |
| model_name | str | Name of the model. |
| [image](#model-image) | str | Docker image where to serve the model. |

#### Model Path

The model path must follow the pattern:

```python
path_regex = r"^(store://([^/]+)/model/sklearn/.*)|.*\\.pkl$|.*\\.joblib$"
```

#### Model Image

Model image must follow the pattern:

```python
image_regex = r"^seldonio\\/mlserver?:.*-sklearn$"
```

### Task Parameters

Can only be specified when calling `function.run()`.

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

No specific parameters for run of this action.

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
