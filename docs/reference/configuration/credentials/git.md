# Credentials

There are two common ways to provide credentials for private Git repositories:

## Environment variables (local executions)

- Export the token into the process environment before creating the function. This is the simplest approach and works well for local executions.

```python
import os
import digitalhub as dh

os.environ['GIT_TOKEN'] = 'ghp_...'

func = dh.new_function(
    name='f',
    kind='python',
    code_src='git+https://github.com/my/repo',
    handler='src.app:run',
)
```

Notes: this exposes the token to the process environment; prefer scoped tokens and avoid committing them.

## DigitalHub Secret (remote executions)

- Create a `Secret` entity and store the token securely in the platform. Attach it to the function or job that requires it.

Example — create and set a secret value:

```python
import digitalhub as dh

project = dh.get_or_create_project('my-project')
# create with value
secret_token = project.new_secret(name='git-token', secret_value='ghp_...')
# or, for user and password
secret_user = project.new_secret(name='git-user', secret_value='my-username')
secret_password = project.new_secret(name='git-password', secret_value='my-password')
```

Example — attach the secret to a run/build so the runtime injects it:

```python
# create function (no token in env)
func = dh.new_function(name='f', kind='python', code_src='git+https://github.com/my/repo', handler='src.app:run')

# run a build or job and provide the secret name
run = func.run(action='build', secrets=['git-token'])
```
