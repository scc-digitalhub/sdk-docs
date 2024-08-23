# CRUD

The CRUD methods are used to create, read, update and delete functions. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# From library
function = dh.new_function(project="my-project",
                           name="my-function",
                           kind="function-kind")

# From project
function = project.new_function(name="my-function",
                                kind="function-kind")
```

The syntax is the same for all CRUD methods. If you want to manage functions from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.

A `function` entity can be managed with the following methods.

Create:

- [**`new_function`**](#new)
- [**`log_function`**](#log)

Read:

- [**`get_function`**](#get)
- [**`get_function_versions`**](#get-versions)
- [**`import_function`**](#import)
- [**`list_functions`**](#list)

Update:

- [**`update_function`**](#update)

Delete:

- [**`delete_function`**](#delete)

## Create

You can create a function with the `new_function()` or with `log_function()` method.

### New

This function create a new entity and saves it into the backend.

::: digitalhub_core.entities.function.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - new_function

Example:

```python
function = dh.new_function(project="my-project",
                           name="my-function",
                           kind="function-kind")
```

### Log

This function create a new entity into the backend and also upload a local file into a function store (eg. *S3*).

::: digitalhub_core.entities.function.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - log_function

Example:

```python
function = dh.log_function(project="my-project",
                           name="my-function",
                           kind="function-kind")
```

## Read

To read functions you can use the `get_function()`, `get_function_versions()`, `list_functions()` or `import_function()` functions.

### Get

This function searches for a single function into the backend.
If you want to collect a function from the backend using `get_function()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-name>:<entity-id>`.
- The second one is to use the entity name as `identifier`, the project name as `project` and the entity id as `entity_id` parameters. If you do not specify the entity id, you will get the latest version.

::: digitalhub_core.entities.function.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_function

Example:

```python
# Use key
function = dh.get_function("store://my-function-key")

# Use name, project and id
function = dh.get_function("function-name",
                           project="my-project",
                           entity_id="some-uuid4")
```

### Get versions

This function returns all the versions of a function from the backend.

::: digitalhub_core.entities.function.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_function_versions

Example:

```python
# Use key
functions = dh.get_function_versions("store://my-function-key")

# Use name, project and id
functions = dh.get_function_versions("function-name",
                                     project="my-project")
```

### List

This function returns all the latest functions from the backend related to a project.

::: digitalhub_core.entities.function.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - list_functions

Example:

```python
functions = dh.list_functions(project="my-project")
```

### Import

This function load the function from a local yaml file descriptor.

::: digitalhub_core.entities.function.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - import_function

Example:

```python
function = dh.import_function(file="./some-path/my-function.yaml")
```

## Update

To update a function you can use the `update_function()` method.

::: digitalhub_core.entities.function.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - update_function

Example:

```python
function = dh.new_function(project="my-project",
                           name="my-function",
                           kind="function-kind")

function.metadata.description = "My new description"

function = dh.update_function(function=function)
```

## Delete

To delete a function you can use the `delete_function()` method.

::: digitalhub_core.entities.function.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - delete_function

Example:

```python
dh.delete_function("store://my-function-key")
```
