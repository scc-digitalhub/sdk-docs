# Execution Overview

This section explains how to execute a workflow in the Hera runtime.
First, we examine the usage pattern, then delve into the parameter structure.

## Usage Pattern

To execute a Hera workflow, follow this pattern:

1. Use `dh.new_workflow()` or `project.new_workflow()` to create the workflow, passing **workflow parameters**.
2. Call `workflow.run()` with the desired action, passing **task parameters** and **run parameters**.

```python
# Create workflow with workflow parameters
workflow = dh.new_workflow(
    name="my-workflow",
    kind="hera",
    code_src="pipeline.py",
    handler="pipeline"
)

# Build the pipeline
run_build = workflow.run(
    action="build"  # Task parameter

)

# Execute the pipeline
run_pipeline = workflow.run(
    action="pipeline",  # Task parameter
    parameters={"url": "https://example.com"}  # Run parameter
)
```

Hera workflows are executed remotely on Kubernetes clusters managed by the platform.

## Parameter Structure

Parameters are organized into three categories:

- **Workflow Parameters**: Define the workflow's `spec` attributes, such as source code and execution environment. These are set when creating the workflow.

- **Task Parameters**: Specify the action type and execution environment configuration. For Hera runtimes, actions are `build` and `pipeline`.

- **Run Parameters**: Control runtime behavior, such as pipeline parameters passed to the workflow function.

## Task Actions

The Hera runtime supports two task actions:

- **`build`**: Build the pipeline definition in Argo YAML
- **`pipeline`**: Execute the built pipeline

## Detailed Documentation

For comprehensive details on each parameter category:

- [Workflow Parameters](entities/workflow.md) — Complete reference for workflow creation and configuration.
- [Task Parameters](entities/task.md) — Execution modes and runtime settings.
- [Run Parameters](entities/run.md) — Input/output mappings and execution options.
