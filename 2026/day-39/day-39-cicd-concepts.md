# Day 39 – What is CI/CD?

## Task: Completed

Before writing a single pipeline, understand **why CI/CD exists** and what it actually does.

Today is a research and diagram day — no pipelines yet. Get the concepts right first.

---

## Challenge Tasks

### Task 1: The Problem

Think about a team of 5 developers all pushing code to the same repo manually deploying to production.

Write in your notes:

1. What can go wrong?
   --> Many issues can happen:

• Code conflicts between developers
• One developer deploys an untested feature
• Different versions of the app get deployed
• Deployment steps may be missed or done incorrectly
• Human errors during manual deployment
• Production outages or broken features
• No clear rollback strategy

Manual deployment is slow, risky, and inconsistent.

2. What does "it works on my machine" mean and why is it a real problem?
   -->
   "It works on my machine" means:

The code runs successfully on a developer’s local system but fails on another system or server.

This happens because of:

• Different OS environments
• Different dependency versions
• Missing environment variables
• Different configuration files

Example:

A developer runs an app locally with:

Python 3.11
PostgreSQL 14

But the production server has:

Python 3.9
PostgreSQL 12

The application may fail in production.

This is why standardized environments and CI pipelines are important.

3. How many times a day can a team safely deploy manually?
   -->
   Usually 1–2 times per day at most, because:

• Manual checks take time
• Deployment risk is high
• Teams prefer deploying during safe windows

With CI/CD automation, teams like Netflix or Amazon deploy hundreds or thousands of times per day safely.

---

### Task 2: CI vs CD

Research and write short definitions (2-3 lines each):

1. **Continuous Integration** — what happens, how often, what it catches
   -->

   Continuous Integration is the practice where developers frequently merge code into a shared repository.
   Every commit automatically triggers builds and automated tests to detect errors early.

Purpose:
• Catch bugs quickly
• Ensure code integrates properly
• Maintain code quality

Example:

A developer pushes code to GitHub →
GitHub Actions runs:

Install dependencies
Run unit tests
Check code formatting

If tests fail, the pipeline fails.

2. **Continuous Delivery** — how it's different from CI, what "delivery" means
   -->

   Continuous Delivery means that after CI completes successfully, the application is automatically prepared for release.

The code is ready to deploy to production, but deployment usually requires manual approval.

Example:

Pipeline flow:

Push Code
↓
Run Tests
↓
Build Application
↓
Deploy to Staging
↓
Manual Approval
↓
Deploy to Production

This allows teams to release quickly but safely.

3. **Continuous Deployment** — how it differs from Delivery, when teams use it
   -->

   Continuous Deployment goes one step further.

If the pipeline passes all tests, the application is automatically deployed to production without manual approval.

Example:

Push Code
↓
Run Tests
↓
Build Docker Image
↓
Deploy to Production Automatically

Companies like Netflix, Facebook, and Amazon use this approach.

---

### Task 3: Pipeline Anatomy

A pipeline has these parts — write what each one does:

- **Trigger** — what starts the pipeline
  -->
  Examples:

• Code push
• Pull request
• Scheduled time
• Manual trigger

Example:

on: push

- ## **Stage** — a logical phase (build, test, deploy)

  -->

      A stage is a major phase in the pipeline.

Common stages:

Build
Test
Deploy

Each stage represents a logical step in the delivery process.

- **Job** — a unit of work inside a stage
  --> A job is a group of tasks executed on a machine.

Example:

Build Job
Test Job
Security Scan Job

Jobs inside a stage can run in parallel or sequentially.

- **Step** — a single command or action inside a job
  -->
  A step is a single command or action inside a job.

Example:

npm install
npm test
docker build

Each job contains multiple steps.

- **Runner** — the machine that executes the job
  -->
  A runner is the machine that executes the pipeline jobs.

Examples:

• GitHub Runner
• Jenkins Agent
• GitLab Runner

It can be:

• A virtual machine
• A container
• A physical server

- **Artifact** — output produced by a job
  -->
  An artifact is a file or output generated during the pipeline.

Examples:

• Compiled application
• Docker image
• Build logs
• Test reports

Artifacts can be passed between pipeline stages.

Example:

build/app.jar
docker-image.tar
test-report.html

---

### Task 4: Draw a Pipeline

Draw a CI/CD pipeline for this scenario:

> A developer pushes code to GitHub. The app is tested, built into a Docker image, and deployed to a staging server.

Include at least 3 stages. Hand-drawn and photographed is perfectly fine.

-->

    Scenario:

A developer pushes code to GitHub.
The app is tested, built into a Docker image, and deployed to staging.

Pipeline flow:

Developer Push Code
│
▼
Trigger Pipeline
│
▼
┌───────────────┐
│ Stage 1 │
│ Build │
│ Install deps │
│ Compile app │
└───────────────┘
│
▼
┌───────────────┐
│ Stage 2 │
│ Test │
│ Run unit tests│
│ Run linting │
└───────────────┘
│
▼
┌───────────────┐
│ Stage 3 │
│ Dockerize │
│ Build image │
│ Push to repo │
└───────────────┘
│
▼
┌───────────────┐
│ Stage 4 │
│ Deploy │
│ Deploy to │
│ Staging │
└───────────────┘

---

### Task 5: Explore in the Wild

1. Open any popular open-source repo on GitHub (Kubernetes, React, FastAPI — pick one you know)
2. Find their `.github/workflows/` folder
3. Open one workflow YAML file
4. Write in your notes:
   - What triggers it?
   - How many jobs does it have?
   - What does it do? (best guess)

answer:

Repository:
FastAPI

GitHub Repo:
https://github.com/fastapi/fastapi

Workflow folder:

.github/workflows/

Example workflow file:

test.yml
What triggers it?

Example triggers:

on:
push:
pull_request:

This means the pipeline runs whenever code is pushed or a pull request is created.

How many jobs does it have?

Example:

jobs:
test
lint
build

This workflow contains 3 jobs.

What does it do?

Typical actions include:

• Installing Python dependencies
• Running unit tests
• Checking code formatting
• Running static analysis
• Validating pull requests

Purpose: ensure code quality before merging.

---
