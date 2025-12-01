# Azure VM Deployment with Terraform

**Project by Oluwatobi Ojuade**

This project provisions a complete Azure infrastructure using Terraform, including a virtual network, public/private subnets, network security groups, an Ubuntu VM, and an NGINX web server. Everything is automated using Infrastructure-as-Code and designed following Azure architectural best practices.

---

## 🌐 Architecture Diagram
```
+---------------------------- Azure Subscription -----------------------------+
|                                                                             |
|   +-------------------------- Resource Group ---------------------------+   |
|   | rg-terraform-demo                                                   |   |
|   |                                                                     |   |
|   |   +---------------------- Virtual Network ---------------------+   |   |
|   |   | vnet-terraform-demo (10.0.0.0/16)                          |   |   |
|   |   |                                                            |   |   |
|   |   |  +-------------+         +-----------------------------+   |   |   |
|   |   |  | Subnet      |         | Subnet                      |   |   |   |
|   |   |  | private     |         | public (NSG attached)       |   |   |   |
|   |   |  | 10.0.2.0/24 |         | 10.0.1.0/24                 |   |   |   |
|   |   |  +-------------+         +-----------------------------+   |   |   |
|   |   |                                                            |   |   |
|   |   +------------------------------------------------------------+   |   |
|   |                                                                     |   |
|   |   +------------------------ VM (NGINX) -----------------------+    |   |
|   |   | vm-web-demo                                               |    |   |
|   |   | Ubuntu 20.04 (B1s SKU)                                    |    |   |
|   |   | Public IP: Assigned                                       |    |   |
|   |   | NIC protected by NSG (allow SSH/HTTP)                     |    |   |
|   |   +-----------------------------------------------------------+    |   |
|   +---------------------------------------------------------------------+   |
|                                                                             |
+-----------------------------------------------------------------------------+
```

---

## 🧱 Terraform Folder Structure
```
/terraform
├── provider.tf
├── network.tf
├── compute.tf
├── variables.tf
├── outputs.tf
└── terraform.tfvars
```

**Descriptions:**

- **provider.tf** – AzureRM provider + features
- **network.tf** – VNet, subnets, NSG
- **compute.tf** – VM, public IP, NIC
- **variables.tf** – Input variables
- **outputs.tf** – VM IP & resource outputs
- **terraform.tfvars** – Environment config (excluded from GitHub)

---

## ⚙️ Deployment Steps

### 1. Initialize Terraform
```bash
terraform init
```

### 2. Preview execution plan
```bash
terraform plan
```

### 3. Deploy infrastructure
```bash
terraform apply
```

### 4. SSH into the VM
```bash
ssh -i azure-key azureuser@<public-ip>
```

### 5. Install & start NGINX
```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 6. Edit the NGINX HTML page
```bash
sudo nano /var/www/html/index.nginx-debian.html
```

---

## 🖥️ NGINX Page (Edited)

Your VM serves a custom webpage:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Oluwatobi, Milestone Cloud Demo</title>
</head>
<body>
    <h1>Welcome to my Azure VM running NGINX!</h1>
    <p>Deployed using Terraform</p>
</body>
</html>
```

---

## 🖼️ Screenshots Included in This Project

The following images are stored in `/screenshots/`:

- `azure-resource-group.png`
- `virtual-network-overview.png`
- `subnets.png`
- `vm-details-page.png`
- `ssh-into-vm.png`
- `azure-vm-running-nginx.png`
- `terraform-apply.png`

---

## 🔥 Challenges & Troubleshooting

### 1. VM SKU Capacity Restrictions

**Issue:** Azure region "East US" returned:
```
SkuNotAvailable: Standard_B1ls not available
```

**Fix:** Switched to East US 2, which supports B-series SKUs.

---

### 2. Resource Group Could Not Delete

**Issue:** Terraform blocked deletion due to remaining nested resources.

**Fix:** Added:
```hcl
features {
  resource_group {
    prevent_deletion_if_contains_resources = false
  }
}
```

---

### 3. Provider Inconsistent State Errors

**Issue:** AzureRM provider returned:
```
unexpected new value: Root object was present but now absent
```

**Fix:** Reinitialized Terraform, destroyed state, and redeployed cleanly.

---

### 4. SSH Identity File Not Found

**Issue:** Error:
```
Identity file azure-key.pem not accessible
```

**Fix:** Used correct filename (`azure-key`) and verified directory path.

---

### 5. NGINX Page Not Updating

**Issue:** HTML edits didn't show due to caching.

**Fix:** Saved via `CTRL+O` → `ENTER` → `CTRL+X` and refreshed browser.

---

## 🧠 Skills Demonstrated

- Azure Compute (VMs)
- Azure Virtual Networks
- Subnetting & IP Addressing
- Network Security Groups
- Public IP management
- SSH access and Linux administration
- NGINX configuration
- Terraform IaC patterns
- State management & debugging
- Real Azure troubleshooting scenarios
- GitHub project organization
- Clean technical documentation

---

## 📫 Contact

**Oluwatobi Ojuade**  
[LinkedIn](https://linkedin.com/in/your-profile) | [GitHub](https://github.com/your-username)
