# Hera Pipelines Runtime

The Hera runtime enables running Hera workflows on the platform. It defines Workflow objects of kind `hera` and supports a `pipeline` task action.

## Prerequisites

Supported Python version and required package:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-hera`

Install from PyPI:

```bash
python -m pip install digitalhub-runtime-hera
```

## Overview

Use a Workflow's `run()` method to build and execute Hera pipelines. Typical workflow:

1. Define the functions (steps) to be executed; these may belong to other runtimes.
2. Implement a pipeline function that returns a Hera `Workflow` object (see Pipeline definition).
3. Build the pipeline by calling `run(action="build")` (required).
4. Execute the pipeline with `run(action="pipeline")`; a stepper will execute the Hera steps.

### Pipeline definition

Define a pipeline by creating a Python function that returns a Hera `Workflow` object. Any arguments in the function signature are passed as `parameters` during `run(action="build")`.

The runtime provides DSL helpers in `digitalhub_runtime_hera.dsl`. Use `step` and `container_op` to wrap digitalhub functions and workflows into Hera steps and container templates. The DSL supports both `DAG` and `Steps` contexts.

Core components:

- `step`: defines an individual workflow step inside a `DAG` or `Steps` context; it represents a task and can declare inputs, outputs and parameters.
- `container_op`: constructs a Hera container template (image, command, args). It is used by `step` and also available for advanced custom templates.

Typical pipeline function structure:

1. Create a `Workflow` object and set the entrypoint (usually a DAG).
2. Use a `DAG` or `Steps` context to define the workflow structure.
3. Add steps via `step(...)`, providing templates, function names, inputs/outputs and parameters.
4. Chain steps using Hera operators to define dependencies.
5. Return the `Workflow` object.

#### `step` function

`step(**step_kwargs)` creates a workflow step (a Hera Task) inside a DAG or Steps context. Main arguments:

| Parameter   | Type      | Example    | Description |
|---|---:|---|---|
| template    | dict | {"action": "job"} | Parameters template to pass to `function.run()` or `workflow.run()`. The `action` key is always required. To pass inputs from other steps use the `{{inputs.parameters.parameter_name}}` template syntax. |
| function    | str  | "download-data" | Name of the digitalhub function to execute. |
| function_id | str  | "abc123"        | Function ID (optional). |
| workflow    | str  | "my-workflow"   | Workflow name (optional). |
| workflow_id | str  | "def456"        | Workflow ID (optional). |
| name        | str  | "step1"         | Step name. |
| inputs      | dict | {"some-input": ANOTHER_STEP.get_parameter("some-output")} | Step inputs. Keys become Hera Parameters; values can reference other steps' outputs. |
| outputs     | list | ["output1"]     | Step outputs. These become Hera Outputs and Artifacts. |

Other keyword arguments are forwarded to the underlying container template. `step` must be called inside a `DAG` or `Steps` context.

#### `container_op` function

`container_op(...)` builds a Hera container template for a workflow step. It returns a Hera `Container` object and accepts similar arguments to `step` (template, function, name, inputs, outputs, ...). Use it directly for advanced scenarios or custom templates.

#### Workflow definition example

```python
from hera.workflows import Workflow, DAG, Parameter
from digitalhub_runtime_hera.dsl import step


def pipeline():

    # Create a new Workflow with an entrypoint DAG and a parameter
    with Workflow(entrypoint="dag", arguments=Parameter(name="url")) as w:
        with DAG(name="dag"):

            # First step: takes the workflow parameter and outputs a dataset
            A = step(template={"action":"job", "inputs": {"url": "{{workflow.parameters.url}}"}},
                     function="download-data",
                     outputs=["dataset"])

            # Subsequent steps consume A's output
            B = step(template={"action":"job", "inputs": {"di": "{{inputs.parameters.di}}"}},
                     function="process-spire",
                     inputs={"di": A.get_parameter("dataset")})

            C = step(template={"action":"job", "inputs": {"di": "{{inputs.parameters.di}}"}},
                     function="process-measures",
                     inputs={"di": A.get_parameter("dataset")})

            # Chain the steps
            A >> [B, C]

    return w

```

### Workflow

The Hera runtime defines Workflow objects of kind `hera`.

#### Workflow parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only when creating from the library; otherwise **MUST NOT** be set. | |
| name | str | Name that identifies the object. | required |
| [kind](#workflow-kinds) | str | Workflow kind. | required |
| uuid | str | Object ID in UUID4 format. | None |
| description | str | Description of the object. | None |
| labels | list[str] | List of labels. | None |
| embedded | bool | Whether the object should be embedded in the project. | True |
| [code_src](../configuration/code_src/overview.md#code-source-uri) | str | URI pointing to the source code. | None |
| [code](../configuration/code_src/overview.md#plain-text-source) | str | Source code provided as plain text. | None |
| base64 | str | Source code encoded as base64. | None |
| [handler](../configuration/code_src/overview.md#handler) | str | Function entrypoint. | None |
| lang | str | Source code language (informational). | None |

##### Workflow kinds

The `kind` parameter must be:

- `hera`

#### Workflow example

```python
# From project ...

workflow = project.new_workflow(name="workflow",
                                kind="hera",
                                code_src="pipeline.py",
                                handler="handler")

# .. or from sdk

workflow = dh.new_workflow(project="my-project",
                           name="workflow",
                           kind="hera",
                           code_src="pipeline.py",
                           handler="handler")
```

### Task

The Hera runtime supports a `pipeline` task action to run workflows. A `Task` is created by calling `run()` on the Workflow; task parameters are passed through that call and may vary by action.

#### Task parameters

| Name | Type | Description | Default | Kind specific |
| --- | --- | --- | --- | --- |
| [action](#task-actions) | str | Task action. | required | |
| [node_selector](./../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. | None | |
| [volumes](./../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. | None | |
| [resources](./../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. | None | |
| [affinity](./../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. | None | |
| [tolerations](./../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. | None | |
| [envs](./../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. | None | |
| [secrets](./../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. | None | |
| [profile](./../configuration/kubernetes/overview.md#profile) | str | Profile template. | None | |

##### Task actions

Supported actions:

- `build`
- `pipeline`

#### Task example

```python
run_build = workflow.run(action="build")

run_pipeline = workflow.run(action="pipeline")
```

### Run

The `Run` object is created by calling `run()` on a Workflow. Run-level parameters are provided alongside task parameters.

#### Run parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| parameters | dict | Inputs for the build function. | None |

#### Run example

```python
run_build = workflow.run(action="build", parameters={"dataitem": dataitem.key})

run = workflow.run(action="pipeline")
```

#### Run methods

::: digitalhub_runtime_hera.entities.run.hera_run.entity.RunHeraRun.result
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_hera.entities.run.hera_run.entity.RunHeraRun.results
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true
