# Run kinds

At the moment, we support the following kinds and the related runtime:

- [**`python`**](../../runtimes/python/entities/run.md)
- [**`dbt`**](../../runtimes/dbt/entities/run.md#run)
- [**`container`**](../../runtimes/container/execution.md)
- [**`modelserve`**](../../runtimes/modelserve/entities/run.md#run)
- [**`kfp`**](../../runtimes/kfp.md#run)

For each different kind, the `Run` object has its own subclass with different `spec` and `status` attributes.

Please see the runtime documentation for more information on the related to the various `spec` arguments.
