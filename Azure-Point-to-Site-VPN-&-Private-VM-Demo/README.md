# Azure Point-to-Site VPN & Private VM Demo

![Azure VPN Demo](./assets/azure_projects_banner.png)

---

## 🔹 Overview

This project demonstrates a **secure Azure networking architecture** where a Virtual Machine (VM) is deployed **without a public IP** and accessed **only via a Point-to-Site VPN** configured on an **Azure Virtual Network Gateway (VPN)**.  

The VM runs **Nginx** and serves a custom webpage. This setup ensures all traffic is routed securely through the VPN tunnel, providing **enhanced security** for private Azure resources.

---

## 🔹 Prerequisites:

Before starting this project, make sure you have:

- Active **Azure subscription** with permissions to create:
  - Resource Groups
  - Virtual Networks and Subnets.
  - VPN Gateways (Azure-VGW)
  - Virtual Machines
- Basic knowledge of **Linux commands**, SSH, and Azure Portal
- Certificate generation tools (**PowerShell**) for VPN authentication
- OpenVPN / IKEv2 compatible client installed locally

---

## 🔹 Project Components

| Component                  | Purpose |
|-----------------------------|---------|
| Virtual Network (VNet)      | Isolated Azure network for hosting private resources |
| Subnet (GatewaySubnet)      | Required subnet for VPN Gateway |
| Virtual Network Gateway (VPN) | Connects Azure VNet to individual clients securely via VPN |
| Point-to-Site VPN           | Secure tunnel (IKEv2/OpenVPN) for individual clients |
| Root Certificate            | Authenticates VPN clients |
| Virtual Machine (Linux)     | Runs Nginx server without public IP |

---

## 🔹 Step-by-Step Implementation

### 1️⃣ Create Virtual Network
- **Name:** VM-for-NGW  
- **Purpose:** Host internal Azure resources securely  
- **Subnets:**
  - `GatewaySubnet` → 10.0.1.0/24 (mandatory for VPN Gateway)  
- **Service Endpoints:** Microsoft.Web (for Nginx access)

### 2️⃣ Deploy Virtual Network Gateway (VPN)
- **Name:** Azure-VGW  
- **Region:** Central USA  
- **Gateway Type:** VPN  
- **SKU:** VpnGw2AZ  
- **Generation:** Generation2  
- **Advanced Connectivity:** Enabled  
- **Subnet Attached:** GatewaySubnet  
- **Public IPs:** Static (ip-for-vgw-1 & ip-for-vgw-2)  
- **Active-Active:** Disabled  
- **BGP:** Enabled (ASN 65515)  

> **Note:** Before creating the gateway, generate and export a root certificate for Point-to-Site VPN using PowerShell.  
> Official guide: [Azure VPN Certificates](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-certificates-point-to-site)

### 3️⃣ Configure Point-to-Site VPN
- **Address Pool:** 192.168.100.0/24  
- **Tunnel Type:** IKEv2  
- **Authentication Type:** Azure Certificate  
- **Public IP:** vpn-ip (for client connectivity)  
- **Root Certificate:** Upload base64-encoded public certificate  

### 4️⃣ Download & Connect VPN Client
1. Download the VPN client from Azure Portal  
2. Install as per system architecture  
3. Connect via Windows Settings → Network & Internet → VPN  

### 5️⃣ Create Linux VM (Private)
- Place VM in **default subnet** (no public IP)  
- Pass this **custom data script** to install and configure Nginx:

```bash
#!/bin/bash
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
sudo bash -c 'cat > /var/www/html/index.html <<EOF
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Secure Azure VM</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 50px; background-color: #f4f4f4; }
        h1 { color: #2c3e50; }
        p { color: #34495e; }
    </style>
</head>
<body>
    <h1>Welcome to My Secure Azure VM</h1>
    <p>This Nginx server is running on a private VM without public IP.</p>
    <p>Access is available only via your Point-to-Site VPN (IKEv2).</p>
</body>
</html>
EOF'
sudo systemctl restart nginx

6️⃣ Test Access

    Copy the private IP of the VM
    Open a browser → http://<VM_Private_IP>:80
    You should see the Nginx page confirming secure VPN access

🔹 Key Notes

    VM does not have a public IP, ensuring isolation
    Access is only via Point-to-Site VPN (IKEv2/OpenVPN)
    Root certificate ensures secure certificate-based authentication
    Nginx server demonstrates private service hosting in Azure