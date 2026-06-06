# AZURE_Cloud_Administrator_Syllabus

---

## Module 1: Azure Cloud Computing Basics

**Learning Objective:** In this Module you will learn about Azure Cloud Architecture, Subscription, Support Plan, Resource Group, and Tags

- Overview of Azure Cloud
- Azure Free Tier Account
- Azure Cloud Architecture
- Azure Subscription
- Need of Azure Support Plan and Type of Support Plan
- Importance of Azure Resource Group
- Creating, Managing and Deleting Azure Resource Group
- Adding Azure Tags to Azure Resources

---

## Module 2: Identity and Access Management (IAM), Role Based Access Control (RBAC) and Entra ID (Previously Azure AD)

**Learning Objective:** In this Module you will learn about Identity and Access Management (IAM), Azure Role Based Access Control (RBAC), Difference between RBAC Role and Entra ID Role, how to add custom domain to Entra ID, how to give access of your azure account to other users and how to setup MFA

- What is Microsoft Entra ID
- User Creation in Microsoft Entra ID
- Giving Microsoft Entra ID Roles to user
- Adding Custom Domain in Microsoft Entra ID
- How to buy Custom Domain from Domain Registrar
- Identity and Access Management (IAM) Overview
- Need of Azure Role Based Access Control (RBAC)
- Understanding Roles in RBAC
- Assigning RBAC Roles to user
- How to recover deleted user from Microsoft Entra ID
- Difference between Microsoft Entra ID Role and RBAC Role
- Type of Scope where you can Assign RBAC Roles
- Enable Multi Factor Authentication (MFA) for user

---

## Module 3: Azure Virtual Machine (VM), Azure Networking, Availability Zone and Availability Set

**Learning Objective:** In this Module you will learn about creating a Highly Available Design using concept of Availability Set and Availability Zone and Deploy Virtual Machine in Availability Zone and Availability Set

- Understand Azure Region, Location and Geography
- Need of Availability Set in Azure
- Create Availability Set in Azure and Deploy Virtual Machine in Availability Set
- Understand Fault Domain and Update Domain concept in Availability Set
- Need of Azure Proximity Placement
- Create Proximity Placement and attach to Availability Set
- Importance of Availability Zone in Azure
- Deploy Virtual Machine in Availability Zone
- Difference Between Availability Zone and Availability Set
- Understand Virtual Network (VNET) and Subnet
- Create Virtual Network (VNET) and Subnet
- Deploy Virtual Machine in VNET and Subnet
- Change Azure Virtual Machine Size and Family Type
- Understand Concept of STOPPED versus DEALLOCATE in Azure Virtual Machine
- Boot Diagnostic in Azure Virtual Machine
- Configuring Auto-shutdown to save cost of Azure Virtual Machine
- Reset User Password of Azure Virtual Machine

---

## Module 4: Azure NSG (Firewall), NIC (LAN Card) and Public IP

**Learning Objective:** In this Module you will be introduced to Azure Virtual Machine Components like NSG (Firewall), NIC (LAN Card), and Public IP Address

- Azure Virtual Machine Network Interface (LAN Card)
- Setting Custom DNS in Azure Virtual Machine
- Azure Virtual Machine Public IP
- Dissociating Azure Virtual Machine Public IP
- Understanding Network Security Group (NSG)
- Attaching NSG to Azure Virtual Machine
- Attaching NSG to Subnet
- Understanding NSG Default Rule
- Creating Custom Rule in NSG
- Understanding Concept of NSG Inbound and Outbound Rule
- Create Rule using Priority Number
- Create Allow and Deny NSG Rule

---

## Module 5: VNET Peering and NAT Gateway

**Learning Objective:** In this Module you will learn to Create Peering between different Virtual Network (VNET) and Implement NAT Gateway

- How one Virtual Machine Communicates with other Virtual Machine in Azure
- Need of VNET Peering
- Performing VNET Peering
- Local Peering and Global Peering
- Need of NAT Gateway
- Implement NAT Gateway
- Attach NAT Gateway to Subnet

---

## Module 6: Azure Managed Disk, Snapshot and Locks

**Learning Objective:** In this Module you will learn about Azure Disk and Disk Type and how to create snapshot and protect your resources using locks

- What is Azure Managed Disk
- Disk Type Premium/Standard SSD & Standard HDD
- Concept of IOPS and Throughput in Disk
- Understand Type of Disk — Operating System Disk (OS), Temporary Disk & Data Disk
- Increase Size of OS and Data Disk
- Create and Attach Data Disk to Azure Virtual Machine
- What is Azure Snapshot
- Azure Snapshot Type
- Create Azure Snapshot from Disk
- Need of Azure Locks and their Types
- Apply different Type of Lock to Resource Group

---

## Module 7: Azure Cost Calculation and Cost Saving Techniques

**Learning Objective:** In this Module you will learn about Azure Virtual Machine, Disk, and Public IP Cost Calculation using Azure Pricing Calculator

- How to Use Azure Cloud in a cost saving way
- How Virtual Machine Pricing is calculated
- How to save cost in Prod and Non-Prod Virtual Machine
- Reservation of Virtual Machine in Azure
- Concept of Azure Hybrid Benefits in saving cost
- Using Spot Virtual Machine for Non Prod Virtual Machine
- Understanding Pricing of Disk and Public IP
- Configure Azure Budget
- Understand Azure Cost Management
- Use Azure Pricing Calculator for cost calculation

---

## Module 8: Azure Virtual Machine Backup & Restore and Azure Site Recovery (Disaster Recovery)

**Learning Objective:** In this Module you will learn how to create Backup of Azure Virtual Machine and Restore using Azure Backup and how to setup a DR (Disaster Recovery) Plan using Azure Site Recovery

- Create Recovery Service Vault for Virtual Machine Backup
- How Azure Backup Works
- Understanding Backup Redundancy options like LRS and GRS
- Take Backup of Azure Virtual Machine
- Understand Concept of Azure Backup Instant Restore
- Restore Virtual Machine from Backup
- Restore File from Backup
- Soft Delete in Azure Backup
- How Azure Site Recovery Works
- Create a Disaster Recovery Plan
- Select the Primary & Secondary Region
- Initiate Replication from Primary to Secondary Region
- Perform Failover in Azure Site Recovery

---

## Module 9: Azure Load Balancer, Traffic Manager and DNS

**Learning Objective:** In this Module you will learn about Azure Load Balancer, Traffic Manager and DNS

- How Load Balancer Works
- Public and Private Load Balancer
- Installing IIS Role in Virtual Machine
- Create Backend Pool, Health Probe and Load Balancing Rule in Load Balancer
- Create NAT Rule in Load Balancer
- Understand Working of Azure Traffic Manager
- Use Traffic Manager for DR Scenario
- Perform Traffic Management on Internet using Traffic Manager

---

## Module 10: Azure Virtual Machine Scale Set and Automate Deployment using JSON Template

**Learning Objective:** In this Module you will learn how to keep your design highly available using Azure Virtual Machine Scale Set and how to use scale set to increase or decrease number of Virtual Machines and you will be introduced to Infrastructure as a Code using JSON Template which is useful for repeated deployment

- How Azure Virtual Machine Scale Set Works
- Create Virtual Machine Scale Set
- Create a Scale in and Scale out policy
- See in action VM Scale in and Scale out operation
- Automate Deployment using JSON Template
- Create Virtual Machine and Storage using JSON Template

---

## Module 11: Azure Storage Account, Azure File Share and AZCOPY Tool

**Learning Objective:** In this Module you will learn how to create Azure Storage Account, How to access Storage Account, how to put data in Azure Storage Account, how to copy data between storage accounts using AZCOPY tool, and further we will have a look on File Share

- What is Azure Storage Account
- Premium and Standard Storage Options
- Hot/Cool and Archive Tier in Storage Account
- Type of Replication in Storage Account — LRS/GRS/RA-GRS/ZRS/GZRS/RA-GZRS
- Azure Storage Endpoints
- Access keys and Shared Access Signature of Azure Storage Account
- Access Azure Storage Account using Storage Explorer
- Create Azure File Share
- Azure Storage Account Life Cycle
- Copy Data between storage accounts using AZCOPY Tool

---

## Module 12: Azure VPN/Express Route, Entra ID Connect (Previously Azure AD Connect)

**Learning Objective:** In this Module you will learn how to connect your office with Azure Cloud using Azure VPN Gateway and then you will learn about connecting on-premises Domain Controller with Microsoft Entra ID using Entra ID Connect, so that the user has a single sign on experience

- How to connect On-Premises Office with Azure Cloud using VPN
- Create Azure VPN Gateway
- Create Fortigate Firewall
- Create Site to Site VPN Tunnel
- Understand Azure Express Route
- Difference between VPN and Express Route
- Understand Entra ID Connect
- Implement Single-Sign-On using Entra ID Connect
- Use Entra ID Connect Password Hash Authentication
- Perform Synchronization between On-Premises Domain Controller and Entra ID

---

## Module 13: Azure Bastion, Azure Monitor, Azure Policy and PowerShell

**Learning Objective:** In this Module you will learn how to secure your VM by not exposing RDP ports to the outside world, you will learn about Azure Monitor service which is used to send Alerts, you will create Virtual Machine using PowerShell and you will learn about Azure Policy for compliance

- Secure Virtual Machine Access using Azure Bastion
- Create Azure Bastion
- Need of Azure Monitor
- Create an Alert for Virtual Machine Restart
- Perform Monitoring of Azure Virtual Machine
- Create Azure Policy for maintaining compliance
- Create Virtual Machine and Storage using PowerShell
- Transfer Snapshot from one Region to another Region using PowerShell

---

## Module 14: Migration to Azure Clouds

**Learning Objective:** In this Module you will get an overview of Azure Cloud Migration

- Overview of Migration process to Azure Cloud
- Create Azure Migrate Service
- Concept of Discovery and Replication Server in Migration
- Understanding Discovery, Assess, Replication and Migration
