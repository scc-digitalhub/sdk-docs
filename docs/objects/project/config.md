# Config

The **`config`** parameter can be used to provide a dictionary containing the project configuration like user and password for basic auth or a bearer token. The format of the dictionary for basic auth must be as this:

```python
{
    "user": "user",
    "password": "password"
}
```

The format of the dictionary for bearer token must be as this:

```python
{
    "access_token": "token"
}
```

In case you try to get a project without from the backend with invalid credentials, an exception will be raised.
Because the backend client is a Singleton object, it will autoconfigure credentials at startup, so the only way to setup proper credentials once it fails to connect is to use the SDK method `set_dhub_env()`.
The method accepts the following optional parameters:

- **`endpoint`**: the endpoint of the backend
- **`user`**: the user for basic auth
- **`password`**: the password for basic auth
- **`token`**: the auth token

Example:

```python
dh.set_dhub_env(
    endpoint="https://some-digitalhub:8080",
    token="token"
)
```

Note that the `set_dhub_env()` method ovverrides the environment variables and (if already instantiated) the credentials attributes of the backend client.
