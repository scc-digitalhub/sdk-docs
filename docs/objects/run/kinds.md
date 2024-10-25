# Run kinds

At the moment, we support the following kinds and the related runtime:

- [**`python`**](../../runtimes/python.md#run-methods)
- [**`dbt`**](../../runtimes/dbt.md#run-methods)
- [**`container`**](../../runtimes/container.md#run-methods)
- [**`modelserve`**](../../runtimes/modelserve.md#run-methods)
- [**`kfp`**](../../runtimes/kfp.md#run-methods)

For each different kind, the `Run` object has its own subclass with different `spec` and `status` attributes.

Please see the runtime documentation for more information on the related to the various `spec` arguments.
