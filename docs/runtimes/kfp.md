# KFP Pipelines Runtime

The **kfp runtime** allows you to run workflows within the platform.
The runtime introduces a function of kind `kfp` and a task of kind `pipeline`.

## Prerequisites

Python version and libraries:

- `python >= 3.9`
- `digitalhub-runtime-kfp`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-kfp
```

## HOW TO

With the kfp runtime you can use the function's `run()` method to execute a workflow you have defined.
The kfp runtime execution workflow follows roughly these steps:

1. Define one or more functions to be executed. These functions can be from other runtimes.
2. Define somewhere a [pipeline](./kfp.md#pipeline-definition).
3. The workflow's `run()` method calls a stepper that create various KFP ContainerOP and executes them.

### Pipeline definition

To define a pipeline you need to define function with the `def` keyword. You can give the function a name and declare its arguments as usual.
From `digitalhub_runtime_kfp.dsl` you must import `pipeline_context`. Its a context manager object that allows you to order the various steps of execution and chain them together with inputs and outputs. Once you write the pipeline function, store it in a file .py.
When you define the steps inside the pipeline, you specify also inputs, outputs, parameters and values for the steps.

#### Step parameters

| Parameter | Type | Example | Description |
| --- | --- | --- | --- |
| name | str | "download" | Name of the step |
| function | str | "downloader-funct" | Name of the dh function to execute. It must exists in the dh project context |
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

      # Defaine first step
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

The kfp runtime introduces a function of kind `kfp`.

#### Workflow parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only if creating from library, otherwise **MUST NOT** be set | |
| name | str | Name that identifies the object | required |
| [kind](#workflow-kinds) | str | Workflow kind | required |
| uuid | str | ID of the object in form of UUID4 | None |
| description | str | Description of the object | None |
| labels | list[str] | List of labels | None |
| embedded | bool | Flag to determine if object must be embedded in project | True |
| [code_src](../objects/code_source.md#code-source-uri) | str | URI pointer to source code | None |
| [code](../objects/code_source.md#plain-text-source) | str | Source code (plain text)| None |
| [base64](../objects/code_source.md#base64-encoded-source) | str | Source code (base64 encoded)| None |
| [handler](../objects/code_source.md#handler) | str | Function entrypoint | None |
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
| [node_selector](./kubernetes-resources.md#node-selector) | list[dict] | Node selector | None | |
| [volumes](./kubernetes-resources.md#volumes) | list[dict] | List of volumes | None | |
| [resources](./kubernetes-resources.md#resources) | dict | Resources restrictions | None | |
| [affinity](./kubernetes-resources.md#affinity) | dict | Affinity | None | |
| [tolerations](./kubernetes-resources.md#tolerations) | list[dict] | Tolerations | None | |
| [envs](./kubernetes-resources.md#envs) | list[dict] | Env variables | None | |
| [secrets](./kubernetes-resources.md#secrets) | list[str] | List of secret names | None | |
| [profile](./kubernetes-resources.md#profile) | str | Profile template | None | |
| [schedule](./kubernetes-resources.md#schedule) | str | Schedule for the job | None | |

##### Task actions

Actions must be one of the following:

- `pipeline`

#### Task example

```python
run = function.run(action="pipeline")
```

### Run

The `Run` object is, similar to the `Task`, created with the `run()` method.
The run's parameters are passed alongside the task's ones.

#### Run parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| inputs | dict | Inputs for the pipeline function. | None |

#### Run example

```python
run = function.run(
    action="job",
    inputs={"dataitem": dataitem.key}
)
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
