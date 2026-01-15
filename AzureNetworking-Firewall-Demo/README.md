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
and copy the index.html to /var/www/html/index.html 

8️⃣ Configure Firewall DNAT Rule

Go to Firewall → Firewall Policy → Rules → DNAT
Create Rule Collection: firewall-nginx-rule
Add Rule: nginx-rule
Source IP: Any or your public IP
Destination IP: Firewall Public IP
Protocol: TCP
Destination Port: 4000 (browser)
Translated Address: VM Private IP (10.0.0.4)
Translated Port: 80 (Nginx port)

Flow:
Internet → Firewall Public IP:4000 → DNAT → VM:80 → Response via Firewall

9️⃣ Test Access

Open browser → http://<Firewall_Public_IP>:4000
Nginx page should display:
This is a VM without a public IP, connected via Bastion and behind Azure Firewall

Azure Components Summary

  Component	                    Purpose
Resource Group	        Container for all resources
Virtual Network	        Logical isolated network
Subnets	                Default → VM, BastionSubnet → Bastion, FirewallSubnet → Firewall
Bastion Host	        Secure admin access without public IP
Linux VM	            Private server running Nginx
Azure Firewall	        Protects VNet, controls inbound/outbound traffic
DNAT Rule	            Exposes private VM service securely to internet


Key Notes

    Bastion: Admin access only, not for public web traffic
    Azure Firewall: Securely exposes services like Nginx
    DNAT: Maps firewall public port → VM private port
    Priority Rules: Determines traffic matching order


Cleanup / Delete Resources
To avoid extra charges, delete all Azure resources created for this project in the correct order
1. Virtual Machine (VM)
2. Public IPs (if any assigned manually)
3. Azure Bastion
4. Azure Firewall
5. Subnets
6. Virtual Network (VNet)
7. Resource Group