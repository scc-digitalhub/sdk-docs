# Runtimes

Functions are the logical description of something that the platform may execute and track for you. A function may represent code to run as a job, an ML model inference to be used as batch procedure or as a service, a data validation, etc.

In the platform we perform actions over functions (also referred to as "tasks"), such as job execution, deploy, container image build. A single action execution is called run, and the platform keeps track of these runs, with metadata about function version, operation parameters, and runtime parameters for a single execution.

They are associated with a given runtime, which implements the actual execution and determines which actions are available. Examples are DBT, Container, Python, etc. Runtimes are highly specialized components which can translate the representation of a given execution, as expressed in the run, into an actual execution operation performed via libraries, code, external tools etc.

Runtimes define the key point of extension of the platform: new runtimes may be added in order to implement the low-level logic of "translating" the high level operation definition into an executable run. For example, DBT runtime allows for defining the transformation as a task that, given the input table reference, produces a dataset appyling the function defined as SQL code. The runtime in this case is responsible for converting the specification and the references to a dedicated Kubernetes Job that runs DBT transformation and stores the corresponding dataset.

## Supported runtimes

- [Python](python.md)
- [Container](container.md)
- [DBT](dbt.md)
- [KFP](kfp.md)
- [ModelServe](modelserve.md)
