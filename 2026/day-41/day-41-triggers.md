# Day 41 – Triggers & Matrix Builds

## Task: Completed

Your pipeline runs on push. Today you learn **every way to trigger a workflow** and how to run jobs across multiple environments at once.

---

## Expected Output

- New workflow files in your `github-actions-practice` repo
- A markdown file: `day-41-triggers.md`

---

## Challenge Tasks

### Task 1: Trigger on Pull Request

1. Create `.github/workflows/pr-check.yml`
2. Trigger it only when a pull request is **opened or updated** against `main`
3. Add a step that prints: `PR check running for branch: <branch name>`
4. Create a new branch, push a commit, and open a PR
5. Watch the workflow run automatically

**Verify:** Does it show up on the PR page?

:
name: PR check

on:
pull_request:
branches: [main]

jobs:
pr-check:
runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Run Linter
        run: echo "PR CHECK FOR branch ${{ github.head_ref }}"

---

### Task 2: Scheduled Trigger

1. Add a `schedule:` trigger to any workflow using cron syntax
2. Set it to run every day at midnight UTC
3. Write in your notes: What is the cron expression for every Monday at 9 AM?

code:

name: Scheduled Workflow

on:
schedule: - cron: "58 10 \* \* \*" # Every day at midnight

jobs:
scheduled-job:
runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Run Scheduled Task
        run: date && echo "This workflow runs every day at midnight!"

---

### Task 3: Manual Trigger

1. Create `.github/workflows/manual.yml` with a `workflow_dispatch:` trigger
2. Add an **input** that asks for an `environment` name (staging/production)
3. Print the input value in a step
4. Go to the **Actions** tab → find the workflow → click **Run workflow**

**Verify:** Can you trigger it manually and see your input printed?

code:

name: manual workflow

on:
workflow_dispatch:
inputs:
environment:
description: "Select environment"
required: true
default: "dev"
type: choice
options: - dev - staging - prod
jobs:
manual-job:
runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Print Selected Environment
        run: echo "Selected environment is ${{ github.event.inputs.environment }}"

---

### Task 4: Matrix Builds

Create `.github/workflows/matrix.yml` that:

1. Uses a matrix strategy to run the same job across:
   - Python versions: `3.10`, `3.11`, `3.12`
2. Each job installs Python and prints the version
3. Watch all 3 run in parallel

Then extend the matrix to also include 2 operating systems — how many total jobs run now?

name: matrix workflow build

on:
push:
branches: [main]

jobs:
matrix-build:
runs-on: ubuntu-latest
strategy:
fail-fast: false
matrix:
python-version: ["3.10", "3.11", "3.12"]

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}

      - name: print python version
        run: python --version

---

### Task 5: Exclude & Fail-Fast

1.  In your matrix, **exclude** one specific combination (e.g., Python 3.10 on Windows)

        exclude:
          - os: windows-latest
            python-version: 3.10

2.  Set `fail-fast: false` — trigger a failure in one job and observe what happens to the rest
    fail-fast: false

This means:

If one job fails, the other jobs will continue running.

Example result:

Ubuntu + Python 3.10 ✅
Ubuntu + Python 3.11 ❌
Ubuntu + Python 3.12 ✅
Windows + Python 3.11 ❌
Windows + Python 3.12 ✅

All jobs still complete.

3.  Write in your notes: What does `fail-fast: true` (the default) do vs `false`?
    Default behavior (fail-fast: true)

If any job in the matrix fails, GitHub Actions cancels the remaining jobs immediately.

Example:

Job 1 ❌ failed
Job 2 ⛔ cancelled
Job 3 ⛔ cancelled
Job 4 ⛔ cancelled

Purpose:

Save CI time

Stop wasting compute resources

fail-fast: false

If a job fails:

Job 1 ❌
Job 2 ✅
Job 3 ✅
Job 4 ❌
Job 5 ✅

All jobs continue running.

Purpose:

Debugging

Multi-platform testing
