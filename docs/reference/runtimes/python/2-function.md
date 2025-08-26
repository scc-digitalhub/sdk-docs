# Define a Python function

You declare a Python handler in the usual way using the `def` keyword. Handlers can be simple or accept platform-provided objects and inputs; the runtime injects a small set of reserved arguments and provides helpers to map inputs and outputs.

## Anatomy of a function

![function](./asset/function.png)

Example handler:

```python
from digitalhub_runtime_python import handler


@handler(outputs=["my-sdk-output", "my-primitive-output"])
def func(project,
         run,
         context,
         event,
         input_1,
         parameter_1):

    project.log_artifact("my-artifact",
                         "artifact",
                         source="some-file.ext")

    run.log_metric("my-metric", -14.6)

    context.logger.info("log-some-string")

    body = event.body
    # work with the event body

    df = input_1.as_df(sep=";")
    df.head(70)

    parameter_1.pop("some-key")

    return df, 19.45
```

Handlers may also be parameterless and return nothing:

```python
def func():
    print("hello world")
```

The function you define becomes the entrypoint when referenced as the `handler` in the run configuration.

The runtime supplies utilities to work with platform objects and to handle inputs/outputs. This document covers:

- [Define a Python function](#define-a-python-function)
  - [Anatomy of a function](#anatomy-of-a-function)
  - [Reserved arguments](#reserved-arguments)
  - [Inputs and parameters](#inputs-and-parameters)
  - [Handler and outputs](#handler-and-outputs)
  - [Init function](#init-function)

## Reserved arguments

The runtime injects a small set of reserved arguments when it invokes your handler. Commonly injected values are:

- `project` — the current [`Project` object](../../objects/project/entity.md).
- `run` — the active [`Run` object](../../objects/run/entity.md).
- `context` — the Nuclio runtime context object (see Nuclio Python runtime docs) — only available in remote execution.
- `events` — the Nuclio events helper — only available in remote execution.

!!! warning "Local execution: Nuclio context and events"
    When running locally, `context` and `events` are not provided automatically; if your handler expects them you must pass them explicitly through `function.run()`.

## Inputs and parameters

Inputs and parameters map function argument names to values provided at run time. They are passed to the run via the `inputs` and `parameters` arguments of `function.run()` and are stored in the `Run` spec.

- Inputs must reference platform entities (for example `Dataitem`, `Artifact`, or `Model`) by their keys.
- Parameters may be plain Python values (strings, numbers, dicts, lists, etc.).

Example:

```python
# Function signature: di is a Dataitem, param1 is a string
def func(di: Dataitem, param1: str):
    # do something
    ...

# Create or obtain the dataitem
sdk_dataitem = sdk.new_dataitem(...)

# Run the function, mapping the argument name to the dataitem key
sdk_function.run(inputs={"di": sdk_dataitem.key},
                 parameters={"param1": "some value"})
```

!!! warning "Inputs vs parameters"
    Passing a parameter where an input is expected can produce an error stating the SDK cannot parse an `entity_key`. If you see that error, double-check which values you provided in `inputs` vs `parameters`.

## Handler and outputs

Decorating a function with `@handler` (from `digitalhub_runtime_python`) allows you to name and collect outputs from the run. The decorator maps returned values to `outputs` and `results` on the resulting `Run` object.

```python
from digitalhub_runtime_python import handler

@handler(outputs=["data", "string"])
def func(di: Dataitem, param1: str):
    # produce a Dataitem and a primitive
    return pd.DataFrame, "some value"


run = sdk_function.run(inputs={"di": sdk_dataitem.key},
                       parameters={"param1": "some value"},
                       ...)

# After the run completes
run.output("data")   # returns a Dataitem object
run.result("string") # returns "some value"
```

You can omit the decorator; in that case the SDK will assign default placeholder names to any outputs you return. If you use `@handler`, map outputs explicitly to collect named results.

## Init function

When executing remotely, the Nuclio wrapper calls an `init` function (if present) before invoking your handler. The runtime injects the Nuclio `context` into `init` at invocation time. Additional parameters may be supplied via `init_parameters` in `function.run()`.

```python
def init(context, param1, param2):
    # initialization logic
    ...


run = sdk_function.run(...,
                       init_parameters={"param1": "some value",
                                        "param2": "some value"})
```

See [create](3-entities.md) for the arguments required to create a `Function` object and start a run.
