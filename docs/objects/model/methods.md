# Model object

The `Model` object comes with three sets of methods: CRUD methods, I/O methods and kind specific methods.

## CRUD methods

Crud methods are used to interact with the entity object in the backend or locally.

- `save()`: Save or update the entity into the backend.
- `export()`: Export the entity locally as yaml file.
- `refresh()`: Refresh (read) the entity from the backend.

::: digitalhub_ml.entities.model.entity._base.Model
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

## I/O methods

I/O methods are used to handle objects as files.

- `as_file()`: Dowloads the model into a local temporary destination
- `download()`: Downloads the model into a specified path
- `upload()`: Uploads the model to model spec path

::: digitalhub_ml.entities.model.entity._base.Model
    options:
        heading_level: 3
        show_bases: false
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        inherited_members: true
        members:
            - as_file
            - download
            - upload

## Kind specific methods

Kind specific methods are used to express potential behaviors of different object kinds.
See the [kinds section](kinds.md) for more information.