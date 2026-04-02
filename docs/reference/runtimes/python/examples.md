# Examples

## Function Creation

```python
import digitalhub as dh

project = dh.get_or_create_project("my_project")

# Create function from project
function = project.new_function(
    name="python-function",
    kind="python",
    code_src="main.py",
    handler="function",
    python_version="PYTHON3_10"
)

# Or create function from SDK
function = dh.new_function(
    project="my-project",
    name="python-function",
    kind="python",
    code_src="main.py",
    handler="function",
    python_version="PYTHON3_10"
)
```

**Guardrail function:**

```python
guardrail_function = project.new_function(
    name="request-guardrail",
    kind="guardrail",
    code_src="guardrail.py",
    handler="process",
    python_version="PYTHON3_10",
    processing_mode="preprocessor"
)
```

**OpenInference function:**

```python
openinference_function = project.new_function(
    name="text-inference",
    kind="openinference",
    code_src="inference.py",
    handler="predict",
    python_version="PYTHON3_10",
    model_name="text-classifier",
    inputs=[{"name": "input-0", "shape": [-1], "datatype": "BYTES"}],
    outputs=[{"name": "output-0", "shape": [-1], "datatype": "FP32"}]
)
```

## Task Execution

**Job execution:**

```python
run = function.run(
    action="job",
    inputs={"dataitem": dataitem.key},
    parameters={"param1": "value1"},
)
```

**Build image:**

```python
run = function.run(
    action="build",
    instructions=["apt-get install -y git"]
)
```

**Serve as service:**

```python
run = function.run(
    action="serve",
    replicas=2,
    service_type="NodePort"
)
```

**Guardrail serve:**

```python
run = guardrail_function.run(
    action="serve",
    replicas=1,
    service_type="ClusterIP"
)
```

**OpenInference build:**

```python
run = openinference_function.run(
    action="build",
    instructions=["pip install transformers"]
)
```

## Service Invocation

After deploying a service:

```python
run = function.run("serve", ...)

# Pass some data to the function
json_data = {
    "some-func-param": data
}

# run invoke returns a requests.Response object
run.invoke(json=json_data)

# Run invoke method accept requests.request parameters.
# It accepts also url parameter. The url MUST start
# with a valid HTTP scheme (http:// or https://) and should
# include the service url. To check the service url:
run.status.service['url']
```

An OpenInference endpoint typically receives tensor-oriented payloads:

```python
json_payload = {
    "inputs": [
        {
            "name": "input-0",
            "shape": [-1],
            "datatype": "BYTES",
            "data": ["hello world"]
        }
    ]
}

serve_run = openinference_function.run(action="serve", replicas=1)
serve_run.invoke(json=json_payload)
```

## Tutorials

Find additional examples in the [tutorial repository](https://github.com/scc-digitalhub/digitalhub-tutorials) of the DSLab GitHub organization.
