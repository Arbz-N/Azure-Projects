# 🌐 Azure Projects Repository

![Azure Projects](./assets/azure_projects_banner.png) 
*Professional Azure projects collection for hands-on learning, demos, and real-world implementations.*

---

## 🔹 Overview

This repository is a curated collection of **Microsoft Azure projects**, covering a wide range of services such as **Virtual Machines, Networking, Firewalls, Storage, Containers, AI, and more**.
The main goal of this repository is to provide **hands-on practical projects** to help learners, developers, and cloud enthusiasts understand and implement Azure services in real-world scenarios.
Each project is organized in its **own folder**, with step-by-step instructions, code snippets, and diagrams wherever necessary.

---

## 📂 Project List

Below is the list of projects included in this repository. New projects will be added sequentially with numbering:

1. **[Azure Networking & Firewall Demo](./AzureNetworking-Firewall-Demo)**  
   - **Description:** Demonstrates a Linux VM without a public IP connected via Azure Bastion, protected by Azure Firewall, with DNAT to access Nginx from the internet.  
   - **Key Services:** Virtual Machine, Azure Bastion, Azure Firewall, Nginx, DNAT.  
   - **Folder:** `AzureNetworking-Firewall-Demo`


---

## 🔹 How to Use

1. Clone the repository:

```bash
    git clone https://github.com/<YourUsername>/azure-projects.git
    Navigate to the project folder you want to try:
    cd AzureNetworking-Firewall-Demo
    Follow the instructions inside each project folder to deploy or run the project on Azure.


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