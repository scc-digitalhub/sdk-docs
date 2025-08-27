# Task Parameters

A set of tasks of kinds `job`, `serve` and `build` allow you to run a Python function execution, serve a function as a service, or build the Docker image used to execute the function.
A `Task` is created with the `run()` method, so it's not managed directly by the user. The parameters for the task creation are passed directly to the `run()` method, and may vary depending on the kind of task.

## Task parameters (shared)

| Name | Type | Description |
| --- | --- | --- |
| [action](#task-actions) | str | Task action. One of: `job`, `build`, `serve`. **Required.** |
| [node_selector](../../../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. |
| [volumes](../../../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. |
| [resources](../../../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. |
| [affinity](../../../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. |
| [tolerations](../../../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. |
| [envs](../../../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. |
| [secrets](../../../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. |
| [profile](../../../configuration/kubernetes/overview.md#profile) | str | Profile template. |

### Action-specific parameters

- `serve`

| Name | Type | Description |
| --- | --- | --- |
| [replicas](../../../configuration/kubernetes/overview.md#replicas) | int | Number of replicas. |
| [service_type](../../../configuration/kubernetes/overview.md#service-port-type) | str | Service type. |

- `build`

| Name | Type | Description |
| --- | --- | --- |
| [instructions](#instructions) | list[str] | Build instructions executed as RUN lines in the generated Dockerfile. |

## Task actions

Actions must be one of the following:

- `job`
- `build`
- `serve`

### Serving

Use the `serve` action to deploy a function as a service on Kubernetes.

!!! warning "Service responsiveness"
    It may take some time for the service to become ready and for the platform to notify the client.

After the service is ready, call the inference endpoint with `run.invoke()`.
`run.invoke()` accepts the same keyword arguments as `requests.request`; by default the `url` is taken from the `run` object but you may override it with an explicit `url` parameter.

```python
run = function.run("serve", ...)

json = {
    "some-func-param": data
}

run.invoke(json=json)
```

### Instructions

List of `str` representing the instructions to be executed as RUN instructions in Dockerfile.

```python
instructions = ["apt-get install -y git"]
```
