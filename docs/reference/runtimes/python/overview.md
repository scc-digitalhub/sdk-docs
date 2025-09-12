# Python Runtime

The Python runtime enables you to execute user-defined Python handlers. It registers a `Function` kind `python` and supports three actions: `job`, `serve`, and `build`.

- **`job`**: Execute a Python handler as a one-off job
- **`serve`**: Deploy a Python handler as a long-lived service
- **`build`**: Create a Docker image with all required dependencies for efficient execution

## Prerequisites

**Supported Python versions:**

- Python ≥ 3.9, < 3.13

**Required packages:**

- `digitalhub-runtime-python`

Install from PyPI:

```bash
pip install digitalhub-runtime-python
```

## Usage overview

To execute a Python handler on the platform:

1. Implement the handler as described in [handler definition](define-function.md).
2. Create a `Function` resource that references your handler and declares inputs/outputs.
3. Call `function.run()` to execute the handler.

See [how to](how-to.md) for detailed instructions on which actions you can execute and how to configure your Python functions.
See [Examples](examples.md) for code samples.
