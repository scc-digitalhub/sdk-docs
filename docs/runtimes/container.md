# Container runtime

The **Container runtime** allows you to create deployments, jobs and services on Kubernetes.

## Prerequisites

Python version and libraries:

- `python >= 3.9`
- `digitalhub-runtime-container`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-container
```

## HOW TO

With the Container runtime you can launch pods and services on Kubernetes. It is built having **remote online execution** capabilities.

### Function

The Container runtime introduces a function of kind `container` that allows you to deploy deployments, jobs and services on Kubernetes.

#### Function parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name | required (if creating from library) |
| name | str | Name that identifies the object | required |
| kind | str | Kind of the object | required (must be `container`) |
| uuid | str | ID of the object in form of UUID | None |
| description | str | Description of the object | None |
| git_source | str | Remote git source for object | None |
| labels | list[str] | List of labels | None |
| embedded | bool | Flag to determine if object must be embedded in project | True |
| [code_src](#source) | str | URI pointer to source code | None |
| code | str | Source code (plain text)| None |
| base64 | str | Source code (base64 encoded)| None |
| handler | str | Function entrypoint | None |
| lang | str | Source code language (hint)| None |
| image | str | The image to use | None |
| base_image | str | The base container image | None (required if task is `build`) |
| command | str | The command to run inside the container | None |
| args | list[str] | The arguments to pass to the command | None |

##### Source

Source code can be specified with `code_src` as an URI. It can have three different type of schema:

| schema | value | description |
| --- | --- | --- |
| None | "path/to/file.ext" | Local file path |
| git+https | "git+https://github.com/some-user/some-repo" | Remote git repository |
| zip+s3 | "zip+s3://some-bucket/some-key.zip" | Remote zip s3 archive |

#### Function example

```python
import digitalhub as dh

project = dh.get_or_create_project('my_project')
function = dh.new_function(
    kind='container',
    name='my_function',
    image="hello-world:latest"
)
```

### Task

The Container runtime introduces three task's kinds:

- `job`: to deploy a job
- `deploy`: to deploy a deployment
- `serve`: to deploy a service
- `build`: to build a docker image

#### Task parameters

| Name | Type | Description | Default | Kind specific |
| --- | --- | --- | --- | --- |
| action | str | Task action. Must be one of: <li>`job`</li><li>`deploy`</li><li>`build`</li><li>`build`</li> | required | |
| [node_selector](kubernetes-resources.md#node_selector) | list[dict] | Node selector | None | |
| [volumes](kubernetes-resources.md#volumes) | list[dict] | List of volumes | None | |
| [resources](kubernetes-resources.md#resources) | dict | Resources restrictions | None | |
| [affinity](kubernetes-resources.md#affinity) | dict | Affinity | None | |
| [tolerations](kubernetes-resources.md#tolerations) | list[dict] | Tolerations | None | |
| [envs](kubernetes-resources.md#envs) | list[dict] | Env variables | None | |
| [secrets](kubernetes-resources.md#secrets) | list[str] | List of secret names | None | |
| [profile](kubernetes-resources.md#profile) | str | Profile template | None | |
| backoff_limit | int | Backoff limit | None | `job` |
| schedule | str | Schedule for the job | None | `job` |
| instructions | list[str] | Build instructions to be executed as RUN instructions in Dockerfile.<br>Example: `apt install git -y` | None | `build` |
| replicas | int | Number of replicas | None | `deploy`, `serve` |
| service_port| list[dict] | Service port where to expose the service. Must be: [{port: port, target_port: target_port}, ...] | `NodePort` | `serve` |
| service_type| str | Service type. Must be one of: <li>`ClusterIP`</li><li>`LoadBalancer`</li><li>`NodePort`</li> | `NodePort` | `serve` |
