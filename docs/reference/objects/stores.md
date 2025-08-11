# Stores

In SDK there is two types of stores: `Store` and `DataStore`. The first one is used to store/collect files, the second one is used to store/collect dataframes. Every `DataStore` has a `Store` as parent.
We support natively the following stores:

- **`s3`** for MinIO (default store)
- **`local`** for local files
- **`remote`** for remote http/https files
- **`sql`** for PostgreSQL database tables

## Entity paths

When declaring an `Artifact`, a `Dataitem` or a `Model`, you must specify a `path` parameter to declare where the files are stored. There are some rules to follow when specifying the path:

1. The `path` parameter is as `str`.
2. It can have a scheme declared in the beginning of the path.
3. Based on the scheme, the SDK will create a `Store` object to interact with the files/tables in the path.
4. The path can point to a single file, a directory, a partition or a table. See below for more details.

### Supported path types

#### Local paths

A local path is declared by not providing a scheme. For example:

```python
local_dir = "./my-path"
local_file = "./my-path/my-file.csv"
```

#### S3 paths

To declare an S3 path, you need to provide the scheme `s3://`. The first element of the path must be the bucket name and the second must be the key/partition. If you provide a partition path, you need to append a final `/` at the end. For example:

```python
s3_path = "s3://my-bucket/my-key.file"
s3_partition_path = "s3://my-bucket/my-key/"
```

#### Remote paths

To declare a remote path, you need to provide the scheme `http://` or `https://`. For example:

```python
http_path = "http://my-url.csv"
https_path = "https://my-url.csv"
```

#### SQL paths

To declare a SQL path, you need to provide the scheme `sql://`. An SQL path will always point to a single table. The path is composed by three parts, delimited by `/`. The first part is the database name, the second (optional) is the schema and the third is the table name. For example:

```python
sql_path = "sql://my-database/my-schema/my-table"
```
