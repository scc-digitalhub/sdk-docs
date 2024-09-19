# Run object

The `Run` object comes with three sets of methods: CRUD methods, generic run methods and (eventual) kind specific methods.

## CRUD methods

Crud methods are used to interact with the entity object in the backend or locally.

- `save()`: Save or update the entity into the backend.
- `export()`: Export the entity locally as yaml file.
- `refresh()`: Refresh (read) the entity from the backend.

::: digitalhub_core.entities.run.entity.Run.save
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.run.entity.Run.export
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.run.entity.Run.refresh
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

## Run methods

There are several generic run methods on the `Run` object.

- `wait`: Wait for the run to finish.
- `stop`: Stop the run.

::: digitalhub_core.entities.run.entity.Run.wait
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.run.entity.Run.stop
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

## Kind specific methods

Kind specific methods are used to express potential behaviors of different object kinds.
See the [kinds section](kinds.md) for more information.
