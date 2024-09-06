# CRUD

The CRUD methods are used to create, read, update and delete artifacts. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.
The syntax is the same for all CRUD methods. If you want to manage artifacts from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.

An `artifact` entity can be managed with the following methods.

Create:

- [**`new_artifact`**](#new)
- [**`log_artifact`**](#log)

Read:

- [**`get_artifact`**](#get)
- [**`get_artifact_versions`**](#get-versions)
- [**`import_artifact`**](#import)
- [**`list_artifacts`**](#list)

Update:

- [**`update_artifact`**](#update)

Delete:

- [**`delete_artifact`**](#delete)

## Create

You can create an artifact with the `new_artifact()` or with `log_artifact()` method.
The `kwargs` parameters are determined by the **kind** of the object, and are described in the [kinds section](kinds.md).
The `kwargs` parameters are the same for both *new* and *log* methods.

### New

This function create a new entity and saves it into the backend.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - new_artifact

### Log

This function create a new entity into the backend and also upload a local file into an artifact store (eg. *S3*).

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - log_artifact

## Read

To read artifacts you can use the `get_artifact()`, `get_artifact_versions()`, `list_artifacts()` or `import_artifact()` functions.

### Get

This function searches for a single artifact into the backend.
If you want to collect an artifact from the backend using `get_artifact()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-name>:<entity-id>`.
- The second one is to use the entity name as `identifier`, the project name as `project` and the entity id as `entity_id` parameters. If you do not specify the entity id, you will get the latest version.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - get_artifact

### Get versions

This function returns all the versions of an artifact from the backend.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - get_artifact_versions

### List

This function returns all the latest artifacts from the backend related to a project.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - list_artifacts

### Import

This function load the artifact from a local yaml file descriptor.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - import_artifact

## Update

To update an artifact you can use the `update_artifact()` method.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - update_artifact

## Delete

To delete an artifact you can use the `delete_artifact()` method.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        show_source: false
        members:
            - delete_artifact
