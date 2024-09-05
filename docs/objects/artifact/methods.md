# Artifact object

The `Artifact` object comes with three sets of methods: CRUD methods, I/O methods and kind specific methods.

## CRUD methods

Crud methods are used to interact with the entity object in the backend or locally.

- `save()`: Save or update the entity into the backend.
- `export()`: Export the entity locally as yaml file.
- `refresh()`: Refresh (read) the entity from the backend.

::: digitalhub_core.entities.artifact.entity._base.Artifact.save
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.artifact.entity._base.Artifact.export
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.artifact.entity._base.Artifact.refresh
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

## I/O methods

I/O methods are used to handle objects as files.

- `as_file()`: Dowloads the artifact into a local temporary destination
- `download()`: Downloads the artifact into a specified path
- `upload()`: Uploads the artifact to artifact spec path

::: digitalhub_core.entities.artifact.entity._base.Artifact.as_file
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.artifact.entity._base.Artifact.download
    options:
        heading_level: 3
        show_signature: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub_core.entities.artifact.entity._base.Artifact.upload
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