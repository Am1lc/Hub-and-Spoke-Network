# Multi-Region Secure Hub-and-Spoke Azure Architecture

## 📌 Project Overview
This project demonstrates an enterprise-grade, highly secure Hub-and-Spoke network topology deployed across multiple Azure regions. By decoupling application workloads from the public internet, this architecture enforces a Zero-Trust security model. All outbound traffic from isolated application subnets is hijacked and forced through a centralized Azure Firewall for deep packet inspection and network egress control.


## 🏗️ Technical Architecture & Step-by-Step Lab Execution

### Step 1: Resource Group & Core Virtual Networks
I initialized a unified resource container (`Prod-RG`) and engineered distinct address layers for our network core.
* **Hub-VNet (East US):** Configured with a `10.0.0.0/16` space, hosting the dedicated `AzureFirewallSubnet` (`10.0.1.0/26`).
* **EastUS2-Spoke-VNet (East US 2):** Configured with an isolated `172.30.0.0/16` address space hosting our `Workload-Subnet` (`172.30.1.0/24`) to completely eliminate cross-network address space overlaps.

> **Proof of Deployment:**
> *Drop your network creation or resource group screenshot here*
<img width="781" height="870" alt="Screenshot 2026-07-21 192950" src="https://github.com/user-attachments/assets/19712d85-2275-4442-ad73-d49b7da9b671" />
<img width="1004" height="871" alt="Screenshot 2026-07-21 220417" src="https://github.com/user-attachments/assets/08c1877b-d001-4826-8cb5-9b4c395593d1" />
<img width="1043" height="871" alt="Screenshot 2026-07-21 222139" src="https://github.com/user-attachments/assets/de9161d3-844b-437c-bea6-e25c1ccb49c5" />



---

### Step 2: Global Virtual Network Peering
To connect the disparate regions over Microsoft's high-speed private fiber backbone, I established a bidirectional **Global VNet Peering** bridge (`Hub-to-EastUS2-Spoke` and `EastUS2-Spoke-to-Hub`), ensuring full cross-region traffic forwarding capabilities.

> **Proof of Connection:**
> *Drop your VNet Peering "Connected" status screenshot here*
<img width="1043" height="871" alt="Screenshot 2026-07-21 222139" src="https://cloud.githubusercontent.com/assets/142388129//1f6192ab-57cf-46f3-971d-3988b52475a1.png" />


---

### Step 3: Centralized Azure Firewall
I provisioned a Standard Azure Firewall (`Central-Firewall`) attached to a static public IP (`FW-Public-IP`) inside the Hub network. This gateway is tied to a custom `FW-Policy` that acts as the centralized traffic rulebook for inspecting all enterprise egress data.

> **Proof of Security Gateway:**
> *Drop your Azure Firewall overview screenshot here*
<img width="1428" height="869" alt="Screenshot 2026-07-22 140446" src="https://github.com/user-attachments/assets/f2c8474b-ca07-40dc-a37b-55567e6138ba" />


---

### Step 4: Zero-Trust Compute Provisioning & Capacity Troubleshooting
Deployed a secure Linux server (`Spoke-Web-VM-02`) using an `F1als_v7` compute footprint inside the isolated spoke network. 
* **Security Hardening:** Stripped all public IP addresses and locked inbound ports strictly to `None` to prevent direct internet threat exposure.
* **Troubleshooting Scenario:** Successfully diagnosed datacenter allocation failures (`AllocationFailed` faults due to trial subscription quotas) in alternative zones and dynamically re-mapped the entire deployment region and IP schema to guarantee service availability.

> **Proof of VM Isolation:**
> *Drop your Virtual Machine networking tab screenshot here*
<img width="920" height="869" alt="Screenshot 2026-07-22 121129" src="https://github.com/user-attachments/assets/22164f21-850d-4ed6-a7c4-a12df489a8ba" />


---

### Step 5: User Defined Routing (UDR) Traffic Hijack
I created a custom Azure Route Table (`Spoke-To-Firewall-Route`) in East US 2 and populated it with a `0.0.0.0/0` User Defined Route. This next-hop rule forces all outgoing traffic from the server's subnet directly into the Firewall's internal IP (`10.0.1.4`) for deep packet inspection before hitting the internet.

> **Proof of Route Configuration:**
> *Drop your Route Table routes & subnet association screenshots here*
<img width="1045" height="869" alt="Screenshot 2026-07-22 201750" src="https://github.com/user-attachments/assets/e3bce74a-f093-4604-bcb5-333f761e7b4a" />
<img width="1083" height="869" alt="Screenshot 2026-07-22 202529" src="https://github.com/user-attachments/assets/1c0f7cbb-b28b-4e4b-8362-f74d5e392b37" />


---

### 🚀 Key Technical Takeaways for Recruiters
* **Zero-Trust Implementation:** Absolute separation of backend compute from the public web.
* **Network Engineering:** Hands-on experience with CIDR block calculations, routing tables, and firewalls.
* **Production Problem Solving:** Proven ability to troubleshoot real-world datacenter resource limits and adapt infrastructure on the fly.
