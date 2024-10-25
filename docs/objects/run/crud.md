# CRUD

The CRUD methods are used to create, read, update and delete runs. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.
The syntax is the same for all CRUD methods. If you want to manage runs from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.
In any case, you need to first import the SDK and instantiate a `Project` object that will be the context in which you can manage entities.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# Use CRUD method on project

run = project.new_run(kind="python+run",
                      task="task-string")

# Use CRUD method from SDK

run = dh.new_run(project="my-project",
                 kind="python+run",
                 task="task-string")
```

A `run` entity can be managed with the following methods.

Create:

- [**`new_run`**](#new)

Read:

- [**`get_run`**](#get)
- [**`import_run`**](#import)
- [**`list_runs`**](#list)

Update:

- [**`update_run`**](#update)

Delete:

- [**`delete_run`**](#delete)

## Create

You can create a run with the `new_run()`.
The `kwargs` parameters are determined by the **kind** of the object, and are described in the [kinds section](kinds.md).

### New

This run create a new entity and saves it into the backend.

::: digitalhub.entities.run.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - new_run

## Read

To read runs you can use the `get_run()`, `list_runs()` or `import_run()` runs.

### Get

This run searches for a single run into the backend.
If you want to collect a run from the backend using `get_run()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-id>`.
- The second one is to use the entity id as `identifier`, the project name as `project`.

::: digitalhub.entities.run.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - get_run

### List

This run returns all the latest runs from the backend related to a project.

::: digitalhub.entities.run.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - list_runs

### Import

This run load the run from a local yaml file descriptor.

::: digitalhub.entities.run.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - import_run

## Update

To update a run you can use the `update_run()` method.

::: digitalhub.entities.run.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - update_run

## Delete

To delete a run you can use the `delete_run()` method.

::: digitalhub.entities.run.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - delete_run
