# Config

The **`config`** parameter can be used to provide a dictionary containing the project configuration like user and password for basic auth or a bearer token.

## Basic auth

The format of the dictionary for basic auth must be as this:

```python
{
    "user": "user",
    "password": "password"
}
```

## OAuth token

The format of the dictionary for bearer token must be as this:

```python
{
    "client_id": "id",
    "access_token": "token",
    "refresh_token": "token"
}
```

## Set manually credentials

In case you try to get a project without from the backend with invalid credentials, an exception will be raised.
Because the backend client is a Singleton object, it will autoconfigure credentials at startup, so the only way to setup proper credentials once it fails to connect is to use the SDK method `set_dhcore_env()`.

::: digitalhub.client.dhcore.utils.set_dhcore_env
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

Example:

```python
dh.set_dhcore_env(
    endpoint="https://some-digitalhub:8080",
    access_token="token"
)
```

Note that the `set_dhcore_env()` method ovverrides the environment variables and (if already instantiated) the credentials attributes of the backend client.
