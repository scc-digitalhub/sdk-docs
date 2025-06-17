# Trigger object

The `Trigger` object comes with two sets of methods: CRUD methods and status methods inherited from the `VersionedEntity` class.

## CRUD methods

CRUD methods are used to interact with the entity object in the backend or locally:

- `save()`: Save or update the entity into the backend
- `export()`: Export the entity locally as yaml file
- `refresh()`: Refresh (read) the entity from the backend

::: digitalhub.entities.trigger._base.entity.Trigger.save
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub.entities.trigger._base.entity.Trigger.export
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

::: digitalhub.entities.trigger._base.entity.Trigger.refresh
    options:
        heading_level: 3
        show_signature: false
        show_source: false
        show_root_heading: true
        show_symbol_type_heading: true
        show_root_full_path: false
        show_root_toc_entry: true

## Trigger Specifications

The trigger object has specific attributes based on its kind:

### Base Trigger

All triggers have these base specifications:

- `task`: The task to execute
- `template`: Configuration template
- `function` or `workflow`: The target to execute (either a function or workflow)

### Scheduler Trigger

Additional specifications:

- `schedule`: Quartz cron expression

### Lifecycle Trigger

Additional specifications:

- `key`: Entity key to monitor (format: `store://<project>/<entity-type>/<entity-kind>/<name>`)
- `states`: List of states that will trigger execution

For more details about trigger kinds and their specific configurations, see the [kinds section](kinds.md).

## Creating Triggers from Functions and Workflows

Triggers can be created directly from Function and Workflow objects using their `trigger()` method. This provides a convenient way to set up triggers for specific functions or workflows.

Example using a Function:

```python
function = project.get_function("my-function")

# Create a scheduler trigger
trigger = function.trigger(
    action="job",
    trigger_kind="scheduler",
    trigger_name="daily-function-run",
    schedule="0 0 * * *"  # Run daily at midnight
)

# Create a lifecycle trigger
trigger = function.trigger(
    action="job",
    trigger_kind="lifecycle",
    trigger_name="function-on-event",
    key="store://project/model/kind/name",
    states=["COMPLETED"]
)
```

Example using a Workflow:

```python
workflow = project.get_workflow("my-workflow")

# Create a scheduler trigger
trigger = workflow.trigger(
    action="pipeline",
    trigger_kind="scheduler",
    trigger_name="weekly-workflow-run",
    schedule="0 0 * * 0"  # Run weekly on Sunday
)

# Create a lifecycle trigger
trigger = workflow.trigger(
    action="pipeline",
    trigger_kind="lifecycle",
    trigger_name="workflow-on-event",
    key="store://project/model/kind/name",
    states=["completed"]
)
```
