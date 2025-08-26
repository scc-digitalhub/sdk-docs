# KFP Pipelines Runtime

The **KFP runtime** allows you to run workflows within the platform.
The runtime introduces a workflow of kind `kfp` and a task of kind `pipeline`.

## Prerequisites

Python version and libraries:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-kfp`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-kfp
```

## Overview

Use a workflow's `run()` method to build and execute Kubeflow Pipelines. The typical flow is:

1. Define the functions to be executed (these may belong to other runtimes).
2. Implement a pipeline function (see [pipeline definition](#pipeline-definition)).
3. Build the pipeline with `run(action="build")` (required).
4. Execute the pipeline with `run(action="pipeline")`; the runtime will step through the KFP ContainerOps.

### Pipeline definition

To define a pipeline you need to define a function with the `def` keyword. You can give the function a name and declare its arguments as usual. From `digitalhub_runtime_kfp.dsl` import `pipeline_context`. It's a context-manager object that lets you order the steps of execution and chain inputs and outputs. Once you write the pipeline function, store it in a `.py` file.
When defining the steps inside the pipeline, also specify inputs, outputs, parameters and values for the steps.

#### Step parameters

| Parameter | Type | Example | Description |
| --- | --- | --- | --- |
| name | str | "download" | Name of the step |
| function | str | "downloader-funct" | Name of the dh function to execute. It must exist in the dh project context |
| action | str | "job" | Action to execute |
| inputs | dict | {"url": "dataitem_key", "dataset": previous_step.outputs["some_key"]} | Input dh parameters keys (dataitems, artifacts, models). The syntax for the inputs is the same as in the `kfp` package when it comes to link an output step to an input. |
| outputs | dict | {"dataset": "dataset"} | Dh outputs mapped |
| parameters | dict | {"param": "value"} | Function generic parameters |
| values | list | ["val1", "val2"] | List of non dh outputs referenced as strings |

#### Workflow definition example

```python
from digitalhub_runtime_kfp.dsl import pipeline_context

def myhandler(url):
   # Use pipeline_context() manager
   with pipeline_context() as pc:

    # Define first step
      step1 = pc.step(name="download",                         # Name of the step 1
                      function="downloader-funct",              # Name of the dh function to execute
                      action="job",                             # Action to execute
                      inputs={"url": url},                      # Input parameters
                      outputs={"dataset": "dataset"})           # Mapped outputs

      step2 = pc.step(name="extract_parking",                  # Name of the step 2
                      function="extract-parkings",              # Name of the dh function to execute
                      action="job",                             # Action to execute
                      inputs={"di": step1.outputs['dataset']},  # Input parameters from previous step
                      outputs={"parkings": "parkings"})         # Mapped outputs

```

### Workflow

The KFP runtime introduces a function of kind `kfp`.

#### Workflow parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only if creating from library, otherwise **MUST NOT** be set | |
| name | str | Name that identifies the object | required |
| [kind](#workflow-kinds) | str | Workflow kind | required |
| uuid | str | ID of the object in form of UUID4 | None |
| description | str | Description of the object | None |
| labels | list[str] | List of labels | None |
| embedded | bool | Whether the object should be embedded in the project. | True |
| [code_src](../configuration/code_src/overview.md#code-source-uri) | str | URI pointer to source code | None |
| [code](../configuration/code_src/overview.md#plain-text-source) | str | Source code (plain text)| None |
| base64 | str | Source code (base64 encoded)| None |
| [handler](../configuration/code_src/overview.md#handler) | str | Function entrypoint | None |
| lang | str | Source code language (hint)| None |
| image | str | Image where the workflow will be executed | None |
| tag | str | Tag of the image where the workflow will be executed | None |

##### Workflow kinds

The `kind` parameter must be one of the following:

- `kfp`

#### Workflow example

```python
# From project ...

workflow = project.new_workflow(name="workflow",
                                kind="kfp",
                                code_src="pipeline.py",
                                handler="handler")

# .. or from sdk

workflow = dh.new_workflow(project="my-project",
                           name="workflow",
                           kind="kfp",
                           code_src="pipeline.py",
                           handler="handler")
```

### Task

The KFP runtime introduces a task of kind `pipeline` that allows you to run a workflow.
A `Task` is created with the `run()` method, so it's not managed directly by the user. The parameters for the task creation are passed directly to the `run()` method, and may vary depending on the kind of task.

#### Task parameters

| Name | Type | Description | Default | Kind specific |
| --- | --- | --- | --- | --- |
| [action](#task-actions) | str | Task action | required | |
| [node_selector](./../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector | None | |
| [volumes](./../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes | None | |
| [resources](./../configuration/kubernetes/overview.md#resources) | dict | Resources restrictions | None | |
| [affinity](./../configuration/kubernetes/overview.md#affinity) | dict | Affinity | None | |
| [tolerations](./../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations | None | |
| [envs](./../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Env variables | None | |
| [secrets](./../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names | None | |
| [profile](./../configuration/kubernetes/overview.md#profile) | str | Profile template | None | |

##### Task actions

Actions must be one of the following:

- `build`
- `pipeline`

#### Task example

```python
run_build = workflow.run(action="build")

run_pipeline = workflow.run(action="pipeline")
```

### Run

The `Run` object is, similar to the `Task`, created with the `run()` method.
The run's parameters are passed alongside the task's ones.

#### Run parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| Parameters | dict | Inputs for the pipeline function. | None |

#### Run example

```python
run_build = workflow.run(action="build")

run = workflow.run(action="pipeline", parameters={"dataitem": dataitem.key})
```

#### Run methods

::: digitalhub_runtime_kfp.entities.run.kfp_run.entity.RunKfpRun.output
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_kfp.entities.run.kfp_run.entity.RunKfpRun.outputs
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_kfp.entities.run.kfp_run.entity.RunKfpRun.result
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_kfp.entities.run.kfp_run.entity.RunKfpRun.results
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_kfp.entities.run.kfp_run.entity.RunKfpRun.values
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true
