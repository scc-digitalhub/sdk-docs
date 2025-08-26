# Hera Pipelines Runtime

The **hera runtime** allows you to run workflows within the platform.
The runtime introduces a workflow of kind `hera` and a task of kind `pipeline`.

## Prerequisites

Python version and libraries:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-hera`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-hera
```

## HOW TO

With the hera runtime you can use the function's `run()` method to execute a workflow you have defined.
The hera runtime execution workflow follows roughly these steps:

1. Define one or more functions to be executed. These functions can be from other runtimes.
2. Define somewhere a [pipeline](./hera.md#pipeline-definition).
3. Build the pipeline with the `run(action="build")` method. (Mandatory step!)
4. Execute the pipeline with `run(action="pipeline")` method. This calls a stepper that executes various Hera steps.

### Pipeline definition

To define a pipeline, create a Python function using the `def` keyword. This function should return a Hera `Workflow` object, which describes the pipeline structure and steps. You can declare arguments in the function signature; these will be passed as `parameters` during the `run(action="build")` method.

Use the DSL helpers from `digitalhub_runtime_hera.dsl`. The `step` and `container_op` functions from `digitalhub_runtime_hera.dsl` are used to wrap digitalhub functions and workflows into Hera steps/tasks and containers. These allow you to build complex workflows using the Hera DSL, supporting both DAG and Steps contexts and the digitalhub entities.
The main components are:

- `step`: Defines individual steps within the pipeline, to be used inside a `DAG` or `Steps` context. Each step represents a task, and can specify inputs, outputs, and parameters for chaining data between steps. See below for details.
- `container_op`: Defines the container template for a step, specifying the image, command, and arguments for execution. Used internally by `step`, but available for advanced customization.

The pipeline function is typically structured as follows:

1. Create a `Workflow` object, specifying the entrypoint (usually a DAG).
2. Use a `DAG` or `Steps` context to define the workflow structure.
3. Add steps using the `step` function, specifying templates, function names, inputs, outputs, and parameters.
4. Chain steps using Hera operators for dependencies.
5. Return the workflow object.

#### `step` function

`step(**step_kwargs)` creates a workflow step inside a DAG or Steps context. It wraps a container operation and returns a Hera `Task` object. The main arguments are:

| Parameter   | Type      | Example    | Description                                                     |
|---|---|---|---|
| template    | dict | {"action": "job"}| Parameters template to pass to function.run() or workflow.run(). Check the proper runtime documentation for details. Remember also that the `action` key is always required. If you need to pass inputs from other steps for example in a digitalhub/python function, use the `{{inputs.parameters.parameter_name}}` syntax.         |
| function    | str  | "download-data" | Name of the function to execute. |
| function_id | str  | "abc123"        | Function ID (optional).          |
| workflow    | str  | "my-workflow"   | Workflow name (optional).        |
| workflow_id | str  | "def456"        | Workflow ID (optional).          |
| name        | str  | "step1"         | Step name.                       |
| inputs      | dict | {"some-input": ANOTHER_STEP.get_parameter("some-output")} | Step inputs (parameter names). Inputs keys will be mapped in the step as Hera Parameters. The whole inputs as arguments for the task. |
| outputs     | list | ["output1"]     | Step outputs (parameter names).   Outputs will be mapped in the step as Hera Outputs and Artifacts.|

Other keyword arguments are passed to the underlying container template. The step must be called inside a DAG or Steps context, otherwise an error is raised.

#### `container_op` function

`container_op(...)` creates a Hera container template for a workflow step. It is used internally by `step`, but can be used directly for advanced scenarios. Main arguments (see above for details):

| Parameter   | Type      | Description                                  |
|-------------|-----------|----------------------------------------------|
| template    | dict      | Parameters template to pass to .run() method.|
| function    | str       | Function name.                               |
| function_id | str       | Function ID.                                 |
| workflow    | str       | Workflow name.                               |
| workflow_id | str       | Workflow ID.                                 |
| name        | str       | Step name.                                   |
| inputs      | list      | Step inputs.                                 |
| outputs     | list      | Step outputs.                                |

Returns a Hera `Container` object, which is used as the template for the step. It must be used outside a `DAG` or `Steps` context.

#### Workflow definition example

```python
from hera.workflows import Workflow, DAG, Parameter
from digitalhub_runtime_hera.dsl import step


# Define the pipeline function with no arguments
def pipeline():

    # Create a new Workflow with an entrypoint DAG
    with Workflow(entrypoint="dag", arguments=Parameter(name="url")) as w:
        with DAG(name="dag"):

            # Define the first step. It takes inputs from the workflow parameters
            # and outputs a dataset that can be used in subsequent steps.
            A = step(template={"action":"job", "inputs": {"url": "{{workflow.parameters.url}}"}},
                     function="download-data",
                     outputs=["dataset"])

            # Define subsequent steps
            # Note the way inputs are passed from one step to another,
            # using the get_parameter method to retrieve outputs from previous steps
            # and use the template {{inputs.parameters.parameter_name}}
            # This allows chaining data between steps
            # Because we shortcut the definition of a container template inside a dag,
            # we can pass directly the inputs as a dictionary. The key will be used
            # as the input Parameter name, the whole inputs as arguments for the task.
            B = step(template={"action":"job", "inputs": {"di": "{{inputs.parameters.di}}"}},
                     function="process-spire",
                     inputs={"di": A.get_parameter("dataset")})
            C = step(template={"action":"job", "inputs": {"di": "{{inputs.parameters.di}}"}},
                     function="process-measures",
                     inputs={"di": A.get_parameter("dataset")})

            # Chain the steps
            A >> [B, C]

    # Return the workflow
    return w

```

### Workflow

The hera runtime introduces a function of kind `hera`.

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
| [code_src](../configuration/code_src/overview.md#code-source-uri) | str | URI pointer to source code | None |
| [code](../configuration/code_src/overview.md#plain-text-source) | str | Source code (plain text)| None |
| base64 | str | Source code (base64 encoded)| None |
| [handler](../configuration/code_src/overview.md#handler) | str | Function entrypoint | None |
| lang | str | Source code language (hint)| None |

##### Workflow kinds

The `kind` parameter must be one of the following:

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

The Hera runtime introduces a task of kind `pipeline` that allows you to run a workflow.
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
