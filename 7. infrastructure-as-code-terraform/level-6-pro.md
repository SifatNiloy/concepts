# Level 6: Pro - GitOps & Automated Terraform with GitHub Actions

In top-tier engineering teams, **nobody runs `terraform apply` from their personal laptops**.

Why? Because running it locally means credentials live on employee laptops, and state files can become corrupted.

Instead, Terraform runs automatically inside **GitHub Actions** via **GitOps**.

---

## 🤖 The Automated GitOps Pipeline

```
1. Developer creates a branch: add-redis-cache
2. Developer opens a Pull Request (PR)
3. GitHub Actions triggers:
   ├── Runs `terraform fmt -check` (Checks code styling)
   └── Runs `terraform plan` (Posts the exact plan as a comment on the PR!)
4. Lead Engineer reviews the plan and clicks "Merge"
5. GitHub Actions triggers on `main`:
   └── Runs `terraform apply` to provision the real infrastructure!
```

---

## 📜 The GitHub Actions Workflow (`terraform.yml`)

```yaml
name: Terraform GitOps Pipeline

on:
  pull_request:
    branches: [ main ]
  push:
    branches: [ main ]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3

      - name: Terraform Init
        run: terraform init

      - name: Terraform Plan (On Pull Requests)
        if: github.event_name == 'pull_request'
        run: terraform plan -no-color

      - name: Terraform Apply (On Merge to Main)
        if: github.ref == 'refs/heads/main' && github.event_name == 'push'
        run: terraform apply -auto-approve
```

---

## 🎓 The Complete Master Pyramid: You Are Now Ready for Anything

Look at the complete stack you have assembled across all 7 guides:

```
                  ┌───────────────────────────────┐
                  │ 7. TERRAFORM (IaC & GitOps)   │  <-- Provisions Cloud Servers & Networks
                  ├───────────────────────────────┤
                  │ 6. KUBERNETES & HELM          │  <-- Orchestrates Fleets of Containers
                  ├───────────────────────────────┤
                  │ 5. PROMETHEUS & GRAFANA       │  <-- Monitors Health & Alerts
                  ├───────────────────────────────┤
                  │ 4. REVERSE PROXY (Caddy/Nginx)│  <-- Routes Domains & Free SSL
                  ├───────────────────────────────┤
                  │ 3. LINUX & SERVER SECURITY    │  <-- Operates & Hardens the OS
                  ├───────────────────────────────┤
                  │ 2. CI/CD (GitHub Actions)     │  <-- Automates Testing, Building & Deploying
                  ├───────────────────────────────┤
                  │ 1. DOCKER & DOCKER COMPOSE    │  <-- Packages Applications & Databases
                  └───────────────────────────────┘
```

Every single layer is **interconnected**, **production-tested**, and **completely free to learn and practice**. 

You now hold the keys to modern cloud engineering. Start with Guide 2, take your time, and enjoy the journey! 🚀
