# CRUD

The CRUD methods are used to create, read, update and delete workflows. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# From library
workflow = dh.new_workflow(project="my-project",
                           name="my-workflow",
                           kind="workflow-kind")

# From project
workflow = project.new_workflow(name="my-workflow",
                                kind="workflow-kind")
```

The syntax is the same for all CRUD methods. If you want to manage workflows from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.

A `workflow` entity can be managed with the following methods.

Create:

- [**`new_workflow`**](#new)
- [**`log_workflow`**](#log)

Read:

- [**`get_workflow`**](#get)
- [**`get_workflow_versions`**](#get-versions)
- [**`import_workflow`**](#import)
- [**`list_workflows`**](#list)

Update:

- [**`update_workflow`**](#update)

Delete:

- [**`delete_workflow`**](#delete)

## Create

You can create a workflow with the `new_workflow()` or with `log_workflow()` method.

### New

This function create a new entity and saves it into the backend.

::: digitalhub_core.entities.workflow.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - new_workflow

Example:

```python
workflow = dh.new_workflow(project="my-project",
                           name="my-workflow",
                           kind="workflow-kind")
```

### Log

This function create a new entity into the backend and also upload a local file into a workflow store (eg. *S3*).

::: digitalhub_core.entities.workflow.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - log_workflow

Example:

```python
workflow = dh.log_workflow(project="my-project",
                           name="my-workflow",
                           kind="workflow-kind")
```

## Read

To read workflows you can use the `get_workflow()`, `get_workflow_versions()`, `list_workflows()` or `import_workflow()` functions.

### Get

This function searches for a single workflow into the backend.
If you want to collect a workflow from the backend using `get_workflow()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-name>:<entity-id>`.
- The second one is to use the entity name as `identifier`, the project name as `project` and the entity id as `entity_id` parameters. If you do not specify the entity id, you will get the latest version.

::: digitalhub_core.entities.workflow.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_workflow

Example:

```python
# Use key
workflow = dh.get_workflow("store://my-workflow-key")

# Use name, project and id
workflow = dh.get_workflow("workflow-name",
                           project="my-project",
                           entity_id="some-uuid4")
```

### Get versions

This function returns all the versions of a workflow from the backend.

::: digitalhub_core.entities.workflow.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_workflow_versions

Example:

```python
# Use key
workflows = dh.get_workflow_versions("store://my-workflow-key")

# Use name, project and id
workflows = dh.get_workflow_versions("workflow-name",
                                     project="my-project")
```

### List

This function returns all the latest workflows from the backend related to a project.

::: digitalhub_core.entities.workflow.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - list_workflows

Example:

```python
workflows = dh.list_workflows(project="my-project")
```

### Import

This function load the workflow from a local yaml file descriptor.

::: digitalhub_core.entities.workflow.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - import_workflow

Example:

```python
workflow = dh.import_workflow(file="./some-path/my-workflow.yaml")
```

## Update

To update a workflow you can use the `update_workflow()` method.

::: digitalhub_core.entities.workflow.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - update_workflow

Example:

```python
workflow = dh.new_workflow(project="my-project",
                           name="my-workflow",
                           kind="workflow-kind")

workflow.metadata.description = "My new description"

workflow = dh.update_workflow(workflow=workflow)
```

## Delete

To delete a workflow you can use the `delete_workflow()` method.

::: digitalhub_core.entities.workflow.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - delete_workflow

Example:

```python
dh.delete_workflow("store://my-workflow-key")
```
