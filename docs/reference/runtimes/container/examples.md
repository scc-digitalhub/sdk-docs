# Examples

## Function Creation

```python
import digitalhub as dh

project = dh.get_or_create_project('my_project')
function = dh.new_function(
    kind='container',
    name='my_function',
    image='hello-world:latest'
)
```

## Task Execution

**Job execution:**

```python
run = function.run(
    action='job',
    instructions=["apt install git -y"],
)
```

**Build image (builds an image from source using a base image and instructions):**

```python
run = function.run(
    action='build',
    base_image='python:3.11-slim',
    instructions=["apt-get update && apt-get install -y git"],
    image='myrepo/myapp:0.1.0'
)
```

**Serve as service (exposes a service):**

```python
run = function.run(
    action='serve',
    replicas=2,
    service_ports=[{"port": 8080, "targetPort": 8080}],
    service_type='NodePort'
)
```

**Deploy application workload:**

```python
run = function.run(
    action='deploy',
    replicas=3,
    envs=[{"name": "ENV", "value": "prod"}],
    volumes=[{"name": "data", "mountPath": "/data"}]
)
```
