# Run

The `Run` object is created by calling `run()` on a Function. Run-level parameters are provided alongside task parameters.

## Parameters

| Name | Type | Description |
| --- | --- | --- |
| local_execution | bool | Execute the run locally instead of remotely. (Default: False) |
| inputs | dict | Mapping of function argument names to entity keys. |
| outputs | dict | Mapping of outputs. **Must be** in the form: {"output_table": "your-table-output-name"} |
| parameters | dict | Extra parameters passed to the function. |

## Methods

Once the run is created, you can access its attributes and methods through the `run` object.

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

::: digitalhub_runtime_dbt.entities.run.dbt_run.entity.RunDbtRun.inputs
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true
