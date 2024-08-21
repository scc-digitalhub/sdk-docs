# CRUD

The CRUD methods are used to create, read, update and delete dataitems. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# From library
dataitem = dh.new_dataitem(project="my-project",
                           name="my-dataitem",
                           kind="dataitem",
                           path="s3://my-bucket/my-dataitem.ext")

# From project
dataitem = project.new_dataitem(name="my-dataitem",
                                kind="dataitem",
                                path="s3://my-bucket/my-dataitem.ext")
```

The syntax is the same for all CRUD methods. If you want to manage dataitems from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.

A `dataitem` entity can be managed with the following methods.

Create:

- [**`new_dataitem`**](#new)
- [**`log_dataitem`**](#log)

Read:

- [**`get_dataitem`**](#get)
- [**`get_dataitem_versions`**](#get-versions)
- [**`import_dataitem`**](#import)
- [**`list_dataitems`**](#list)

Update:

- [**`update_dataitem`**](#update)

Delete:

- [**`delete_dataitem`**](#delete)

## Create

You can create a dataitem with the `new_dataitem()` or with `log_dataitem()` method.

### New

This function create a new entity and saves it into the backend.

::: digitalhub_data.entities.dataitem.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - new_dataitem

Example:

```python
dataitem = dh.new_dataitem(project="my-project",
                           name="my-dataitem",
                           kind="dataitem",
                           path="s3://my-bucket/my-dataitem.ext")
```

### Log

This function create a new entity into the backend and also upload a local file into a dataitem store (eg. *S3*).

::: digitalhub_data.entities.dataitem.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - log_dataitem

Example:

```python
dataitem = dh.log_dataitem(project="my-project",
                           name="my-dataitem",
                           kind="dataitem",
                           source="./my-dataitem.ext")
```

## Read

To read dataitems you can use the `get_dataitem()`, `get_dataitem_versions()`, `list_dataitems()` or `import_dataitem()` functions.

### Get

This function searches for a single dataitem into the backend.
If you want to collect a dataitem from the backend using `get_dataitem()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-name>:<entity-id>`.
- The second one is to use the entity name as `identifier`, the project name as `project` and the entity id as `entity_id` parameters. If you do not specify the entity id, you will get the latest version.

::: digitalhub_data.entities.dataitem.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_dataitem

Example:

```python
# Use key
dataitem = dh.get_dataitem("store://my-dataitem-key")

# Use name, project and id
dataitem = dh.get_dataitem("dataitem-name",
                           project="my-project",
                           entity_id="some-uuid4")
```

### Get versions

This function returns all the versions of a dataitem from the backend.

::: digitalhub_data.entities.dataitem.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_dataitem_versions

Example:

```python
# Use key
dataitems = dh.get_dataitem_versions("store://my-dataitem-key")

# Use name, project and id
dataitems = dh.get_dataitem_versions("dataitem-name",
                                     project="my-project")
```

### List

This function returns all the latest dataitems from the backend related to a project.

::: digitalhub_data.entities.dataitem.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - list_dataitems

Example:

```python
dataitems = dh.list_dataitems(project="my-project")
```

### Import

This function load the dataitem from a local yaml file descriptor.

::: digitalhub_data.entities.dataitem.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - import_dataitem

Example:

```python
dataitem = dh.import_dataitem(file="./some-path/my-dataitem.yaml")
```

## Update

To update a dataitem you can use the `update_dataitem()` method.

::: digitalhub_data.entities.dataitem.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - update_dataitem

Example:

```python
dataitem = dh.new_dataitem(project="my-project",
                           name="my-dataitem",
                           kind="dataitem",
                           path="s3://my-bucket/my-dataitem.ext")

dataitem.metadata.description = "My new description"

dataitem = dh.update_dataitem(dataitem=dataitem)
```

## Delete

To delete a dataitem you can use the `delete_dataitem()` method.

::: digitalhub_data.entities.dataitem.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - delete_dataitem

Example:

```python
dh.delete_dataitem("store://my-dataitem-key")
```
