# CRUD

The CRUD methods are used to create, read, update and delete projects.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")
```

A `project` entity can be managed with the following methods.

Create:

- [**`new_project`**](#new)

Read:

- [**`get_project`**](#get)
- [**`import_project`**](#import)
- [**`load_project`**](#load)

Read or create:

- [**`get_or_create_project`**](#read-or-create)

Update:

- [**`update_project`**](#update)

Delete:

- [**`delete_project`**](#delete)

For project configuration options, please refer to the [Config](config.md) section, and the [Setup](setup.md) section.

## Create

You can create a project with the `new_project()` or with `log_project()` method.

### New

This function create a new entity and saves it into the backend.

::: digitalhub_core.entities.project.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - new_project

Example:

```python
project = dh.new_project(name="my-project",)
```

## Read

To read projects you can use the `get_project()`, `import_project()` or `load_project()`.

### Get

This function searches for a single project into the backend.

::: digitalhub_core.entities.project.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_project

Example:

```python
project = dh.get_project("project-name")
```

### Import

This function load the project from a local yaml file descriptor.

::: digitalhub_core.entities.project.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - import_project

Example:

```python
project = dh.import_project(file="./some-path/my-project.yaml")
```

### Load

This function returns a projects from the backend or from a local file according to the parameter passed to the function. If the parameter is `name`, the function will try to load the project from the backend. If the parameter is `file`, the function will try to load the project from the local file.

::: digitalhub_core.entities.project.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - load_project

Example:

```python
project = dh.load_project(name="my-project")

project = dh.load_project(file="./some-path/my-project.yaml")
```

## Read or create

You can read or create a project with the `get_or_create_project()` method.

::: digitalhub_core.entities.project.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_or_create_project

Example:

```python
project = dh.get_or_create_project("my-project")
```

## Update

To update a project you can use the `update_project()` method.

::: digitalhub_core.entities.project.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - update_project

Example:

```python
project = dh.new_project(name="my-project")

project.metadata.description = "My new description"

project = dh.update_project(name=project)
```

## Delete

To delete a project you can use the `delete_project()` method.

::: digitalhub_core.entities.project.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - delete_project

Example:

```python
dh.delete_project("store://my-project-key")
```
