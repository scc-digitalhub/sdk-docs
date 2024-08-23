# Workflow object

The `Workflow` object comes with three sets of methods: CRUD methods, one run method and (eventual) kind specific methods.

## CRUD methods

Crud methods are used to interact with the entity object in the backend or locally.

- `save()`: Save or update the entity into the backend.
- `export()`: Export the entity locally as yaml file.
- `refresh()`: Refresh (read) the entity from the backend.

::: digitalhub_core.entities.workflow.entity.Workflow
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

## Run method

The `run()` method is used to execute the workflow.

::: digitalhub_core.entities.workflow.entity.Workflow
    options:
        heading_level: 3
        show_bases: false
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        inherited_members: true
        members:
            - run

## Kind specific methods

Kind specific methods are used to express potential behaviors of different object kinds.
See the [kinds section](kinds.md) for more information.
