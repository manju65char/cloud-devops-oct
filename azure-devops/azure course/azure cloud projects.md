# Azure Administrator — Project Index & Solution Guide
> **Level:** Azure Administrator (AZ-104 aligned)
> **Updated:** June 2026

---

## 📋 Project Index

| # | Project | Core Azure Services |
|---|---------|-------------------|
| 1 | [Creating VM in Azure Cloud](#1-creating-vm-virtual-machine-in-azure-cloud) | Azure Compute, Availability Sets/Zones |
| 2 | [VNet Peering between two VNets](#2-vnet-peering-between-two-vnets) | Azure Virtual Network |
| 3 | [Taking Backup of VM in Azure](#3-taking-backup-of-vm-in-azure) | Recovery Services Vault, Azure Backup |
| 4 | [Site Recovery — Disaster Recovery](#4-site-recovery--disaster-recovery-in-azure) | Azure Site Recovery, RSV |
| 5 | [Load Balancer in Action](#5-load-balancer-in-action) | Azure Load Balancer, IIS |
| 6 | [Virtual Machine Scale Set (VMSS)](#6-virtual-machine-scale-set-vmss) | VMSS, Auto-scale |
| 7 | [Azure Storage Account](#7-azure-storage-account) | Blob, LRS/GRS, Storage Explorer |
| 8 | [Azure VPN — Site-to-Site](#8-azure-vpn--site-to-site) | VPN Gateway, Fortigate |
| 9 | [Azure AD Connect](#9-azure-ad-connect) | Azure AD, AD Connect, SSO |
| 10 | [Azure Firewall & NSG](#10-azure-firewall--nsg) | Azure Firewall, NSG, UDR |
| 11 | [Azure Monitor & Log Analytics](#11-azure-monitor--log-analytics) | Monitor, Log Analytics, Alerts |
| 12 | [Azure Key Vault](#12-azure-key-vault) | Key Vault, Managed Identity |
| 13 | [Azure Policy & Governance](#13-azure-policy--governance) | Azure Policy, Blueprints, Management Groups |
| 14 | [Azure Kubernetes Service (AKS)](#14-azure-kubernetes-service-aks) | AKS, ACR, kubectl |
| 15 | [Azure App Service & Web Apps](#15-azure-app-service--web-apps) | App Service, Deployment Slots |

---

## 1. Creating VM (Virtual Machine) in Azure Cloud

### 🎯 Real-Time Use Case
> A software company wants to host its internal HR application on Azure instead of an on-premises server. The admin must spin up Windows Server VMs with the right size, region, and redundancy.

### 📌 Steps
1. **Select the Proper Region** — Choose a region close to your users (e.g., `East US`, `Central India`) to minimize latency.
2. **Choose Availability Set / Availability Zone**
   - *Availability Set*: Protects against rack-level failures (for same-datacenter HA)
   - *Availability Zone*: Protects against datacenter failures (separate physical locations)
3. **Choose the Right VM Size**
   - `B2s` → Dev/Test
   - `D4s_v5` → General production workloads
   - `E8s_v5` → Memory-intensive (SQL, SAP)
4. **Configure OS Disk & Data Disk** — Use Premium SSD for production.
5. **Configure Networking** — Assign to a VNet/Subnet, configure NSG rules.
6. **Take RDP/SSH Access** — RDP for Windows (`port 3389`), SSH for Linux (`port 22`).
7. **Enable Boot Diagnostics & Monitoring**.

### ✅ Solution Architecture
```
User → Azure Portal → VM (Windows/Linux)
                    ├── VNet + Subnet
                    ├── NSG (allow RDP/SSH)
                    ├── Availability Zone (Zone 1/2/3)
                    └── Premium SSD OS Disk
```

### 💡 Pro Tips
- Always use **Spot Instances** for dev/test to save up to 90% cost.
- Tag VMs with `Environment`, `Owner`, `CostCenter` for governance.
- Use **Azure Bastion** instead of direct RDP/SSH for secure access (no public IP needed).

---

## 2. VNet Peering Between Two VNets

### 🎯 Real-Time Use Case
> A company has two departments — HR (VNet-HR: `10.1.0.0/16`) and Finance (VNet-Finance: `10.2.0.0/16`). They need to communicate internally without going over the public internet.

### 📌 Steps
1. **Create two VNets in the same region** (or different regions for Global Peering)
   - VNet-A: `10.1.0.0/16`
   - VNet-B: `10.2.0.0/16`
2. **Initiate Peering from VNet-A → VNet-B** (Azure Portal → VNet-A → Peerings → Add)
3. **Peering is automatically reciprocated** in the same subscription (or manually accept in different subscriptions).
4. **Verify Peering Status** → Should show `Connected`.
5. **Test connectivity** using `ping` or `Test-NetConnection` between VMs in each VNet.

### ✅ Solution Architecture
```
VNet-A (10.1.0.0/16)  ←——— VNet Peering ———→  VNet-B (10.2.0.0/16)
  └── VM-A (10.1.0.4)                              └── VM-B (10.2.0.4)
       ↕ Private traffic flows directly ↕
```

### 💡 Pro Tips
- VNet address spaces **must not overlap**.
- Enable **"Allow forwarded traffic"** and **"Allow gateway transit"** for hub-spoke topologies.
- Use **Global VNet Peering** to peer VNets across regions (e.g., East US ↔ West Europe).

---

## 3. Taking Backup of VM in Azure

### 🎯 Real-Time Use Case
> A retail company's e-commerce VM hosts critical order data. Compliance requires daily backups retained for 30 days and weekly backups for 12 weeks. An accidental file deletion must be recoverable within hours.

### 📌 Steps
1. **Create a VM** in Azure Cloud.
2. **Create a Recovery Services Vault (RSV)** — in the same region as the VM.
3. **Create a Backup Policy**
   - Schedule: Daily at 11:00 PM
   - Retention: 30 days daily / 12 weeks weekly / 12 months monthly
4. **Associate the VM with the Backup Policy** → Enable Backup.
5. **Trigger an on-demand backup** to validate.
6. **Restore a File from Backup** → Mount recovery point → Browse files → Copy needed file.
7. **Restore entire VM from Backup** → Create new VM from restore point.

### ✅ Solution Architecture
```
Azure VM  ──→  Recovery Services Vault
                    └── Backup Policy
                          ├── Daily (30 days)
                          ├── Weekly (12 weeks)
                          └── Monthly (12 months)
                    └── Restore Options
                          ├── File-level restore
                          └── Full VM restore
```

### 💡 Pro Tips
- Enable **Soft Delete** on RSV to protect against accidental vault deletion (14-day grace period).
- Use **Cross-Region Restore (CRR)** to restore VMs in a paired region during regional outage.
- Monitor backup jobs using **Azure Monitor Alerts** on RSV.

---

## 4. Site Recovery — Disaster Recovery in Azure

### 🎯 Real-Time Use Case
> A banking application runs in East US. Regulatory compliance requires a Recovery Time Objective (RTO) of < 2 hours and Recovery Point Objective (RPO) of < 15 minutes. If East US goes down, the app must failover to West US automatically.

### 📌 Steps
1. **Create primary VM** in East US (Source Region).
2. **Create Recovery Services Vault in West US** (Target/Secondary Region).
3. **Create a Replication Policy**
   - RPO threshold: 15 minutes
   - Recovery point retention: 24 hours
   - App-consistent snapshot frequency: 4 hours
4. **Enable Replication** → Source: VM in East US → Target: West US.
5. **Monitor Replication Health** → Wait for status `Protected`.
6. **Run Test Failover** to validate without impacting production.
7. **Perform Failover** (planned or unplanned) from Primary → Secondary Region.
8. **Reprotect** after failover to replicate back (failback-ready).

### ✅ Solution Architecture
```
Primary Region (East US)          Secondary Region (West US)
  └── VM (Production)  ──ASR──→   └── VM (Replica - standby)
  └── VNet-Primary                └── VNet-Secondary
  └── RSV (Replication Source)    └── RSV (Target Vault)

Failover Trigger → Azure Portal / Recovery Plan / Automation
```

### 💡 Pro Tips
- Use **Recovery Plans** to automate failover of multi-tier apps (Web → App → DB in order).
- Test failover uses an **isolated network** — zero production impact.
- Integrate with **Azure Automation Runbooks** for pre/post failover scripts.

---

## 5. Load Balancer in Action

### 🎯 Real-Time Use Case
> An e-commerce website receives 50,000 concurrent users during a sale event. Two IIS web servers must share traffic equally. If one VM goes down, all traffic routes to the healthy VM automatically.

### 📌 Steps
1. **Create 2 Windows VMs** in the same Availability Set (for HA).
2. **Install IIS Role** on both VMs: `Install-WindowsFeature -name Web-Server -IncludeManagementTools`
3. **Customize default web pages** to identify VM1 vs VM2 in responses.
4. **Create an Azure Load Balancer** (Standard SKU, Public)
   - Frontend IP: Public IP address
   - Backend Pool: Add both VMs
   - Health Probe: HTTP on port 80 every 5 seconds
   - Load Balancing Rule: TCP port 80 → Backend port 80
5. **Test Load Balancing** → Refresh browser → Responses alternate between VM1 and VM2.
6. **Stop one VM** → All traffic goes to remaining healthy VM (HA validated).

### ✅ Solution Architecture
```
Internet → Public IP → Azure Load Balancer (Standard)
                          ├── Health Probe (HTTP:80)
                          ├── VM-1 (IIS) — Availability Set
                          └── VM-2 (IIS) — Availability Set
```

### 💡 Pro Tips
- Use **Standard Load Balancer** (not Basic) for production — supports Availability Zones.
- Enable **Session Persistence** (sticky sessions) for stateful apps.
- For HTTPS apps, use **Azure Application Gateway** (Layer 7) with WAF instead.

---

## 6. Virtual Machine Scale Set (VMSS)

### 🎯 Real-Time Use Case
> A video streaming platform sees 10x traffic during weekend evenings. The platform needs to auto-scale from 2 VMs (minimum) to 20 VMs (maximum) based on CPU load, and scale back down when traffic drops.

### 📌 Steps
1. **Create a VM Scale Set** (Uniform or Flexible orchestration)
   - Base Image: Windows Server / Ubuntu
   - Initial count: 2
2. **Define Scaling Policy**
   - Scale Out: When CPU > 75% for 5 minutes → Add 2 instances
   - Scale In: When CPU < 25% for 10 minutes → Remove 1 instance
3. **Set Minimum (2) and Maximum (20) VM count**.
4. **Install application** via Custom Script Extension or VM Image.
5. **Apply CPU Stress** using tools like `stress` (Linux) or `CPUSTRES.exe` (Windows).
6. **Observe auto-scaling** in Azure Portal → VMSS → Instances tab.

### ✅ Solution Architecture
```
Load Balancer → VMSS (2–20 instances)
                    └── Auto-scale Rules
                          ├── Scale Out: CPU > 75%
                          └── Scale In: CPU < 25%
                    └── Custom Script Extension (App Install)
```

### 💡 Pro Tips
- Use **Flexible Orchestration** for mixing VM sizes and AZs.
- Combine VMSS with **Azure Load Balancer** or **Application Gateway**.
- Use **custom VM images** (via Azure Compute Gallery) for faster scale-out.

---

## 7. Azure Storage Account

### 🎯 Real-Time Use Case
> A media company stores millions of video/image assets. They need a storage solution that is cost-effective, regionally redundant, and accessible to developers via Storage Explorer and applications via SDK.

### 📌 Steps
1. **Select Region** — Choose the region closest to your application.
2. **Select Performance Tier**
   - *Standard*: HDD-backed, cost-effective (general purpose)
   - *Premium*: SSD-backed, low-latency (databases, high-throughput)
3. **Select Replication Type**
   - `LRS` (Locally Redundant) → 3 copies in 1 datacenter
   - `ZRS` (Zone Redundant) → 3 copies across 3 AZs
   - `GRS` (Geo-Redundant) → 6 copies across 2 regions
   - `GZRS` (Geo-Zone Redundant) → Best protection
4. **Create Blob Container** → Upload media files.
5. **Configure Access Tiers**: Hot → Warm → Cool → Archive (based on access frequency).
6. **Access via Azure Storage Explorer** → Connect using connection string or Azure AD.

### ✅ Solution Architecture
```
Application / Storage Explorer
        ↓
Azure Storage Account
    ├── Blob Storage (images, videos, backups)
    ├── File Share (SMB mount for VMs)
    ├── Queue Storage (async messaging)
    └── Table Storage (NoSQL key-value)
Replication: GRS (primary East US + secondary West US)
```

### 💡 Pro Tips
- Use **Lifecycle Management Policies** to auto-move blobs from Hot → Cool → Archive.
- Enable **Soft Delete** for blobs (retain deleted blobs for N days).
- Use **Private Endpoints** to keep storage traffic off the public internet.

---

## 8. Azure VPN — Site-to-Site

### 🎯 Real-Time Use Case
> A manufacturing company has an on-premises datacenter in Bangalore and Azure VNet in Central India. Employees must securely access Azure resources as if they were on the local network.

### 📌 Steps
1. **Create Azure VPN Gateway** in Azure (GatewaySubnet required)
   - SKU: `VpnGw1` or higher for production
   - Type: Route-based VPN
2. **Create a Local Network Gateway** → Represents on-prem network (Fortigate IP + address space)
3. **Configure Fortigate Firewall** on-premises as VPN peer
   - IKEv2, Pre-Shared Key (PSK)
   - Define Azure VNet as remote subnet
4. **Create VPN Connection** in Azure → Link VPN Gateway to Local Network Gateway
5. **Check VPN Tunnel Status** → Azure Portal → Connections → Status = `Connected`
6. **Test connectivity** → Ping Azure VM from on-prem machine using private IP.

### ✅ Solution Architecture
```
On-Premises Network (192.168.0.0/16)
  └── Fortigate Firewall ──── IPSec/IKEv2 Tunnel ────→ Azure VPN Gateway
                                                          └── VNet (10.0.0.0/16)
                                                               └── Azure VMs
```

### 💡 Pro Tips
- Use **BGP (Border Gateway Protocol)** on the VPN for dynamic routing.
- For higher bandwidth/reliability use **ExpressRoute** (private dedicated circuit) instead of VPN.
- Deploy **Active-Active VPN Gateway** for zero-downtime HA.

---

## 9. Azure AD Connect

### 🎯 Real-Time Use Case
> A 500-employee enterprise has all users in on-premises Active Directory. They're migrating to Microsoft 365. Users should log into Azure/M365 with their existing corporate credentials (SSO) without managing two sets of passwords.

### 📌 Steps
1. **Create a Domain Controller** on-premises (or in Azure as a VM)
   - AD Domain: `corp.contoso.com`
   - Create sample users in AD
2. **Install Azure AD Connect** on the Domain Controller (or member server)
   - Download from Microsoft: `https://www.microsoft.com/en-us/download/details.aspx?id=47594`
3. **Configure AD Connect**
   - Select sync method: *Password Hash Sync* (recommended) or *Pass-through Auth* or *Federation (ADFS)*
   - Select OUs to sync
4. **Sync Domain Controller users to Azure AD** → Run initial sync: `Start-ADSyncSyncCycle -PolicyType Initial`
5. **Verify** users appear in Azure AD (Entra ID).
6. **Test Single Sign-On (SSO)** → User logs into Azure Portal with corporate credentials.

### ✅ Solution Architecture
```
On-Premises AD (corp.contoso.com)
  └── Domain Controller
        └── Azure AD Connect ──sync──→ Azure AD (Entra ID)
                                          └── Microsoft 365
                                          └── Azure Portal (SSO)
```

### 💡 Pro Tips
- **Password Hash Sync** is the simplest and most resilient (works even if on-prem AD is down).
- Enable **Seamless SSO** so domain-joined machines auto-login without password prompts.
- Monitor sync health via **Azure AD Connect Health** dashboard.

---

## 10. Azure Firewall & NSG

### 🎯 Real-Time Use Case
> A fintech company needs centralized, stateful firewall protection for all Azure VNets. Specific VMs should only allow traffic on approved ports, and all outbound internet traffic must be logged and filtered.

### 📌 Steps
1. **Create Azure Firewall** in a hub VNet (Hub-Spoke topology)
   - Deploy in `AzureFirewallSubnet`
   - Enable DNS proxy
2. **Create Firewall Policy** with rules
   - Application Rules: Allow `*.microsoft.com`, `*.windowsupdate.com`
   - Network Rules: Allow TCP 443 from spoke VNets
   - DNAT Rules: Translate public IP → private VM IP
3. **Create Route Table (UDR)** → Force all spoke traffic through Firewall (`0.0.0.0/0 → Firewall private IP`)
4. **Configure NSG** on each subnet → Allow only required ports (deny all by default)
5. **Enable Azure Firewall Diagnostic Logs** → Send to Log Analytics for analysis.

### ✅ Solution Architecture
```
Internet → Azure Firewall (hub VNet)
                └── UDR forces all egress through firewall
                └── Firewall Policy
                      ├── App Rules (FQDN filtering)
                      ├── Network Rules (IP/Port)
                      └── DNAT Rules (inbound NAT)
           → Spoke VNet-A → Subnet NSG → VMs
           → Spoke VNet-B → Subnet NSG → VMs
```

---

## 11. Azure Monitor & Log Analytics

### 🎯 Real-Time Use Case
> An operations team must be alerted within 5 minutes when any VM's CPU exceeds 85% or when a VM becomes unreachable. All logs must be centrally stored for 90 days for security auditing.

### 📌 Steps
1. **Create Log Analytics Workspace**.
2. **Connect VMs to workspace** → Install Azure Monitor Agent (AMA).
3. **Create Data Collection Rules (DCR)** → Collect Performance counters + Windows Event Logs + Syslog.
4. **Create Alert Rules** in Azure Monitor:
   - Signal: `Percentage CPU > 85%` → Severity 2 → Action Group (Email/SMS/Teams)
   - Signal: `Heartbeat == 0` → VM down alert
5. **Create Workbooks** → Visualize VM health dashboard.
6. **Set retention** → 90 days in workspace, archive to Storage Account.

---

## 12. Azure Key Vault

### 🎯 Real-Time Use Case
> A development team hardcoded database passwords in application config files (a security violation). All secrets, certificates, and encryption keys must be stored in a centralized vault, with apps retrieving secrets at runtime without knowing the actual values.

### 📌 Steps
1. **Create Azure Key Vault**.
2. **Store Secrets** → DB connection strings, API keys, passwords.
3. **Store Certificates** → SSL/TLS certificates with auto-renewal.
4. **Enable Managed Identity** on the App Service/VM.
5. **Grant Key Vault Access** → Key Vault Access Policy or RBAC to Managed Identity.
6. **Application retrieves secret at runtime** → No secrets in code or config files.
7. **Enable Soft Delete + Purge Protection** → 90-day recovery window.

---

## 13. Azure Policy & Governance

### 🎯 Real-Time Use Case
> A large enterprise has 50 subscriptions across 5 business units. Cloud governance is broken — resources are created in wrong regions, without tags, and without encryption. The Cloud Center of Excellence (CCoE) needs guardrails enforced automatically.

### 📌 Steps
1. **Create Management Group hierarchy** → Root → Business Units → Subscriptions.
2. **Assign Built-in Policies**:
   - `Allowed locations` → Only allow `Central India`, `South India`
   - `Require a tag and its value` → Enforce `CostCenter` tag
   - `Audit VMs that do not use managed disks`
3. **Create Custom Policy** → Deny creation of public IPs in production subscriptions.
4. **Create Policy Initiative (Blueprint)** → Bundle multiple policies + RBAC + ARM templates.
5. **Review Compliance Dashboard** → Identify non-compliant resources.
6. **Enable Remediation Tasks** → Auto-fix non-compliant resources.

---

## 14. Azure Kubernetes Service (AKS)

### 🎯 Real-Time Use Case
> A SaaS company wants to containerize a microservices application (frontend, API, database) and run it on managed Kubernetes. The team needs auto-scaling, rolling deployments, and zero-downtime updates.

### 📌 Steps
1. **Create Azure Container Registry (ACR)** → Push Docker images.
2. **Create AKS Cluster**:
   - Node pools: System (2 nodes) + User (auto-scale 2–10)
   - Enable Azure CNI networking
   - Attach ACR to AKS
3. **Deploy application** using `kubectl apply -f deployment.yaml`.
4. **Expose via Kubernetes Service** (LoadBalancer type → Azure LB provisioned automatically).
5. **Configure Horizontal Pod Autoscaler (HPA)** → Scale pods based on CPU.
6. **Configure Cluster Autoscaler** → Scale nodes based on pending pods.
7. **Enable Azure Monitor for AKS** → Container Insights.

---

## 15. Azure App Service & Web Apps

### 🎯 Real-Time Use Case
> A startup wants to deploy a Node.js web application without managing VMs or OS updates. They need staging environments, auto-scaling, custom domains with SSL, and zero-downtime deployments.

### 📌 Steps
1. **Create App Service Plan** → Choose tier (S1 for staging, P2v3 for production).
2. **Create Web App** → Choose runtime (Node.js, Python, .NET, Java, PHP).
3. **Configure Deployment Slots**:
   - `Production` slot → live traffic
   - `Staging` slot → test new version
4. **Deploy code** to Staging → Test thoroughly.
5. **Swap slots** → Staging becomes Production (zero-downtime, instant rollback possible).
6. **Configure Auto-Scale** → Scale out to 5 instances when requests > 1000/min.
7. **Add custom domain + SSL certificate** from Key Vault or App Service managed cert.

---

## 📚 Quick Reference — Azure Admin Cheat Sheet

| Task | Azure Service | Key Command / Portal Path |
|------|--------------|--------------------------|
| Create VM | Azure Compute | Portal → Virtual Machines → Create |
| Connect VNets | VNet Peering | VNet → Peerings → Add |
| Backup VM | Recovery Services Vault | RSV → Backup → Azure VM |
| Disaster Recovery | Azure Site Recovery | RSV → Site Recovery → Enable Replication |
| Load Balancing | Azure Load Balancer | LB → Backend Pools → Health Probes |
| Auto-scaling | VMSS | Scale Set → Scaling → Custom Autoscale |
| Store files | Storage Account | SA → Containers → Upload |
| Site-to-Site VPN | VPN Gateway | VNG → Connections → Add |
| Sync AD users | Azure AD Connect | AD Connect wizard on DC |
| Central firewall | Azure Firewall | Firewall → Policies → Rules |
| Monitor & Alert | Azure Monitor | Monitor → Alerts → Create |
| Secret management | Azure Key Vault | KV → Secrets → Generate |
| Enforce governance | Azure Policy | Policy → Assignments → Assign |
| Container workloads | AKS | Kubernetes Services → Create |
| Managed web hosting | App Service | App Services → Create |

---

*Generated for Azure Administrator training — June 2026*
