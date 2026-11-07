# Level 3: The State File (`terraform.tfstate`) & Drift

How does Terraform know which server belongs to your code, and which server belongs to another project?  
Through the **State File (`terraform.tfstate`)**.

---

## 🧠 The Brain of Terraform: `terraform.tfstate`

When you run `terraform apply`, Terraform creates a JSON file called `terraform.tfstate`.

* It records the exact IDs, IP addresses, and metadata of everything created.
* **The Golden Rule:** Never edit `terraform.tfstate` by hand!
* **Security Rule:** Never commit `terraform.tfstate` to public Git repositories (it can contain sensitive database passwords in plain text). Put it in `.gitignore`!

---

## 🌪️ What is "Configuration Drift"?

Imagine you configured a server with port 80 and 443 open in your Terraform code.  
One afternoon, a coworker logs into the AWS console by hand and accidentally deletes port 443!

The real world is now **out of sync** with your code. This is called **Drift**.

### How Terraform fixes Drift:
When you run:
```powershell
terraform plan
```
Terraform contacts the cloud, checks the real world, notices port 443 is missing, and says:
> *"Drift detected! The real world has changed. I will re-add port 443 to match your code."*

Then you run `terraform apply`, and Terraform automatically restores the missing rule!

---

## ✅ Level 3 Checklist

- [ ] I understand why `terraform.tfstate` is the single source of truth.
- [ ] I know why the state file belongs in `.gitignore`.
- [ ] I understand what Configuration Drift is and how `terraform plan` detects it.

👉 Open [level-4-real-world.md](file:///d:/CODES/concepts/7.%20infrastructure-as-code-terraform/level-4-real-world.md) to manage cloud resources for free!
