# CRUD

The CRUD methods are used to create, read, update and delete models. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# From library
model = dh.new_model(project="my-project",
                     name="my-model",
                     kind="model",
                     path="s3://my-bucket/my-model.ext")

# From project
model = project.new_model(name="my-model",
                          kind="model",
                          path="s3://my-bucket/my-model.ext")
```

The syntax is the same for all CRUD methods. If you want to manage models from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.

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

### New

This function create a new entity and saves it into the backend.

::: digitalhub_ml.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - new_model

Example:

```python
model = dh.new_model(project="my-project",
                     name="my-model",
                     kind="model",
                     path="s3://my-bucket/my-model.ext")
```

### Log

This function create a new entity into the backend and also upload a local file into a model store (eg. *S3*).

::: digitalhub_ml.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - log_model

Example:

```python
model = dh.log_model(project="my-project",
                     name="my-model",
                     kind="model",
                     source="./my-model.ext")
```

## Read

To read models you can use the `get_model()`, `get_model_versions()`, `list_models()` or `import_model()` functions.

### Get

This function searches for a single model into the backend.
If you want to collect a model from the backend using `get_model()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-name>:<entity-id>`.
- The second one is to use the entity name as `identifier`, the project name as `project` and the entity id as `entity_id` parameters. If you do not specify the entity id, you will get the latest version.

::: digitalhub_ml.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_model

Example:

```python
# Use key
model = dh.get_model("store://my-model-key")

# Use name, project and id
model = dh.get_model("model-name",
                     project="my-project",
                     entity_id="some-uuid4")
```

### Get versions

This function returns all the versions of a model from the backend.

::: digitalhub_ml.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_model_versions

Example:

```python
# Use key
models = dh.get_model_versions("store://my-model-key")

# Use name, project and id
models = dh.get_model_versions("model-name",
                               project="my-project")
```

### List

This function returns all the latest models from the backend related to a project.

::: digitalhub_ml.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - list_models

Example:

```python
models = dh.list_models(project="my-project")
```

### Import

This function load the model from a local yaml file descriptor.

::: digitalhub_ml.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - import_model

Example:

```python
model = dh.import_model(file="./some-path/my-model.yaml")
```

## Update

To update a model you can use the `update_model()` method.

::: digitalhub_ml.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - update_model

Example:

```python
model = dh.new_model(project="my-project",
                     name="my-model",
                     kind="model",
                     path="s3://my-bucket/my-model.ext")

model.metadata.description = "My new description"

model = dh.update_model(model=model)
```

## Delete

To delete a model you can use the `delete_model()` method.

::: digitalhub_ml.entities.model.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - delete_model

Example:

```python
dh.delete_model("store://my-model-key")
```
