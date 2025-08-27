# Execution Overview

This section explains how to execute a function in the Python runtime.
First, we examine the usage pattern, then delve into the parameter structure.

## Usage Pattern

To execute a function, follow this pattern:

1. Use `dh.new_function()` or `project.new_function()` to create the function, passing **function parameters**.
2. Call `function.run()` with the desired action, passing **task parameters** and **run parameters**.

```python
# Create function with function parameters
function = dh.new_function(
    name="my-function",
    kind="python",
    code_src="handler.py",
    handler="main",
    python_version="PYTHON3_10"
)

# Execute with task and run parameters
run = function.run(
    action="job",  # Task parameter
    inputs={"data": dataitem.key},  # Run parameter
    parameters={"threshold": 0.5}  # Run parameter
)
```

## Parameter Structure

Parameters are organized into three categories, each serving a distinct purpose in the function execution lifecycle and in the specification of execution entities (`Function`, `Task`, `Run`):

- **Function Parameters**: Define the function's `spec` attributes, such as source code, handler, Python version, and execution environment. These are set when creating the function using `dh.new_function()` or `project.new_function()`.

- **Task Parameters**: Specify the action type and execution environment configuration. For Python runtimes, actions are `job`, `serve`, or `build`.

- **Run Parameters**: Control runtime behavior, such as local vs. remote execution, input mappings, and additional parameters passed to the function handler.

## Detailed Documentation

For comprehensive details on each parameter category:

- [Function Parameters](entities/function.md) — Complete reference for function creation and configuration.
- [Task Parameters](entities/task.md) — Execution modes and runtime settings.
- [Run Parameters](entities/run.md) — Input/output mappings and execution options.
