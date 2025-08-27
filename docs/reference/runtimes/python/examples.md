# Examples

Function creation example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my_project")

# From project
function = project.new_function(
    name="python-function",
    kind="python",
    code_src="main.py",
    handler="function",
    python_version="PYTHON3_10"
)

# Or from sdk
function = dh.new_function(
    project="my-project",
    name="python-function",
    kind="python",
    code_src="main.py",
    handler="function",
    python_version="PYTHON3_10"
)
```

Task examples:

```python
# Job execution
run = function.run(
    action="job",
    inputs={
        "dataitem": dataitem.key
    }
)

# Build image
run = function.run(
    action="build",
    instructions=["apt-get install -y git"]
)

# Serve as service
run = function.run(
    action="serve",
    replicas=2,
    service_type="NodePort"
)
```

Service invocation example:

```python
# After serving
run = function.run("serve", ...)

json = {
    "some-func-param": data
}

run.invoke(json=json)
```

You can find a list of tutorials at the [tutorial repository](https://github.com/scc-digitalhub/digitalhub-tutorials) of the DSLab Github organization.

In particular the tutorial on [custom ML model train and serve](https://github.com/scc-digitalhub/digitalhub-tutorials/blob/main/s6-custom-ml-model/notebook-cml-darts-ci.ipynb) covers all the possible task accomplished with the Python runtime.
