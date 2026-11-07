# Level 1: What is Infrastructure as Code (IaC)?

## 🖱️ "ClickOps" vs The Architect's Code

Imagine you want to set up an infrastructure:
1. 2 Ubuntu cloud servers.
2. 1 PostgreSQL cloud database.
3. 1 Firewall allowing ports 80, 443, and 22.
4. 1 DNS record pointing `sifat.dev` to Server #1.

### The Old Way ("ClickOps"):
You log into AWS or Oracle Cloud with your web browser. You click through 47 different menus, checkboxes, dropdowns, and forms.
* **The Disaster:** What happens if you need to create the exact same setup for a **Staging environment** tomorrow? Or what if your account gets hacked and you have to rebuild it from scratch? You have to remember and re-click all 47 menus! If you forget one firewall rule, everything breaks.

### The Modern Way (Infrastructure as Code - IaC):
Instead of clicking buttons in a browser, you write a **declarative text file**:

```hcl
resource "server" "my_app_server" {
  os     = "ubuntu-24.04"
  memory = "4GB"
  ports  = [80, 443, 22]
}
```

You run one command:
```powershell
terraform apply
```
The cloud provider automatically provisions your servers, firewalls, and databases in 60 seconds.

---

## 🌟 Why IaC is Required in Every Senior Job

1. **Version Control:** Your infrastructure is committed to Git. You can see who changed a firewall port 6 months ago with `git blame`!
2. **Instant Rebuilding:** If an entire region goes down, one command recreates your whole company infrastructure in another country.
3. **Cost Control:** When you're done testing for the day, type `terraform destroy` and all paid servers vanish so you don't get billed!

---

## ✅ Level 1 Checklist

- [ ] I understand what "ClickOps" is and why clicking menus in cloud consoles is dangerous.
- [ ] I understand how Infrastructure as Code (IaC) stores servers and networks in Git.
- [ ] I know why Terraform/OpenTofu is the industry standard.
