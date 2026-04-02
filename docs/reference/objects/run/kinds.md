# Run kinds

At the moment, we support the following kinds and the related runtime:

- [**`python+job:run`**](../../runtimes/python/actions/python-job.md)
- [**`python+serve:run`**](../../runtimes/python/actions/python-serve.md)
- [**`python+build:run`**](../../runtimes/python/actions/python-build.md)
- [**`guardrail+build:run`**](../../runtimes/python/actions/guardrail-build.md)
- [**`guardrail+serve:run`**](../../runtimes/python/actions/guardrail-serve.md)
- [**`openinference+build:run`**](../../runtimes/python/actions/openinference-build.md)
- [**`openinference+serve:run`**](../../runtimes/python/actions/openinference-serve.md)
- [**`dbt`**](../../runtimes/dbt/overview.md)
- [**`container`**](../../runtimes/container/overview.md)
- [**`modelserve`**](../../runtimes/modelserve/overview.md)
- [**`flower`**](../../runtimes/flower/overview.md)
- [**`hera`**](../../runtimes/hera/overview.md)

For each different kind, the `Run` object has its own subclass with different `spec` and `status` attributes.

Please see the runtime documentation for more information on the related to the various `spec` arguments.
