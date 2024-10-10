# CRUD

The CRUD methods are used to create, read, update and delete models. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.
The syntax is the same for all CRUD methods. If you want to manage models from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.
In any case, you need to first import the SDK and instantiate a `Project` object that will be the context in which you can manage entities.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# Use CRUD method on project

model = project.new_model(name="my-model",
                          kind="model",
                          path="path-to-some-model")

# Use CRUD method from SDK

model = dh.new_model(project="my-project",
                     name="my-model",
                     kind="model",
                     path="path-to-some-model")
```

A `model` entity can be managed with the following methods.

Create:

- [**`new_model`**](#new)
- [**`log_model`**](#log)

Read:

- [**`get_model`**](#get)
- [**`get_model_versions`**](#get-versions)
- [**`import_model`**](#import)
- [**`list_models`**](#list)

Update:

- [**`update_model`**](#update)

Delete:

- [**`delete_model`**](#delete)

## Create

You can create a model with the `new_model()` or with `log_model()` method.
The `kwargs` parameters are determined by the **kind** of the object, and are described in the [kinds section](kinds.md).
The `kwargs` parameters are the same for both *new* and *log* methods.

### New

This function create a new entity and saves it into the backend.

::: digitalhub.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - new_model

### Log

This function create a new entity into the backend and also upload a local file into a model store (eg. *S3*).

::: digitalhub.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - log_model

## Read

To read models you can use the `get_model()`, `get_model_versions()`, `list_models()` or `import_model()` functions.

### Get

This function searches for a single model into the backend.
If you want to collect a model from the backend using `get_model()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-name>:<entity-id>`.
- The second one is to use the entity name as `identifier`, the project name as `project` and the entity id as `entity_id` parameters. If you do not specify the entity id, you will get the latest version.

::: digitalhub.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - get_model

### Get versions

This function returns all the versions of a model from the backend.

::: digitalhub.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - get_model_versions

### List

This function returns all the latest models from the backend related to a project.

::: digitalhub.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - list_models

### Import

This function load the model from a local yaml file descriptor.

::: digitalhub.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - import_model

## Update

To update a model you can use the `update_model()` method.

::: digitalhub.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - update_model

## Delete

To delete a model you can use the `delete_model()` method.

::: digitalhub.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - delete_model
