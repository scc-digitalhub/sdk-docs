# ModelServe Runtime

The ModelServe runtime allows you to deploy ML models on Kubernetes or locally.

## Prerequisites

Supported Python version and required package:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-modelserve`

Install from PyPI:

```bash
python -m pip install digitalhub-runtime-modelserve
```

## Usage Overview

The ModelServe runtime provides several serve functions (`sklearnserve`, `mlflowserve`, `huggingfaceserve`, `kubeai-text`, `kubeai-speech`) and a `serve` task action. Typical usage:

1. Create a Function for the model and [call](execution.md) its `run()` method.
2. The runtime collects, loads and exposes the model as a service.
3. Call the run's `invoke()` method to send inference requests (the method accepts the same keyword arguments as `requests.request`).
4. Stop the service with `run.stop()` when finished.

The ModelServe runtime deploys an [mlserver](https://mlserver.readthedocs.io/en/latest/) inference server on Kubernetes (Deployment + Service).

!!! warning "Service responsiveness"
    It may take some time for the service to become ready. Use `run.refresh()` and inspect `run.status`. When ready, the `status` will include a `service` attribute.

```python
run.refresh()
run.status
```

After the service is ready, call the inference endpoint with `run.invoke()`. By default the `url` is taken from the `run` object; override it with an explicit `url` parameter if needed.

!!! note
    If you set `model_name` in the function spec and run remotely, pass `model_name` to `invoke()` so the runtime can target the model with the MLServer V2 endpoint ("http://{url-from-k8s}/v2/models/{model_name}/infer").

```python
data = [[...]]  # some array
json = {
    "inputs": [
        {
            "name": "input-0",
            "shape": [x, y],
            "datatype": "FP32",
            "data": data
        }
    ]
}

run.invoke(json=json)
```

See [Examples](examples.md) for code samples.
