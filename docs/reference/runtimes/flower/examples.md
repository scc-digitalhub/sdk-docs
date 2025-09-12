# Examples

```python
import digitalhub as dh

project = dh.get_or_create_project("my_project")

# Create Flower application function
function_app = project.new_function(
    name="flower-app",
    kind="flower-app",
    source="git+https://github.com/your-org/flower-repo"
)

run = function_app.run(
    action="train",
    parameters={"num_rounds": 10, "num_clients": 5}
)
```

## Tutorials

Find additional examples in the [tutorial repository](https://github.com/scc-digitalhub/digitalhub-tutorials) of the DSLab GitHub organization.
