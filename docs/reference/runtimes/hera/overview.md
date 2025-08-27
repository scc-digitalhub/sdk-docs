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

## Usage overview

Use a Workflow's `run()` method to build and execute Hera pipelines. Typical workflow:

1. Define the functions (steps) to be executed; these may belong to other runtimes.
2. Implement a pipeline function that returns a Hera `Workflow` object (see [Pipeline definition](define-pipeline.md)).
3. Build the pipeline by calling `run(action="build")` (required).
4. [Execute](execution.md) the pipeline with `run(action="pipeline")`; a stepper will execute the Hera steps.

The runtime provides DSL helpers in `digitalhub_runtime_hera.dsl`. Use `step` and `container_template` to wrap digitalhub functions and workflows into Hera steps and container templates. The DSL supports both `DAG` and `Steps` contexts.

Core components:

- `step`: defines an individual workflow step inside a `DAG` or `Steps` context; it represents a task and can declare inputs, outputs and parameters.
- `container_template`: constructs a Hera container template (image, command, args). It is used by `step` and also available for advanced custom templates.

See [Examples](examples.md) for code samples.
