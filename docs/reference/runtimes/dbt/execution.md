# Execution Overview

This section explains how to execute a function in the DBT runtime.
First, we examine the usage pattern, then delve into the parameter structure.

## Usage Pattern

To execute a dbt function, follow this pattern:

1. Use `dh.new_function()` or `project.new_function()` to create the function, passing **function parameters**.
2. Call `function.run()` with the desired action, passing **task parameters** and **run parameters**.

```python
# Create function with function parameters
function = dh.new_function(
    name="my-function",
    kind="dbt",
    code="SELECT * FROM {{ ref('my_table_ref') }}"
)

# Execute with task and run parameters
run = function.run(
    action="transform",  # Task parameter
    inputs={"my_table_ref": dataitem.key},  # Run parameter
    outputs={"output_table": "mapped-name"}  # Run parameter
)
```

You can control whether the execution happens locally on your machine or remotely on the platform by setting the `local_execution` parameter (see [Local vs Remote Execution](#local-vs-remote-execution) for details).

### Local vs Remote Execution

When executing a function, you can choose between **local execution** and **remote execution** by setting the `local_execution` parameter in the run parameters.

- **Local Execution** (`local_execution=True`): The function runs directly on your local machine. You need to have the required dependencies of your function installed locally.

- **Remote Execution** (`local_execution=False`, default): The function is executed on a remote server or cluster managed by the platform.

## Parameter Structure

Parameters are organized into three categories:

- **Function Parameters**: Define the function's `spec` attributes, such as source code and execution environment. These are set when creating the function.

- **Task Parameters**: Specify the action type and execution environment configuration. For DBT runtimes, the action is `transform`.

- **Run Parameters**: Control runtime behavior, such as local vs. remote execution, input mappings, and output specifications.

## Task Actions

The DBT runtime supports one task action:

- **`transform`**: Execute a dbt transformation

## Detailed Documentation

For comprehensive details on each parameter category:

- [Function Parameters](entities/function.md) — Complete reference for function creation and configuration.
- [Task Parameters](entities/task.md) — Execution modes and runtime settings.
- [Run Parameters](entities/run.md) — Input/output mappings and execution options.
