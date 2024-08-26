# CRUD

The CRUD methods are used to create, read, update and delete secrets. There are two ways to use them.
The first is through the SDK and the second is through the `Project` object.

Example:

```python
import digitalhub as dh

project = dh.get_or_create_project("my-project")

# From library
secret = dh.new_secret(project="my-project",
                       name="my-secret",
                       secret_value="my-secret-value")

# From project
secret = project.new_secret(name="my-secret",
                            secret_value="my-secret-value")
```

The syntax is the same for all CRUD methods. If you want to manage secrets from the project, you can use the `Project` object and avoid to specify the `project` parameter. In this last case, you need to specify every parameter as keyword argument.

A `secret` entity can be managed with the following methods.

Create:

- [**`new_secret`**](#new)

Read:

- [**`get_secret`**](#get)
- [**`get_secret_versions`**](#get-versions)
- [**`import_secret`**](#import)
- [**`list_secrets`**](#list)

Update:

- [**`update_secret`**](#update)

Delete:

- [**`delete_secret`**](#delete)

## Create

You can create a secret with the `new_secret()` or with `log_secret()` method.

### New

This function create a new entity and saves it into the backend.

::: digitalhub_core.entities.secret.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - new_secret

Example:

```python
secret = dh.new_secret(project="my-project",
                       name="my-secret",
                       secret_value="my-secret-value")
```

## Read

To read secrets you can use the `get_secret()`, `get_secret_versions()`, `list_secrets()` or `import_secret()` functions.

### Get

This function searches for a single secret into the backend.
If you want to collect a secret from the backend using `get_secret()`, you have two options:

- The first one is to use the `key` parameter which has the pattern `store://<project-name>/<entity-type>/<entity-kind>/<entity-name>:<entity-id>`.
- The second one is to use the entity name as `identifier`, the project name as `project` and the entity id as `entity_id` parameters. If you do not specify the entity id, you will get the latest version.

::: digitalhub_core.entities.secret.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_secret

Example:

```python
# Use key
secret = dh.get_secret("store://my-secret-key")

# Use name, project and id
secret = dh.get_secret("secret-name",
                       project="my-project",
                       entity_id="some-uuid4")
```

### Get versions

This function returns all the versions of a secret from the backend.

::: digitalhub_core.entities.secret.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - get_secret_versions

Example:

```python
# Use key
secrets = dh.get_secret_versions("store://my-secret-key")

# Use name, project and id
secrets = dh.get_secret_versions("secret-name",
                                 project="my-project")
```

### List

This function returns all the latest secrets from the backend related to a project.

::: digitalhub_core.entities.secret.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - list_secrets

Example:

```python
secrets = dh.list_secrets(project="my-project")
```

### Import

This function load the secret from a local yaml file descriptor.

::: digitalhub_core.entities.secret.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - import_secret

Example:

```python
secret = dh.import_secret(file="./some-path/my-secret.yaml")
```

## Update

To update a secret you can use the `update_secret()` method.

::: digitalhub_core.entities.secret.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - update_secret

Example:

```python
secret = dh.new_secret(project="my-project",
                       name="my-secret",
                       secret_value="my-secret-value")

secret.metadata.description = "My new description"

secret = dh.update_secret(secret=secret)
```

## Delete

To delete a secret you can use the `delete_secret()` method.

::: digitalhub_core.entities.secret.crud
    options:
        heading_level: 6
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - delete_secret

Example:

```python
dh.delete_secret("store://my-secret-key")
```
