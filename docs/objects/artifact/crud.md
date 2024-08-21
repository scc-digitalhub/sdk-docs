# CRUD

The CRUD methods are used to create, read, update and delete artifacts. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# From library
artifact = dh.new_artifact(project="my-project",
                           name="my-artifact",
                           kind="artifact",
                           path="s3://my-bucket/my-artifact.ext")

# From project
artifact = project.new_artifact(name="my-artifact",
                                kind="artifact",
                                path="s3://my-bucket/my-artifact.ext")
```

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

### New

This function create a new entity and saves it into the backend.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - new_artifact

Example:

```python
artifact = dh.new_artifact(project="my-project",
                           name="my-artifact",
                           kind="artifact",
                           path="s3://my-bucket/my-artifact.ext")
```

### Log

This function create a new entity into the backend and also upload a local file into an artifact store (eg. *S3*).

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - log_artifact

Example:

```python
artifact = dh.log_artifact(project="my-project",
                           name="my-artifact",
                           kind="artifact",
                           source="./my-artifact.ext")
```

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
        members:
            - get_artifact

Example:

```python
# Use key
artifact = dh.get_artifact("store://my-artifact-key")

# Use name, project and id
artifact = dh.get_artifact("artifact-name",
                           project="my-project",
                           entity_id="some-uuid4")
```

### Get versions

This function returns all the versions of an artifact from the backend.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_artifact_versions

Example:

```python
# Use key
artifacts = dh.get_artifact_versions("store://my-artifact-key")

# Use name, project and id
artifacts = dh.get_artifact_versions("artifact-name",
                                     project="my-project")
```

### List

This function returns all the latest artifacts from the backend related to a project.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - list_artifacts

Example:

```python
artifacts = dh.list_artifacts(project="my-project")
```

### Import

This function load the artifact from a local yaml file descriptor.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - import_artifact

Example:

```python
artifact = dh.import_artifact(file="./some-path/my-artifact.yaml")
```

## Update

To update an artifact you can use the `update_artifact()` method.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - update_artifact

Example:

```python
artifact = dh.new_artifact(project="my-project",
                           name="my-artifact",
                           kind="artifact",
                           path="s3://my-bucket/my-artifact.ext")

artifact.metadata.description = "My new description"

artifact = dh.update_artifact(artifact=artifact)
```

## Delete

To delete an artifact you can use the `delete_artifact()` method.

::: digitalhub_core.entities.artifact.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - delete_artifact

Example:

```python
dh.delete_artifact("store://my-artifact-key")
```
