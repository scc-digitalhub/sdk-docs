# Examples

## Function Creation

```python
import digitalhub as dh

project = dh.get_or_create_project('my_project')
function = dh.new_function(
    kind='container',
    name='my_function',
    image='my-nonroot-image:latest',
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
function = dh.new_function(
    kind='container',
    name='my_function',
    base_image='my-image:latest',
)
run = function.run(
    action='build',
    base_image='python:3.11-slim',
    instructions=["RUN apt-get update && apt-get install -y git"],
)
```

**Serve as service (exposes a service):**

```python
function = dh.new_function(
    kind='container',
    name='my_function',
    image='my-nonroot-image:latest',
)
run = function.run(
    action='serve',
    replicas=2,
    service_ports=[{"port": 8080, "targetPort": 8080}],
    service_type='NodePort'
)

data = {
    "message": "Hello, World!"
}

# Send a request to the service
response = run.invoke(json=data)
print(response.json())

# Run invoke method accept requests.request parameters.
# It accepts also url parameter. The url MUST start
# with a valid HTTP scheme (http:// or https://) and should
# include the service url. To check the service url:
run.status.service['url']
```

## Tutorials

Find additional examples in the [tutorial repository](https://github.com/scc-digitalhub/digitalhub-tutorials) of the DSLab GitHub organization.
