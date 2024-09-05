# Project methods

The `Project` object comes with two sets of methods: CRUD methods and, according to the SDK digitalhub layer installed, entity specific CRUD methods.

## CRUD methods

Crud methods are used to interact with the entity object in the backend or locally.

- `save()`: Save or update the entity into the backend.
- `export()`: Export the entity locally as yaml file.
- `refresh()`: Refresh (read) the entity from the backend.

::: digitalhub_core.entities.project.entity._base.Project.save
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.project.entity._base.Project.export
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.project.entity._base.Project.refresh
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

## Entity CRUD

The project acts as context for other entities as mentioned in the introduction. With a `Project` object, you can create, read, update and delete these entities. The methods exposed are basically the same as the CRUD entities, the only difference is that on the project object you omit the project name as parameter. The available methods and are:

- **`new`**: create a new entity
- **`log`**: create and upload an entity (only for `artifacts`, `dataitems` and `models`)
- **`get`**: get an entity from backend
- **`get_versions`**: get all version for a named entity
- **`list`**: list entities related to the project
- **`import`**: import an entity
- **`update`**: update an entity
- **`delete`**: delete an entity

## Layers

Each digitalhub layer exposes CRUD handles different aspects of dataops and mlops. Here follows a list of the methods exposed by the `Project` class according to each layer. Please refer to the specific entity documentation for more information.

### Core layer

The entity handled by the `Project` class in the core layer (`digitalhub_core`) are:

- [**`artifacts`**](../artifact/crud.md)
- [**`functions`**](../function/crud.md)
- [**`workflows`**](../workflow/crud.md)
- [**`secrets`**](../secret/crud.md)

### Data layer

The entity handled by the `Project` class in the data layer (`digitalhub_data`) are:

- [**`dataitems`**](../dataitem/crud.md)

### Ml layer

The entity handled by the `Project` class in the ml layer (`digitalhub_ml`) are:

- [**`models`**](../model/crud.md)
