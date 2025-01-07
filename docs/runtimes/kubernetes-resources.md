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
You need to declare the volume type as `persistent_volume_claim`, a name for the PVC for the user (e.g., `my-pvc`), the mount path on the container and a spec with the name of the PVC on Kubernetes (e.g., `pvc-name-on-k8s`).

```python
volumes = [{
        "volume_type": "persistent_volume_claim",
        "name": "my-pvc",
        "mount_path": "/data",
        "spec": {
            "claim_name": "pvc-name-on-k8s",
            }
}]
```

### ConfigMap

You can ask for a configmap to be mounted on the container being launched by the task.
You need to declare the volume type as `config_map`, a name for the ConfigMap for the user (e.g., `my-config-map`), the mount path on the container and a spec with the name of the ConfigMap on Kubernetes (e.g., `config-map-name-on-k8s`).

```python
volumes = [{
        "volume_type": "config_map",
        "name": "my-config-map",
        "mount_path": "/data",
        "spec": {
            "name": "config-map-name-on-k8s"
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
    "mem"{
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

Profile template.

## Schedule

Schedule for the job. It accepts a cron expression.

```python
schedule = "0 0 * * *"
```

## Replicas

Number of replicas for the pod/deployment. It accepts an integer value.

```python
replicas = 3
```

## Backoff limit

Backoff limit for the job. It accepts an integer value.

```python
backoff_limit = 3
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
