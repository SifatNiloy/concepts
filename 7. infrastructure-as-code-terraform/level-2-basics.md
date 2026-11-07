# Level 2: Core Concepts & The 4 Sacred Commands

Terraform uses a human-readable language called **HCL (HashiCorp Configuration Language)**. Files end with `.tf`.

---

## 🏛️ The 3 Core Building Blocks

### 1. Provider
Tells Terraform which cloud service to talk to (AWS, Cloudflare, Docker, DigitalOcean, GitHub).
```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0"
    }
  }
}
```

### 2. Resource
The physical or virtual thing you want to create (a server, container, DNS record, S3 bucket).
```hcl
resource "docker_image" "nginx" {
  name = "nginx:alpine"
}

resource "docker_container" "web" {
  image = docker_image.nginx.image_id
  name  = "terraform-nginx"
  ports {
    internal = 80
    external = 8080
  }
}
```

### 3. Variables & Outputs
* **Variables:** Inputs so you don't hardcode passwords or server sizes.
* **Outputs:** Prints out the resulting IP address or URL after creation.

---

## ⚡ The 4 Sacred Commands

Every engineer follows this exact 4-step ritual:

| # | Command | What it does | Real-world Analogy |
|---|---|---|---|
| 1 | `terraform init` | Downloads the required cloud plugins/providers | Running `npm install` |
| 2 | `terraform plan` | Compares your code against the real world and shows what WILL happen (adds `+`, modifies `~`, deletes `-`) | Reviewing a git diff before commit |
| 3 | `terraform apply` | Actually contacts the cloud API and creates the resources | Shipping the change to production |
| 4 | `terraform destroy` | Deletes everything created by this code cleanly | Tearing down the building |

---

## 🎯 Hands-On: Test Terraform Locally with Docker (100% Free!)

You don't need a cloud account to learn Terraform! You can use Terraform to control your local Docker engine right now:

### Step 1: Install Terraform or OpenTofu
In PowerShell:
```powershell
winget install Hashicorp.Terraform
```
Verify:
```powershell
terraform -version
```

### Step 2: Create `main.tf`
Create a folder `terraform-practice` and add `main.tf`:
```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name = "nginx:alpine"
}

resource "docker_container" "nginx_server" {
  image = docker_image.nginx.image_id
  name  = "terraform-managed-nginx"
  ports {
    internal = 80
    external = 8085
  }
}
```

### Step 3: Run the 4 commands!
```powershell
terraform init
terraform plan
terraform apply -auto-approve
```

Now open: **`http://localhost:8085`**!  
Terraform just commanded Docker to download the image, map the port, and launch the container!

When done, tear it down cleanly:
```powershell
terraform destroy -auto-approve
```
*(The container and ports are deleted instantly)*

---

## ✅ Level 2 Checklist

- [ ] I understand Providers and Resources in HCL.
- [ ] I know the sequence: `init` $\rightarrow$ `plan` $\rightarrow$ `apply` $\rightarrow$ `destroy`.
- [ ] I used Terraform to manage a real Docker container!

👉 Open [level-3-intermediate.md](file:///d:/CODES/concepts/7.%20infrastructure-as-code-terraform/level-3-intermediate.md) to understand the **State File**!
