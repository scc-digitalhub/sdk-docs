# DBT Runtime

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

## Usage overview

Use a Function's `run()` method to execute a dbt transformation. At a high level the runtime:

1. Downloads input dataitems (attempting to use each dataitem's `path` attribute). Supported path types include:
   - `http(s)://<url>`
   - `s3://<bucket>/<path>`
   - `sql://<database>(/<schema-optional>)/<table>`
   - `<local-path>`
2. Loads the data into temporary, versioned tables in the configured Postgres database (tables named `<dataitem-name>_v<dataitem-id>`). Temporary tables are removed after execution.
3. Collects the dbt project/code, generates required artifacts (profiles.yml, dbt_project.yml, etc.), and runs the dbt transformation.
4. Writes the resulting table back to the configured Postgres database. The table name is derived from the `outputs` parameter. The runtime then creates a Dataitem representing the output (accessible via run.outputs()). Output tables are typically named `<dataitem-output-name>_v<dataitem-output-id>`.

To execute a dbt transformation on the platform:

1. Create a `Function` resource that references your dbt code and declares inputs/outputs and call `function.run()` to [execute](execution.md) the transformation.

See [Examples](examples.md) for code samples.
