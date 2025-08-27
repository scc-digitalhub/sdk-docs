# Function Parameters

The Python runtime supports functions of kind `python`.

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
| [code_src](../../../configuration/code_src/overview.md#code-source-uri) | str | URI pointing to the source code. |
| [code](../../../configuration/code_src/overview.md#plain-text-source) | str | Source code provided as plain text. |
| base64 | str | Source code encoded as base64. |
| [handler](../../../configuration/code_src/overview.md#handler) | str | Function entrypoint. |
| [init_function](#init-function) | str | Init function name for remote (Nuclio) execution. |
| [python_version](#python-versions) | str | Python version to use. **Required.** |
| lang | str | Source code language (informational). |
| image | str | Container image used to execute the function. |
| [base_image](#base-image) | str | Base image (name:tag) used to build the execution image. |
| [requirements](#requirements) | list | List of pip requirements to install into the execution image. |

## Function Kinds

The `kind` parameter must be:

- `python`

## Python Versions

The Python runtime supports versions 3.9, 3.10, and 3.11, expressed as:

- `PYTHON3_9`
- `PYTHON3_10`
- `PYTHON3_11`

## Init Function

The init function is the entrypoint used by the Nuclio init wrapper. Specify the init function name via the `init_function` parameter.

## Base Image

The base image is the image (name:tag) used as the foundation when building the execution image for the function.

!!! warning
    Deploying jobs built from certain base images may be restricted by cluster security policies. Confirm allowed base images with your cluster administrator.

## Requirements

Requirements are a list of strings representing packages to be installed by `pip` in the image where the function will be executed.

```python
requirements = ["numpy", "pandas>1,<3", "scikit-learn==1.2.0"]
```
