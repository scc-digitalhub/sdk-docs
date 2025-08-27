# Task

The DBT runtime provides a `transform` task action to run dbt transformations. A `Task` is created by calling `run()` on the Function; task parameters are passed through that call and may vary by action.

## Parameters (Shared)

| Name | Type | Description |
| --- | --- | --- |
| [action](#task-actions) | str | Task action. One of: `transform`. **Required.** |
| [node_selector](../../../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. |
| [volumes](../../../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. |
| [resources](../../../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. |
| [affinity](../../../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. |
| [tolerations](../../../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. |
| [envs](../../../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. |
| [secrets](../../../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. |
| [profile](../../../configuration/kubernetes/overview.md#profile) | str | Profile template. |

## Task Actions

Supported actions:

- `transform` — run a dbt transformation
