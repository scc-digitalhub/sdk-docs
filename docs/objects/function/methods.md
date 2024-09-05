# Function object

The `Function` object comes with three sets of methods: CRUD methods, one run method and (eventual) kind specific methods.

## CRUD methods

Crud methods are used to interact with the entity object in the backend or locally.

- `save()`: Save or update the entity into the backend.
- `export()`: Export the entity locally as yaml file.
- `refresh()`: Refresh (read) the entity from the backend.

::: digitalhub_core.entities.function.entity.Function.save
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.function.entity.Function.export
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.function.entity.Function.refresh
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

## Run method

The `run()` method is used to execute the function.

::: digitalhub_core.entities.function.entity.Function.run
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

## Kind specific methods

Kind specific methods are used to express potential behaviors of different object kinds.
See the [kinds section](kinds.md) for more information.
