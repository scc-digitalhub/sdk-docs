# Project methods

The `Project` object comes with two sets of methods: CRUD methods and, according to the SDK digitalhub layer installed, entity specific CRUD methods.

## CRUD methods

Crud methods are used to interact with the entity object in the backend or locally.

- `save()`: Save or update the entity into the backend.
- `export()`: Export the entity locally as yaml file.
- `refresh()`: Refresh (read) the entity from the backend.

::: digitalhub_core.entities.project.entity._base.Project
    options:
        heading_level: 3
        show_bases: false
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        inherited_members: true
        members:
            - save
            - export
            - refresh

## Entity CRUD

The project acts as context for other entities as mentioned in the introduction. With a `Project` object, you can create, read, update and delete these entities. The methods exposed are basically five for all entities, and are:

- **`new`**: create a new entity
- **`get`**: get an entity from backend
- **`update`**: update an entity
- **`delete`**: delete an entity
- **`list`**: list entities related to the project

For `artifacts`, `dataitems` and `models` entities, you have also a `log` method.
Each digitalhub layer exposes CRUD handles different aspects of data and ml entities. Here follows a list of the methods exposed by the `Project` class for each layer. Please refer to the specific entity documentation for more information.

### Core layer

The entity handled by the `Project` class in the core layer (`digitalhub_core`) are:

- **`functions`**
- **`artifacts`**
- **`workflows`**
- **`secrets`**

### Data layer

The entity handled by the `Project` class in the data layer (`digitalhub_data`) are:

- **`dataitems`**

### Ml layer

The entity handled by the `Project` class in the ml layer (`digitalhub_ml`) are:

- **`models`**
