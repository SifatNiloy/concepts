# Level 1: What is CI/CD? (Beginner Friendly)

## 📖 The Story of the Two Developers

### Developer A (Manual Manny):
1. Writes code for 2 weeks.
2. Friday afternoon, he decides to deploy.
3. He opens PowerShell, runs `docker build`, copies files manually via FTP or SSH.
4. The server crashes.
5. He realizes his colleague modified a package 4 days ago that broke the build.
6. Manny spends his entire Friday night sweating and trying to fix the server. 😭

### Developer B (Automated Anna):
1. Writes a feature in 20 lines of code.
2. Types: `git push origin main`.
3. She immediately walks away to grab a coffee. ☕
4. In the background, GitHub wakes up an automated server:
   - Tests the code.
   - Builds the Docker image.
   - Verifies there are no errors.
   - Deploys the container to production.
5. If anything is broken, Anna gets a notification on her phone within 60 seconds with the exact error.

**Anna is using CI/CD.**

---

## 🧩 Breaking Down the Acronym

### 1. CI = Continuous Integration
* **Continuous:** Happening frequently (multiple times a day whenever you commit code).
* **Integration:** Merging your code changes into the shared team repository.
* **The Goal of CI:** Ensure that new code **doesn't break existing features**.  
  * Runs tests automatically.
  * Runs linters (`eslint`).
  * Builds the Docker image to prove that it compiles cleanly.

### 2. CD = Continuous Delivery or Continuous Deployment
* **Continuous Delivery:** Your code is always packaged, tested, and ready to be deployed at the click of a button.
* **Continuous Deployment:** Zero human buttons! Every commit that passes tests is automatically deployed directly to the live server.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        THE CI/CD PIPELINE                              │
│                                                                        │
│   [ git push ] ──▶ [ Test & Lint ] ──▶ [ Docker Build ] ──▶ [ Deploy ] │
│      (You)               (CI)                 (CI)             (CD)    │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ The Tool We Will Use: GitHub Actions

There are many CI/CD tools (GitLab CI, Jenkins, CircleCI, TravisCI). But **GitHub Actions** is the #1 industry standard today because:
* It is built right inside GitHub (no separate accounts or servers needed).
* **It is 100% free** for public repositories, and gives you 2,000 free minutes per month for private repositories!

---

## 🎯 Hands-On Challenge 1: Your Very First Workflow

Let's write your first automated action in less than 3 minutes.

### Step 1: Where do CI/CD files live?
GitHub looks for automated scripts in one very specific folder inside your project:
`.github/workflows/`

### Step 2: Create the folder & file
In any project (or your `my-nextjs-docker-app`), create:
1. A folder called `.github`
2. Inside it, a folder called `workflows`
3. Inside it, a file called `first-action.yml`:

```yaml
name: My First Automated Action

# When should this run? (Trigger / Event)
on:
  push:
    branches: [ main ]
  workflow_dispatch: # Allows you to click a button to run it manually!

# What should it do? (Jobs)
jobs:
  say-hello:
    runs-on: ubuntu-latest # GitHub gives you a free temporary Ubuntu virtual machine!

    steps:
      - name: Say Hello
        run: echo "Hello Sifat! Welcome to the automated world of CI/CD!"

      - name: Show Current Date & Time
        run: date

      - name: Check Docker Version on the Runner
        run: docker --version
```

### Step 3: What does this YAML file do?
1. `on: [push]`: Tells GitHub: *"Whenever someone pushes code to the `main` branch, wake up!"*
2. `runs-on: ubuntu-latest`: GitHub spins up a fresh, free Ubuntu cloud machine just for you.
3. `steps`: GitHub runs each command sequentially on that machine and reports the status.

---

## ✅ Level 1 Checklist

- [ ] I understand the difference between Continuous Integration (CI) and Continuous Deployment (CD).
- [ ] I know why CI/CD saves developers from "Friday deployment disasters".
- [ ] I know that GitHub Actions workflows live in `.github/workflows/`.
- [ ] I understand what `on:`, `jobs:`, and `steps:` mean in YAML.

👉 Ready for Level 2? Open [level-2-basics.md](file:///d:/CODES/concepts/2-cicd-learning-guide/level-2-basics.md) to understand the core building blocks of GitHub Actions!
