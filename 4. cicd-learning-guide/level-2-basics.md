# Level 2: GitHub Actions Core Concepts (The 5 Building Blocks)

To master GitHub Actions without getting confused by YAML files, you only need to understand **5 fundamental concepts**.

---

## 🏛️ The 5 Pillars of GitHub Actions

```
┌─────────────────────────────────────────────────────────────┐
│ 1. WORKFLOW (The whole file: deploy.yml)                    │
│                                                             │
│   2. EVENT (The Trigger: on git push, on pull_request)      │
│                                                             │
│   3. JOBS (Units of work: e.g. "lint", "test", "build")     │
│      runs on:                                               │
│      4. RUNNER (Free Ubuntu/Windows machine hosted by GH)   │
│                                                             │
│      5. STEPS (Sequential commands or community actions)    │
│         - Step 1: Checkout code                             │
│         - Step 2: Run npm test                              │
└─────────────────────────────────────────────────────────────┘
```

---

### 1. Workflow
A workflow is an automated procedure stored as a `.yml` file in `.github/workflows/`. You can have multiple workflows in one repo (e.g., `tests.yml` to test pull requests, and `deploy.yml` to deploy releases).

### 2. Event (The Trigger)
What wakes up the workflow?
* `on: push`: Every time you push code.
* `on: pull_request`: When someone opens a pull request.
* `on: schedule`: Runs on a cron timer (e.g., every night at midnight).
* `on: workflow_dispatch`: Adds a manual **"Run workflow"** button in GitHub's web UI.

### 3. Job
A workflow contains one or more **jobs**. 
* By default, multiple jobs run **in parallel** (at the same time) to save time!
* If Job B needs Job A to finish first, you use `needs: [job-a]`.

### 4. Runner
The runner is the virtual machine executing your commands.
* Most developers use `runs-on: ubuntu-latest`.
* You can also use `windows-latest` or `macos-latest` if needed.
* Every time a job starts, GitHub gives you a completely clean, pristine machine with Git, Docker, Node.js, and Python pre-installed. When the job finishes, the machine is wiped.

### 5. Steps
Tasks executed inside a job. A step can be:
* A shell command: `run: npm test`
* A pre-made community action: `uses: actions/checkout@v4`

---

## 🔍 What is `uses:` vs `run:`?

This is the most common confusion for beginners:

```yaml
steps:
  # USES: Uses someone else's pre-built code block (an "Action")
  - name: Download my code to the runner
    uses: actions/checkout@v4

  # RUN: Runs a raw terminal command on the runner
  - name: Install dependencies
    run: npm ci
```

* **`run`**: Runs any bash / powershell command directly.
* **`uses`**: Runs a reusable plugin created by GitHub or the open-source community from the GitHub Marketplace.

> 💡 **Why do we need `actions/checkout@v4`?**  
> When GitHub spins up your fresh Ubuntu runner, the machine is **completely empty**—your repository code is NOT on it yet! `actions/checkout@v4` clones your repo into the runner so subsequent steps can see your files.

---

## 🎯 Hands-On Example: A Complete Multi-Job Workflow

Create `.github/workflows/test-and-build.yml`:

```yaml
name: Test and Build Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  # Job 1: Check code quality
  lint-and-check:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run Linter
        run: npm run lint

  # Job 2: Build the app (Only runs if Job 1 succeeds!)
  build:
    needs: lint-and-check # Waits for lint-and-check to pass
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Build Next.js bundle
        run: npm run build
```

---

## ✅ Level 2 Checklist

- [ ] I can name the 5 building blocks: Workflow, Event, Job, Runner, Step.
- [ ] I understand why `actions/checkout@v4` is almost always the first step.
- [ ] I know the difference between `run:` (terminal command) and `uses:` (marketplace action).
- [ ] I know how to make Job 2 wait for Job 1 using `needs:`.
