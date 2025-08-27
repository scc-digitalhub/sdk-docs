# DBT runtime

The DBT runtime lets you run [dbt](https://www.getdbt.com/) transformations against your data. It wraps the DBT CLI and exposes a Function of kind `dbt` and a Task action for transformations.

## Prerequisites

Supported Python version and required package:

- `python >= 3.9, <3.13`
- `digitalhub-runtime-dbt`

Install from PyPI:

```bash
python -m pip install digitalhub-runtime-dbt    # for remote execution
python -m pip install digitalhub-runtime-dbt[local]  # for local execution
```

## Overview

Use a Function's `run()` method to execute a dbt transformation. At a high level the runtime:

1. Downloads input dataitems (attempting to use each dataitem's `path` attribute). Supported path types include:
   - `http(s)://<url>`
   - `s3://<bucket>/<path>`
   - `sql://<database>(/<schema-optional>)/<table>`
   - `<local-path>`
2. Loads the data into temporary, versioned tables in the configured Postgres database (tables named `<dataitem-name>_v<dataitem-id>`). Temporary tables are removed after execution.
3. Collects the dbt project/code, generates required artifacts (profiles.yml, dbt_project.yml, etc.), and runs the dbt transformation.
4. Writes the resulting table back to the configured Postgres database. The table name is derived from the `outputs` parameter. The runtime then creates a Dataitem representing the output (accessible via run.outputs()). Output tables are typically named `<dataitem-output-name>_v<dataitem-output-id>`.

### Function

The DBT runtime defines a Function of kind `dbt` used to run SQL/dbt transformations.

#### Function parameters

| Name | Type | Description |
| --- | --- | --- |
| project | str | Project name. Required only when creating from the library; otherwise **MUST NOT** be set. |
| name | str | Name that identifies the object. **Required.** |
| [kind](#function-kinds) | str | Function kind. **Required.** |
| uuid | str | Object ID in UUID4 format. |
| description | str | Description of the object. |
| labels | list[str] | List of labels. |
| embedded | bool | Whether the object should be embedded in the project. |
| [code_src](../configuration/code_src/overview.md#code-source-uri) | str | URI pointing to the source code. |
| [code](../configuration/code_src/overview.md#plain-text-source) | str | Source code provided as plain text. |
| base64 | str | Source code encoded as base64. |
| [handler](../configuration/code_src/overview.md#handler) | str | Function entrypoint. |
| lang | str | Source code language (informational). |

##### Function kinds

The `kind` parameter must be:

- `dbt`

### Task

The DBT runtime provides a `transform` task action to run dbt transformations. A `Task` is created by calling `run()` on the Function; task parameters are passed through that call and may vary by action.

#### Task parameters (shared)

| Name | Type | Description |
| --- | --- | --- |
| [action](#task-actions) | str | Task action. One of: `transform`. **Required.** |
| [node_selector](../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. |
| [volumes](../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. |
| [resources](../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. |
| [affinity](../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. |
| [tolerations](../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. |
| [envs](../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. |
| [secrets](../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. |
| [profile](../configuration/kubernetes/overview.md#profile) | str | Profile template. |

##### Task actions

Supported actions:

- `transform` — run a dbt transformation

### Run

The `Run` object is created by calling `run()` on a Function. Run-level parameters are provided alongside task parameters.

#### Run parameters

| Name | Type | Description |
| --- | --- | --- |
| local_execution | bool | Execute the run locally instead of remotely. (Default: False) |
| inputs | dict | Mapping of function argument names to entity keys. |
| outputs | dict | Mapping of outputs. **Must be** in the form: {"output_table": "your-table-output-name"} |
| parameters | dict | Extra parameters passed to the function. |

#### Run example

```python

```

### Run methods

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

## Examples

Function example

```python
import digitalhub as dh

project = dh.get_or_create_project("my_project")

sql = """
SELECT * FROM {{ ref("my_table_ref") }}
"""

dataitem = project.new_dataitem("my_dataitem", kind="table", path="path-to-some-data")

function = dh.new_function(
    kind="dbt",
    name="my_function",
    code=sql
)

run = function.run(
    action="transform",
    inputs={
        "my_table_ref": dataitem.key
    },
    outputs={
        "output_table": "mapped-name"
    }
)
```
