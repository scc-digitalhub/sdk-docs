# Examples

## MLflow Model Example

```python
import digitalhub as dh

project = dh.get_or_create_project("my_project")

function = project.new_function(name="mlflow-serve-function",
                                kind="mlflowserve",
                                path=model.spec.path + "model")

run = function.run(action="serve")
```

## Scikit-Learn Model Example

```python
function = project.new_function(name="sklearn-serve-function",
                                kind="sklearnserve",
                                path=model.spec.path)

run = function.run(action="serve")
```

## KubeAI Text Model Example

```python
function = project.new_function(
    name="kubeai-text-function",
    kind="kubeai-text",
    url="hf://mistralai/Mistral-7B-v0.1",
    features=["TextGeneration"],
    engine="VLLM"
)

run = function.run(action="serve")
```

## KubeAI Speech Model Example

```python
function = project.new_function(
    name="kubeai-speech-function",
    kind="kubeai-speech",
    url="hf://openai/whisper-large-v3",
    features=["SpeechToText"],
    engine="FasterWhisper"
)

run = function.run(action="serve")
```
