# Quickstart

This is the quickstart guide for the Digitalhub SDK. It will walk you through how to use the SDK in the execution of a simple python function that produces some data as output.

## Create a project

We can start creating a project with the following command:

```python
import digitalhub as dh

project = dh.get_or_create_project(
    name="my-project",
    description="My project"
)
```

A project is the context in which you can run functions and manage data and artifacts.

## Write a python function

First, write your function in a file called `downloader.py`:

```python
from digitalhub_runtime_python import handler

@handler(outputs=["df"])
def downloader(url):
    return url.as_df()
```

## Create a dataitem

Second, create a dataitem to refernce a remote table containing the data we want:

```python
url = "https://gist.githubusercontent.com/kevin336/acbb2271e66c10a5b73aacf82ca82784/raw/e38afe62e088394d61ed30884dd50a6826eee0a8/employees.csv"
di = project.new_dataitem(name="employees-table", kind="table", path=url)
```

## Create a function

Third, create a function with the following command:

```python
func = project.new_function(
    name="downloader",
    kind="python",
    code_src="downloader.py",
    handler="downloader",
    python_version="PYTHON3_9",
)
```

## Run the function

Run the function with the following command:

```python
run = func.run(action="job",
               inputs={"url": di.key},
               wait=True)
```

And finally get the output result:

```python
run.output("df").as_df()
```
