# Secret object

The `Secret` object comes with two sets of methods: CRUD methods and read/write methods.

## CRUD methods

Crud methods are used to interact with the entity object in the backend or locally.

- `save()`: Save or update the entity into the backend.
- `export()`: Export the entity locally as yaml file.
- `refresh()`: Refresh (read) the entity from the backend.

::: digitalhub_core.entities.secret.entity.Secret
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

- `set_secret_value()`: Set (update) the secret value
- `read_secret_value()`: Read the secret value

::: digitalhub_core.entities.secret.entity.Secret
    options:
        heading_level: 3
        show_bases: false
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        inherited_members: true
        members:
            - set_secret_value
            - read_secret_value
