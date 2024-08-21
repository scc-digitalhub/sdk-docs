# Artifact kinds

At the moment, we support the following kinds:

- **`artifact`**: represents a generic artifact

For each different kind, the `Artifact` object has its own subclass with different `spec` and `status` attributes.

## Artifact

The `artifact` kind indicates that the artifact is a generic artifact. It's usefull if you intend to manipulate the artifact as a file, you can in fact download/upload it.

### Artifact spec parameters

The `artifact` kind has the following `spec` parameters to pass to the `new_artifact()` method:

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| [`path`](../stores.md#entity-paths) | *str* | Path of the artifact, can be a local path or a remote path, a single filepath or a directory/partition. | *required* |
| `src_path` | *str* | Local file/directory source path. | `None` |

### Artifact methods

The `artifact` kind has no additional methods.
