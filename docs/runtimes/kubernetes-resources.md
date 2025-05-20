# Using Kubernetes Resources for Runs

With SDK you can manage Kubernetes resources for your tasks. When you run a function you can require some Kubernetes resources for the task. Resources and data are specified in the `function.run()` method.
Here follows a description of the resources you can request with the `function.run()` method.

## Node selector

You can request a node selector for the container being launched by the task by passing the selector as a dictionary with the `node_selector` task parameters.

```python
node_selector = {
    "key": "Node selector key.",
    "value": "Node selector value."
}
```

## Volumes

With SDK you can request the following types of volumes:

- **Persistent volume claims (PVC)**
- **ConfigMap**

### Persistent volume claims (PVC)

You can ask for a persistent volume claim (pvc) to be mounted on the container being launched by the task.
You need to declare the volume type as `persistent_volume_claim`, a name for the PVC for the user (e.g., `my-pvc`), the mount path on the container and a optionally a spec with the size of the PVC on Kubernetes.

```python
volumes = [{
        "volume_type": "persistent_volume_claim",
        "name": "my-pvc",
        "mount_path": "/data",
        "spec": {
            "size": "1Gi"
        }
}]
```

### EmptyDir

You can ask for an `emptyDir` volume (ephimeral storage) to be mounted on the container being launched by the task.
You need to declare the volume type as `empty_dir`, a name for the user (e.g., `my-empty-dir`), the mount path on the container and a spec with the size of the emptyDir on Kubernetes (e.g., `size: 1Gi`).

```python
volumes = [{
        "volume_type": "empty_dir",
        "name": "my-empty-dir",
        "mount_path": "/data",
        "spec": {
            "size_limit": "1Gi"
        }
}]
```

## Resources

You can request a specific amount of hardware resources (cpu, memory, gpu) for the task, declared thorugh the `resources` task parameter; `resources` must be a map of Resource objects represented as a dictionary.
At the moment Digitalhub SDK supports:

- **CPU**
- **RAM memory**
- **GPU**

### CPU

You can request a specific amount of CPU for the task.
You need to declare the resource type as `cpu`, request and/or limit specifications.

```python

resources = {
    "cpu": {
        "requests": "12",
        "limits": "16"
    }
}
```

### RAM memory

You can request a specific amount of RAM memory for the task.
You need to declare the resource type as `mem`, request and/or limit specifications.

```python
resources = {
    "mem": {
        "requests": "64Gi",
    }
}
```

### GPU

Please see [Profile documentation](#profile).

## Secrets

You can request a secret injection into the container being launched by the task by passing the reference to the backend with the `secrets` task parameters.

```python
secrets = ["my-secret"]
```

## Envs

You can request an environment variable injection into the container being launched by the task by passing the reference to the backend with the `envs` task parameters.

```python
envs = [{
    "name": "env-name",
    "value": "value"
}]
```

## Tolerations

Please see [Kubernetes documentation](https://kubernetes.io/docs/home/).

## Affinity

Please see [Kubernetes documentation](https://kubernetes.io/docs/home/).

## Profile

Profile template. Ask your administrator for the available profiles.
Profile are used to request specific hardware resources (e.g., GPU) for the task.

```python
# Request 1 GPU A100
profile = "1xa100"
```

## Replicas

Number of replicas for the pod/deployment. It accepts an integer value.

```python
replicas = 3
```

## FS group

File system group ID. It accepts an integer value.

```python
fs_group = 1000
```

## Service port

Service port(s) where to expose the service. Must be: [{port: port, target_port: target_port}, ...].

```python
service_ports = [{
    "port": 80,
    "target_port": 80
}]
```

## Service type

Service type to expose. Must be a `str` of one of the following:

- `ClusterIP`
- `LoadBalancer`
- `NodePort`

```python
service_type = "NodePort"
```

## Run as user

User ID to run the container. It accepts an integer value.

```python
run_as_user = 1000
```

## Run as group

Group ID to run the container. It accepts an integer value.

```python
run_as_group = 1000
```
