# Azure Networking & Firewall Demo

![Azure Networking](./assets/azure_projects_banner.png)  

---

## 🔹 Overview

This project demonstrates a **secure Azure networking architecture** where a Linux Virtual Machine (VM) is deployed **without a public IP** and managed securely via **Azure Bastion**. The VM is protected by **Azure Firewall**, and services like **Nginx** can be accessed from the internet using **DNAT rules**, without exposing the VM directly.

This setup is ideal for understanding **secure cloud networking, firewall policies, and private VM access** in Azure.

---

## 🔹 Prerequisites

Before starting this project, ensure you have:

- An active **Azure subscription**
- Permissions to create:
  - Resource Groups
  - Virtual Networks and Subnets
  - Virtual Machines
  - Public IPs
  - Azure Bastion and Azure Firewall
- Basic knowledge of **Linux commands**, SSH, and **Azure Portal**
- A browser for testing Nginx access

---

## 🔹 Project Steps & Explanation

### 1️⃣ Create Resource Group
- **Location:** Australia East
- **Purpose:** Logical container for all resources (VNets, VM, Bastion, Firewall)

### 2️⃣ Create Virtual Network (VNet)
- **Address space:** 10.0.0.0/16
- **Subnets:**
  - `default` → 10.0.0.0/24 (VMs)
  - `AzureBastionSubnet` → 10.0.1.0/26 (Bastion host)
  - `AzureFirewallSubnet` → 10.0.1.64/26 (Azure Firewall)

### 3️⃣ Deploy Azure Bastion
- Provides **secure RDP/SSH access** to VMs **without public IPs**
- Flow: `User → Bastion → VM`
- Deployed in `AzureBastionSubnet`

### 4️⃣ Deploy Azure Firewall
- **Type:** Standard
- **Purpose:** Protect VNet, manage inbound/outbound traffic
- **Firewall Policy:** `practice-VN-firewall-policy1`
- Supports **DNAT**, **SNAT**, and **application rules**

### 5️⃣ Create Linux VM
- **No Public IP**
- Connected to `default` subnet
- OS: Linux
- Runs **Nginx** for demonstration
- Example Private IP: 10.0.0.4

### 6️⃣ Connect VM via Bastion
- Go to VM → Connect → Bastion → SSH
- Provides **secure admin access** without exposing VM

### 7️⃣ Install Nginx
SSH into VM via Bastion and run:

```bash
sudo apt update
sudo apt install nginx -y
sudo tee /var/www/html/index.html <<EOF
<h1>This is a VM without a public IP, connected via Bastion and behind Azure Firewall</h1>
EOF