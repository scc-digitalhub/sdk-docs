# Dataitem kinds

At the moment, we support the following kinds:

- **`table`**: represents a table

For each different kind, the `Dataitem` object has its own subclass with different `spec` and `status` attributes.

## Table

The `table` kind indicates that the dataitem is a generic table. It's usefull if you intend to manipulate the dataitem as a dataframe, infact it has some methods to do so. The default dataframe framework we use to represent a table as dataframe is `pandas`.

### Table spec parameters

The `table` kind has the following `spec` parameters to pass to the `new_dataitem()` method:

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| [`path`](../stores.md#entity-paths) | *str* | Path of the dataitem, can be a local path or a remote path, a single filepath or a directory/partition. | *required* |
| `schema` | [*TableSchema*](https://specs.frictionlessdata.io/table-schema/) | Frictionless table schema | `None` |

### Table methods

The `table` kind has the following additional methods:

::: digitalhub_data.entities.dataitem.entity.table
    options:
        heading_level: 6
        show_bases: false
        show_signature: false
        show_docstring_description: false
        show_symbol_type_heading: true
        members:
            - DataitemTable
