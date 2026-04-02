# Execution Overview

This section explains how to execute a function in the Python runtime.
First, we list the function types and actions, then we examine the usage pattern, including local vs remote execution.
Finally, we provide links to detailed documentation for each parameter category.

## Function types and Actions

The Python runtime package exposes three function kinds:

- `python`: Execute general-purpose Python handlers on Kubernetes
- `guardrail`: Execute Python handlers that process incoming and outgoing requests
- `openinference`: Execute Python handlers that expose inference-style tensor schemas

Each kind supports a specific action set.

| Function Kind | Supported Actions |
| --- | --- |
| `python` | `job`, `serve`, `build` |
| `guardrail` | `serve`, `build` |
| `openinference` | `serve`, `build` |

## Usage Pattern

To execute a function, follow this pattern:

1. Implement a Python function (see [Function definition](define-function.md) for detailed instructions on creating Python, guardrail, and openinference handlers).
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

The same flow applies to the specialized kinds. The difference is the accepted function parameters and the action set:

- `guardrail` adds `processing_mode` and supports `serve` and `build`.
- `openinference` adds `model_name`, `inputs`, and `outputs` and supports `serve` and `build`.

You can control whether the execution happens locally on your machine or remotely on the platform by setting the `local_execution` parameter (see [Local vs Remote Execution](#local-vs-remote-execution) for details).

### Local vs Remote Execution

When executing a function, you can choose between **local execution** and **remote execution** by setting the `local_execution` parameter in the run parameters.

- **Local Execution** (`local_execution=True`): The function runs directly on your local machine. You need to have the required dependencies of your function installed locally.

- **Remote Execution** (`local_execution=False`, default): The function is executed on a remote server or cluster managed by the platform. Remember to provide the dependencies in the function's `requirements` parameter or in your `requirements.txt`.

!!! note
    Note that some features, like serving functions, are only available with remote execution.

## Parameter Documentation

Here are links to the detailed documentation for each Python action:

- [Python Job](actions/python-job.md) — Execute a `python` function as a one-off task
- [Python Serve](actions/python-serve.md) — Deploy a `python` function as an HTTP endpoint
- [Python Build](actions/python-build.md) — Build a container image for a `python` function
- [Guardrail Serve](actions/guardrail-serve.md) — Deploy a `guardrail` function as a request/response processor
- [Guardrail Build](actions/guardrail-build.md) — Build a container image for a `guardrail` function
- [OpenInference Serve](actions/openinference-serve.md) — Deploy an `openinference` function as an inference endpoint
- [OpenInference Build](actions/openinference-build.md) — Build a container image for an `openinference` function
