# Task Parameters

Tasks of kinds `job`, `serve`, and `build` allow you to execute a Python function, serve it as a service, or build its Docker image. A `Task` is created via the `run()` method and is not managed directly by the user. Task parameters are passed directly to the `run()` method and may vary depending on the task kind.

## Shared Parameters

| Name | Type | Description |
| --- | --- | --- |
| [action](#task-actions) | str | Task action. One of: `job`, `build`, `serve`. **Required.** |
| [node_selector](../../../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector configuration. |
| [volumes](../../../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. |
| [resources](../../../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. |
| [affinity](../../../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. |
| [tolerations](../../../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. |
| [envs](../../../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. |
| [secrets](../../../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. |
| [profile](../../../configuration/kubernetes/overview.md#profile) | str | Profile template. |

## Action-Specific Parameters

### Serve

| Name | Type | Description |
| --- | --- | --- |
| replicas | int | Number of replicas. |
| service_type | str | Service type. |

### Build

| Name | Type | Description |
| --- | --- | --- |
| [instructions](#instructions) | list[str] | Build instructions executed as RUN lines in the generated Dockerfile. |

## Task Actions

Actions must be one of the following:

- **`job`**: Execute function as a one-off task
- **`build`**: Create Docker image with dependencies
- **`serve`**: Deploy function as a service

### Serving

Use the `serve` action to deploy a function as a service on Kubernetes.

!!! warning
    It may take time for the service to become ready. The platform will notify the client when ready.

After the service is ready, call the inference endpoint with `run.invoke()`. This method accepts the same keyword arguments as `requests.request`; by default, the `url` is taken from the `run` object but you may override it with an explicit `url` parameter.

```python
run = function.run("serve", ...)

json_data = {
    "some-func-param": data
}

run.invoke(json=json_data)
```

### Instructions

List of strings representing instructions to be executed as RUN instructions in the Dockerfile.

```python
instructions = ["apt-get install -y git"]
```
