# Level 4: Managing Real Cloud Resources for Free

Now that you know how Terraform controls local Docker, let's look at managing real internet services without spending money.

---

## ☁️ Managing Cloudflare DNS with Terraform (100% Free)

If you own a domain (even a free domain or a cheap \$1 domain), you can manage your entire DNS and DDoS configuration using Terraform instead of the Cloudflare website!

### Step 1: Configure the Cloudflare Provider
```hcl
terraform {
  required_providers {
    cloudflare = {
      source  = "cloudflare/cloudflare"
      version = "~> 4.0"
    }
  }
}

provider "cloudflare" {
  api_token = var.cloudflare_api_token
}

variable "cloudflare_api_token" {
  type      = string
  sensitive = true
}

variable "zone_id" {
  type = string
}
```

### Step 2: Define your DNS Records as Code
```hcl
# Point mysite.com to your VPS IP:
resource "cloudflare_record" "root" {
  zone_id = var.zone_id
  name    = "@"
  value   = "123.45.67.89"
  type    = "A"
  proxied = true # Turns on Cloudflare DDoS protection & CDN!
}

# Create a CNAME for the API:
resource "cloudflare_record" "api" {
  zone_id = var.zone_id
  name    = "api"
  value   = "mysite.com"
  type    = "CNAME"
  proxied = true
}
```

### Step 3: Apply:
```powershell
terraform apply
```
Terraform automatically creates the DNS records and turns on the orange DDoS shield on Cloudflare!

---

## 🧪 Testing AWS Locally with LocalStack (100% Free)

What if you want to learn AWS (S3 buckets, EC2, IAM roles) with Terraform without giving Amazon your credit card?

Use **LocalStack**!
* LocalStack is a **mock AWS cloud running inside a Docker container** on your laptop.
* Terraform talks to `http://localhost:4566` thinking it's the real AWS!

```powershell
docker run -d -p 4566:4566 -p 4510-4559:4510-4559 --name localstack localstack/localstack
```

Now you can create simulated AWS S3 buckets, queues, and databases using Terraform for \$0!

---

## ✅ Level 4 Checklist

- [ ] I can manage Cloudflare DNS records with Terraform.
- [ ] I understand how LocalStack simulates real AWS clouds inside Docker for free.

👉 Open [level-5-advanced.md](file:///d:/CODES/concepts/7.%20infrastructure-as-code-terraform/level-5-advanced.md) for Reusable Modules & Environments!
