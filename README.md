# YAML file notes

- YAML stands for YAML Ain't Markup Language.
- It is used to store data in a simple and human-readable format.
- YAML uses key-value pairs, nested dictionaries, and lists.
- Indentation must be consistent for correct syntax.
- Common data types include strings, numbers, and booleans.

Example:

name: Example YAML
num: 1
is_valid: true

config:
  name: Example YAML
  num: 1
  is_valid: true

array1:
  - element1
  - 10
  - element3

array2:
  - name: element1
    num: 10
    labels:
      - label1
      - label2
  - name: element2
    num: 20



# Workflow jobs and steps

- A workflow is the overall automation process in GitHub Actions.
- A job is a set of tasks that run in the same environment.
- A step is one action or command inside a job.
- Jobs can run in parallel or sequentially depending on the workflow.
- Steps are executed one by one in the order they are written.

Example idea:
- One job can build the project.
- Another job can test the project.
- Each job can have multiple steps like installing dependencies and running tests.


- Workflow: defined at the repository level and contains the automation rules.
- Job: defined inside a workflow and runs in a specific execution environment; it can include one or more steps. Run parllel by default.
- Steps: defined inside a job and are the actual commands or actions GitHub Actions executes. Steps Run sequential by default.

