#[YAML file Notes]

- YAML stands for YAML Ain't Markup Language.
- It is used to store data in a simple and human-readable format.
- YAML uses key-value pairs, nested dictionaries, and lists.
- Indentation must be consistent for correct syntax.
- Common data types include strings, numbers, and booleans.

Example:

```yaml
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
```



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



# To execute a multi-line bash script, you can use the following syntax:

```yaml
steps:
  - name: Multi-line bash
    run: |
      echo "I am"
      echo "a multi-line"
      echo "script."
``` 


# Workflow Runners

Runners are virtual machines or environments that execute the jobs in a workflow.

- GitHub-hosted runners: managed by GitHub and include operating systems like Windows, Ubuntu, and macOS. GitHub handles updates, security patches, and infrastructure. Each job runs on its own virtual machine, and jobs are independent by default.
- Self-hosted runners: run on your own machine or server, giving you more control over the environment and infrastructure.

# Actions

Actions are reusable applications that help perform common or repetitive tasks in a workflow.

- They help avoid long and repeated commands.
- They are often used for setup tasks, dependency installation, or running scripts.

```yaml
steps:
  - uses: some/action
    with:
      input: value
  - run: echo "Hello"
```

You can find popular actions in the GitHub Actions Marketplace: https://github.com/marketplace?type=actions


# Event filters

Event filters specify the conditions under which a workflow should run.

Example:

```yaml
on:
  push:
    branches:
      - main
    paths-ignore:
      - docs/**
  pull_request:
```

This means the workflow runs when code is pushed to the main branch, except when changes are only in the docs folder, or when a pull request is opened.


# Activity types

Activity types define what GitHub events should trigger the workflow.

Example:

```yaml
on:
  pull_request:
    types: [opened, closed]
```

This means the workflow runs when an issue is opened or closed.


# Workflow Contexts
Access information about jobs , variables , runs etc...

syntax : ${{ <context> }}


# Expressions and variables

- `if` condition
- Syntax:

```yaml
if: github.event_name == 'workflow_dispatch' && inputs.debug == true
run: |
  echo "Triggered by: ${{ github.event_name }}"
```

- Environment variables
- Syntax:

```yaml
env:
  MY_OVERRIDE_VAR: 'step'

steps:
  - name: Use step env variable
    run: echo "Variable from step env: ${{ env.MY_OVERRIDE_VAR }}"
```

- Variables from repository level
- Syntax:

```yaml
steps:
  - name: Repository env variable from prod
    run: echo "ENV Variable from GitHub settings: ${{ vars.SECRET_NAME }}"
```

# Functions

Functions let you use built-in logic inside GitHub Actions expressions. They are written inside `${{ ... }}` and help you check values, format text, or inspect the result of previous steps.

Examples:

```yaml
if: startsWith(github.ref, 'refs/heads/main')
run: echo "This is the main branch"
```

```yaml
if: contains(github.event.head_commit.message, 'hotfix')
run: echo "Hotfix commit detected"
```

```yaml
if: ${{ success() }}
run: echo "Previous steps succeeded"
```

Common examples:
- General-purpose functions: `startsWith`, `contains`, `format`, `join`, `endsWith`, `split`, `toJson`, `fromJson`, `hashFiles`
- Status check functions: `success()`, `failure()`, `cancelled()`, `always()`


# Controlling the execution flow

GitHub Actions lets you control how steps and jobs run in a workflow.

- Standard execution: steps run one after another in the order they are written.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Step 1"
      - run: echo "Step 2"
```

This runs `Step 1` first, then `Step 2`.

- Conditional execution: use `if` to run a step or job only when a condition is true.

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy only on main
        if: github.ref == 'refs/heads/main'
        run: echo "Deploying to production"
```

This step runs only when the workflow is triggered from the `main` branch.

- Non-dependent execution: jobs without `needs` can run in parallel.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build"

  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Test"
```

Here, `build` and `test` can run at the same time.

- Dependent execution: use the `needs` key so one job waits for another job to finish.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build"

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Test"
```

Here, `test` starts only after `build` succeeds.