# DBT runtime

The **DBT runtime** allows you to run [DBT](https://www.getdbt.com/) transformations on your data. It is a wrapper around the DBT CLI tool.
The runtime introduces a function of kind `dbt` and a task of kind `transform`.

## Prerequisites

Python version and libraries:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-dbt`

The package is available on PyPI:

```bash
python -m pip install digitalhub-runtime-dbt # for remote execution only
python -m pip install digitalhub-runtime-dbt[local] # for local execution
```

## HOW TO

With the DBT runtime you can use the function's `run()` method to execute a DBT query you have defined.
The DBT runtime execution workflow follows roughly these steps:

1. The runtime fetches the input dataitems by downloading them locally. The runtime tries to get the file from the `path` attribute in the dataitem specification. At the moment, we support the following path types:
     - `http(s)://<url>`
     - `s3://<bucket>/<path>`
     - `sql://<database>(/<schema-optional>)/<table>`
     - `<local-path>`
2. The runtime inserts the data into a temporary versioned table in the default postgres database. These tables are named `<dataitem-name>_v<dataitem-id>`, and will be deleted at the end of the execution.
3. The runtime collect the source code of the DBT query and creates all the necessary DBT artifacts (profiles.yml, dbt_project.yml, etc.) and runs the DBT transformation.
4. The runtime stores the output table into the default postgres database as result of the DBT execution. The table name is built from the `outputs` parameter. Then, the runtime creates a dataitem with the `outputs` name parameter and saves it into the Core backend. You can retrieve the dataitem with the `run.outputs()` method. In general, the output table versioned is named `<dataitem-output-name>_v<dataitem-output-id>` and is stored in the default postgres database passed to the runtime via env variable.

### Function

The DBT runtime introduces a function of kind `dbt` that allows you to execute sql dbt queries on your data.

#### Function parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| project | str | Project name. Required only if creating from library, otherwise **MUST NOT** be set | |
| name | str | Name that identifies the object | required |
| [kind](#function-kinds) | str | Function kind | required |
| uuid | str | ID of the object in form of UUID4 | None |
| description | str | Description of the object | None |
| labels | list[str] | List of labels | None |
| embedded | bool | Flag to determine if object must be embedded in project | True |
| [code_src](../configuration/code_src/overview.md#code-source-uri) | str | URI pointer to source code | None |
| [code](../configuration/code_src/overview.md#plain-text-source) | str | Source code (plain text)| None |
| base64 | str | Source code (base64 encoded)| None |
| [handler](../configuration/code_src/overview.md#handler) | str | Function entrypoint | None |
| lang | str | Source code language (hint)| None |

##### Function kinds

The `kind` parameter must be:

- `dbt`

#### Function example

```python
import digitalhub as dh

project = dh.get_or_create_project("my_project")

sql = """
SELECT * FROM {{ ref("my_table") }}
"""

dataitem = project.new_dataitem("my_dataitem", kind="table", path="path-to-some-data")

function = dh.new_function(
    kind="dbt",
    name="my_function",
    code=sql
)
```

### Task

The DBT runtime introduces a task of kind `transform` that allows you to run a DBT transformation on your data.
A `Task` is created with the `run()` method, so it's not managed directly by the user. The parameters for the task creation are passed directly to the `run()` method, and may vary depending on the kind of task.

#### Task parameters

| Name | Type | Description | Default | Kind specific |
| --- | --- | --- | --- | --- |
| [action](#task-actions) | str | Task action | required | |
| [node_selector](../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector | None | |
| [volumes](../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes | None | |
| [resources](../configuration/kubernetes/overview.md#resources) | dict | Resources restrictions | None | |
| [affinity](../configuration/kubernetes/overview.md#affinity) | dict | Affinity | None | |
| [tolerations](../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations | None | |
| [envs](../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Env variables | None | |
| [secrets](../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names | None | |
| [profile](../configuration/kubernetes/overview.md#profile) | str | Profile template | None | |

##### Task actions

Actions must be one of the following:

- `serve`: to deploy a service

#### Task example

```python
run = function.run(
    action="transform",
    inputs={"my_table": my_dataitem.key},
    outputs={"output_table": "my_output_table"},
)
```

### Run

The `Run` object is, similar to the `Task`, created with the `run()` method.
The run's parameters are passed alongside the task's ones.

#### Run parameters

| Name | Type | Description | Default |
| --- | --- | --- | --- |
| loacal_execution | bool | Flag to indicate if the run will be executed locally | False |
| inputs | dict | Input entity key. | None |
| outputs | dict | Outputs mapped. | None |
| parameters | dict | Extra parameters for a function. | None |

#### Run example

```python
run = function.run(
    action="job",
    inputs={
        "dataitem": dataitem.key
    },
    outputs={
        "dataitem": "mapped-name",
        "label": "some-label"
    }
)
```

#### Run methods

::: digitalhub_runtime_dbt.entities.run.dbt_run.entity.RunDbtRun.output
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_runtime_dbt.entities.run.dbt_run.entity.RunDbtRun.outputs
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true
