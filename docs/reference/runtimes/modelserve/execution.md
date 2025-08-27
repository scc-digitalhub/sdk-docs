# Execution Overview

This section explains how to execute a function in the ModelServe runtime.
First, we examine the usage pattern, then delve into the parameter structure.

## Usage Pattern

To execute a ModelServe function, follow this pattern:

1. Use `dh.new_function()` or `project.new_function()` to create the function, passing **function parameters**.
2. Call `function.run()` with the desired action, passing **task parameters** and **run parameters**.

```python
# Create function with function parameters
function = dh.new_function(
    name="my-model-function",
    kind="mlflowserve",
    path="s3://my-bucket/path-to-model"
)

# Execute with task and run parameters
run = function.run(
    action="serve",  # Task parameter
    replicas=1  # Task parameter
)
```

ModelServe functions are executed remotely on Kubernetes clusters managed by the platform.

## Parameter Structure

Parameters are organized into three categories:

- **Function Parameters**: Define the function's `spec` attributes, such as model path, image, and execution environment. These are set when creating the function.

- **Task Parameters**: Specify the action type and execution environment configuration. For ModelServe runtimes, the action is `serve`.

- **Run Parameters**: Control runtime behavior, such as environment variables and scaling parameters.

## Task Actions

The ModelServe runtime supports one task action:

- **`serve`**: Deploy a model as a service

## Detailed Documentation

For comprehensive details on each parameter category:

- [Function Parameters](entities/function.md) — Complete reference for function creation and configuration.
- [Task Parameters](entities/task.md) — Execution modes and runtime settings.
- [Run Parameters](entities/run.md) — Input/output mappings and execution options.
