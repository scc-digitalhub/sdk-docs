# Task

The ModelServe runtime supports a `serve` task action to deploy ML models on Kubernetes or locally. A `Task` is created by calling `run()` on the Function; task parameters are passed through that call and may vary by action.

## Parameters (Shared)

| Name | Type | Description |
| --- | --- | --- |
| [action](#task-actions) | str | Task action. One of: `serve`. **Required.** |
| [node_selector](../../../configuration/kubernetes/overview.md#node-selector) | list[dict] | Node selector. |
| [volumes](../../../configuration/kubernetes/overview.md#volumes) | list[dict] | List of volumes. |
| [resources](../../../configuration/kubernetes/overview.md#resources) | dict | Resource limits/requests. |
| [affinity](../../../configuration/kubernetes/overview.md#affinity) | dict | Affinity configuration. |
| [tolerations](../../../configuration/kubernetes/overview.md#tolerations) | list[dict] | Tolerations. |
| [envs](../../../configuration/kubernetes/overview.md#secrets-envs) | list[dict] | Environment variables. |
| [secrets](../../../configuration/kubernetes/overview.md#secrets-envs) | list[str] | List of secret names. |
| [profile](../../../configuration/kubernetes/overview.md#profile) | str | Profile template. |
| [replicas](../../../configuration/kubernetes/overview.md#replicas) | int | Number of replicas. |
| [service_type](../../../configuration/kubernetes/overview.md#service-port-type) | str | Service type. |

## Function Kind-Specific Parameters

### HuggingFace Serve

| Name | Type | Description |
| --- | --- | --- |
| [huggingface_task](#huggingface-task) | str | Huggingface task type. |
| [backend](#backend) | str | Backend type. |
| tokenizer_revision | str | Tokenizer revision. |
| max_length | int | Huggingface max sequence length for the tokenizer. |
| disable_lower_case | bool | Do not use lower case for the tokenizer. |
| disable_special_tokens | bool | The sequences will not be encoded with the special tokens relative to their model. |
| [dtype](#dtype) | str | Data type to load the weights in. |
| trust_remote_code | bool | Allow loading of models and tokenizers with custom code. |
| tensor_input_names | list[str] | The tensor input names passed to the model. |
| return_token_type_ids | bool | Return token type ids. |
| return_probabilities | bool | Return all probabilities. |
| disable_log_requests | bool | Disable log requests. |
| max_log_len | int | Max number of prompt characters or prompt. |

## Task Actions

Supported actions:

- `serve` — deploy a service

## HuggingFace Task

You can specify the task type for the Huggingface model. The task type must be one of the following:

- `sequence_classification`
- `token_classification`
- `fill_mask`
- `text_generation`
- `text2text_generation`
- `text_embedding`

## Backend

You can specify the backend type for the Huggingface model. The backend type must be one of the following:

- `AUTO`
- `VLLM`
- `HUGGINGFACE`

## Dtype

You can specify the data type to load the weights in. The data type must be one of the following:

- `AUTO`
- `FLOAT32`
- `FLOAT16`
- `BFLOAT16`
- `FLOAT`
- `HALF`
