# Define a Hera Pipeline

This section describes how to define a Hera pipeline function. A pipeline function is a Python function that returns a Hera `Workflow` object. Any arguments in the function signature are passed as `parameters` during `run(action="build")`.

## Pipeline Function Anatomy

Define a pipeline by creating a Python function that returns a Hera `Workflow` object:

```python
from hera.workflows import Workflow, DAG, Parameter
from digitalhub_runtime_hera.dsl import step

def pipeline(url: str):
    # Create a new Workflow with an entrypoint DAG and parameters
    with Workflow(entrypoint="dag", arguments=Parameter(name="url")) as w:
        with DAG(name="dag"):
            # Define workflow steps here
            ...
    return w
```

## Typical Pipeline Structure

1. Create a `Workflow` Hera object and set the entrypoint (usually a DAG).
2. Use a `DAG` or `Steps` context to define the workflow structure.
3. Add steps via `step(...)`, providing templates, function names, inputs/outputs and parameters.
4. Chain steps using Hera operators to define dependencies.
5. Return the `Workflow` Hera object.

## DSL Components

The runtime provides DSL helpers in `digitalhub_runtime_hera.dsl`:

### `step` function

`step(**step_kwargs)` creates a workflow step (a Hera Task) inside a DAG or Steps context. Main arguments:

| Parameter   | Type      | Example    | Description |
|---|---|---|---|
| template    | dict | {"action": "job"} | Parameters template to pass to `function.run()` or `workflow.run()`. The `action` key is always required. To pass inputs from other steps use the `{{inputs.parameters.parameter_name}}` template syntax. |
| function    | str  | "download-data" | Name of the digitalhub function to execute. |
| function_id | str  | "abc123"        | Function ID (optional). |
| workflow    | str  | "my-workflow"   | Workflow name (optional). |
| workflow_id | str  | "def456"        | Workflow ID (optional). |
| name        | str  | "step1"         | Step name. |
| inputs      | dict | {"some-input": ANOTHER_STEP.get_parameter("some-output")} | Step inputs. Keys become Hera Parameters; values can reference other steps' outputs. |
| outputs     | list | ["output1"]     | Step outputs. These become Hera Outputs and Artifacts. |

Other keyword arguments are forwarded to the underlying container template. `step` must be called inside a `DAG` or `Steps` context.

### `container_template` function

`container_template(...)` builds a Hera container template for a workflow step. It returns a Hera `Container` object and accepts similar arguments to `step` (template, function, name, inputs, outputs, ...). Use it directly for advanced scenarios or custom templates.

## Pipeline Definition Example

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
