# CRUD

The CRUD methods are used to create, read, update and delete triggers. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.
The syntax is the same for all CRUD methods. If you want to manage triggers from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.
In any case, you need to first import the SDK and instantiate a `Project` object that will be the context in which you can manage entities.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# Use CRUD method on project
trigger = project.new_trigger(name="my-trigger",
                            kind="scheduler",
                            task="my-task",
                            function="my-function")

# Use CRUD method from SDK
trigger = dh.new_trigger(project="my-project",
                        name="my-trigger",
                        kind="scheduler",
                        task="my-task",
                        function="my-function")
```

A `trigger` entity can be managed with the following methods.

Create:

- [**`new_trigger`**](#new)

Read:

- [**`get_trigger`**](#get)
- [**`get_trigger_versions`**](#get-versions)
- [**`import_trigger`**](#import)
- [**`list_triggers`**](#list)

Update:

- [**`update_trigger`**](#update)

Delete:

- [**`delete_trigger`**](#delete)

## Create

You can create a trigger with the `new_trigger()`.
The `kwargs` parameters are determined by the **kind** of the object, and are described in the [kinds section](kinds.md).

### New

This function creates a new entity and saves it into the backend.

::: digitalhub.entities.trigger.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - new_trigger

## Read

To read triggers you can use the `get_trigger()`, `get_trigger_versions()`, `list_triggers()` or `import_trigger()` functions.

### Get

This function searches for a single trigger into the backend.
If you want to collect a trigger from the backend using `get_trigger()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-name>:<entity-id>`.
- The second one is to use the entity name as `identifier`, the project name as `project` and the entity id as `entity_id` parameters. If you do not specify the entity id, you will get the latest version.

::: digitalhub.entities.trigger.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - get_trigger

### Get versions

This function returns all the versions of a trigger from the backend.

::: digitalhub.entities.trigger.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - get_trigger_versions

### List

This function returns all the latest triggers from the backend related to a project.

::: digitalhub.entities.trigger.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - list_triggers

### Import

This function loads the trigger from a local yaml file descriptor.

::: digitalhub.entities.trigger.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - import_trigger

## Update

To update a trigger you can use the `update_trigger()` method.

::: digitalhub.entities.trigger.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - update_trigger

## Delete

To delete a trigger you can use the `delete_trigger()` method.

::: digitalhub.entities.trigger.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - delete_trigger
