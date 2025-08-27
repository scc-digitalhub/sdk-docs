# Run

The `Run` object is created by calling `run()` on a Function. Run-level parameters are provided alongside task parameters.

## Parameters (Shared)

No shared specific parameters for run of this runtime.

## Function Kind-Specific Parameters

### KubeAI Text & Speech

| Name | Type | Description |
| --- | --- | --- |
| env | dict | Environment variables. |
| args | list[str] | Arguments. |
| cache_profile | str | Cache profile. |
| [files](#files) | list[KubeaiFile] | Files. |
| [scaling](#scaling) | Scaling | Scaling parameters. |
| processors | int | Number of processors. |

## Files

Files is a list of dict with the following keys:

```python
files = [
    {
        "path": "file-path"
        "content": "file-content"
    }
]
```

## Scaling

Scaling is a `Scaling` object that represents the scaling parameters for the run. Its structure is as follows:

```python
scaling = {
    "replicas": int,
    "min_replicas": int,
    "max_replicas": int,
    "autoscaling_disabled": bool,
    "target_request": int,
    "scale_down_delay_seconds": int,
    "load_balancing": {
        "strategy": str,  # "LeastLoad" or "PrefixHash"
        "prefix_hash": {
            "mean_load_factor": int,
            "replication": int,
            "prefix_char_length": int
        }
    }
}
```

## Methods

Once the run is created, you can access its attributes and methods through the `run` object.

::: digitalhub_runtime_modelserve.entities.run.modelserve_run.entity.RunModelserveRun.invoke
    options:
        heading_level: 6
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true
