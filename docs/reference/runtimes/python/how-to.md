# Execution Overview

This section explains how to execute a function in the Python runtime.
First, we list the function types and actions, then we examine the usage pattern, including local vs remote execution.
Finally, we provide links to detailed documentation for each parameter category.

## Function types and Actions

There is one function kind in the Python runtime:

- `python`: Execute Python handlers on Kubernetes

The kind supports specific actions.

| Function Kind | Supported Actions |
| --- | --- |
| `python` | `job`, `serve`, `build` |

## Usage Pattern

To execute a function, follow this pattern:

1. Implement a Python function (see [Function definition](define-function.md) for detailed instructions on creating Python functions).
2. Use `dh.new_function()` or `project.new_function()` to create the function, passing **function parameters**.
3. Call `function.run()` with the desired action, passing **task parameters** and **run parameters**.

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

You can control whether the execution happens locally on your machine or remotely on the platform by setting the `local_execution` parameter (see [Local vs Remote Execution](#local-vs-remote-execution) for details).

### Local vs Remote Execution

When executing a function, you can choose between **local execution** and **remote execution** by setting the `local_execution` parameter in the run parameters.

- **Local Execution** (`local_execution=True`): The function runs directly on your local machine. You need to have the required dependencies of your function installed locally.

- **Remote Execution** (`local_execution=False`, default): The function is executed on a remote server or cluster managed by the platform. Remember to provide the dependencies in the function's `requirements` parameter or in your `requirements.txt`.

!!! note
    Note that some features, like serving functions, are only available with remote execution.

## Parameter Documentation

Here are links to the detailed documentation for each Python action:

- [Python Job](actions/python-job.md) — Execute a Python function as a one-off task
- [Python Serve](actions/python-serve.md) — Deploy a Python function as an HTTP endpoint
- [Python Build](actions/python-build.md) — Build a container image for a Python function
