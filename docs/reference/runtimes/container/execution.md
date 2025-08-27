# Execution Overview

This section explains how to execute a function in the Container runtime.
First, we examine the usage pattern, then delve into the parameter structure.

## Usage Pattern

To execute a container function, follow this pattern:

1. Use `dh.new_function()` or `project.new_function()` to create the function, passing **function parameters**.
2. Call `function.run()` with the desired action, passing **task parameters** and **run parameters**.

```python
# Create function with function parameters
function = dh.new_function(
    name="my-function",
    kind="container",
    image="my-image:latest",
    command="my-command"
)

# Execute with task and run parameters
run = function.run(
    action="job",  # Task parameter
    args=["arg1", "arg2"]  # Run parameter
)
```

Container functions are executed remotely on Kubernetes clusters managed by the platform.

## Parameter Structure

Parameters are organized into three categories:

- **Function Parameters**: Define the function's `spec` attributes, such as image, command, and execution environment. These are set when creating the function.

- **Task Parameters**: Specify the action type and execution environment configuration. For Container runtimes, actions are `job`, `serve`, `build`, or `deploy`.

- **Run Parameters**: Control runtime behavior, such as command arguments passed to the container.

## Task Actions

The Container runtime supports four task actions:

- **`job`**: Execute a container as a one-off job
- **`serve`**: Deploy a container as a long-lived service
- **`build`**: Create a Docker image with custom instructions
- **`deploy`**: Deploy an application workload

## Detailed Documentation

For comprehensive details on each parameter category:

- [Function Parameters](entities/function.md) — Complete reference for function creation and configuration.
- [Task Parameters](entities/task.md) — Execution modes and runtime settings.
- [Run Parameters](entities/run.md) — Input/output mappings and execution options.
