# Level 5: Reusable Modules & Environments (Dev vs Prod)

In professional codebases, you don't copy-paste 200 lines of Terraform to create a Development server and a Production server.  
You create a **Terraform Module**.

---

## 🧩 What is a Module?

Think of a Module like a **function in JavaScript**:
* It takes **inputs** (`server_size`, `environment_name`).
* It creates the resources.
* It returns **outputs** (`server_ip`).

### Directory Structure of a Pro IaC Project:
```text
my-iac-project/
├── modules/
│   └── web_server/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
├── environments/
│   ├── dev/
│   │   └── main.tf   <-- Calls module with server_size = "1GB"
│   └── prod/
│       └── main.tf   <-- Calls module with server_size = "8GB"
```

---

## 📝 Calling the Module in `environments/prod/main.tf`:

```hcl
module "production_website" {
  source           = "../../modules/web_server"
  environment_name = "production"
  server_size      = "8GB"
  instance_count   = 3
}
```

Now, scaling or updating an entire environment takes **1 line of code**!

---

## ✅ Level 5 Checklist

- [ ] I understand how Terraform Modules eliminate copy-paste code.
- [ ] I can separate `dev` and `prod` configurations using variables and modules.

👉 Open [level-6-pro.md](file:///d:/CODES/concepts/7.%20infrastructure-as-code-terraform/level-6-pro.md) for automated GitOps pipelines!
