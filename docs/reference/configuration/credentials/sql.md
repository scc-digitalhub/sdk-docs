# SQL credentials

The SDK reads SQL database credentials from environment variables or from the `.dhcore.ini` configuration file. Required values are username, password, host, port and database; `DB_PLATFORM` is optional and can be used to select a specific DB adapter.

## Important fields

- DB_HOST — database host or socket
- DB_PORT — database port
- DB_USERNAME — database user
- DB_PASSWORD — database password
- DB_DATABASE — database name
- DB_PLATFORM — optional platform hint (e.g., `postgres`, `mysql`)
- DB_SCHEMA — optional Postgres schema name

## Environment variables

Set the variables in your shell or CI environment. Example:

```bash
export DB_HOST=postgres.example.com
export DB_PORT=5432
export DB_USERNAME=myuser
export DB_PASSWORD=s3cr3t
export DB_DATABASE=mydb
export DB_PLATFORM=postgres
export DB_SCHEMA=public
```

## Configuration file (.dhcore.ini)

You can also place credentials in the `.dhcore.ini` file under a profile. Example format:

```text
[__default]
DB_HOST = postgres.example.com
DB_PORT = 5432
DB_USERNAME = myuser
DB_PASSWORD = s3cr3t
DB_DATABASE = mydb
DB_PLATFORM = postgres
DB_SCHEMA = public
```

## Notes

- The configurator checks environment variables first; if required vars are missing it falls back to `.dhcore.ini`.
