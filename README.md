# Smakonnect Tech Innovation | Azure Infrastructure Administration

**Hands-on Microsoft Azure portfolio | AZ-104 | Systems Administration | Cloud Operations | MSP Support**

**Project status:** Active development · **6 of 12 phases completed (50%)** · **Next: Advanced Azure Networking**

## At a Glance

I designed and administer a connected Microsoft Azure lab for **Smakonnect Tech Innovation**, a fictional organization. The project demonstrates how I approach infrastructure work: plan the environment, deploy services, apply access and security controls, validate functionality, troubleshoot failures, and document the outcome.

The environment currently includes **Azure governance and RBAC, a four-subnet virtual network, Windows Server 2022, Ubuntu Server 24.04, an internal Nginx website, Azure Blob Storage, and Azure Files**. Each completed phase includes technical documentation and screenshots.

> **Scope:** This is a self-managed technical lab, not a production deployment. Implemented controls and verified outcomes are distinguished from planned improvements.

**Explore the work:** [Implementation highlights](#selected-implementation-highlights) · [Troubleshooting examples](#troubleshooting-in-practice) · [Phase documentation](#project-phases-and-evidence) · [Architecture](#current-azure-environment)

## What I've Built and Validated

| Area | Practical work completed |
|---|---|
| **Governance** | Resource organization, standardized tags, Azure Policy audit, resource group Delete lock, and budget alerts |
| **Identity & access** | Azure RBAC scope and inheritance review; Blob data-plane role assignment; SAS authorization troubleshooting |
| **Networking** | `10.10.0.0/16` VNet, four subnets, subnet-associated NSG, source-restricted administration, private connectivity tests |
| **Windows & Linux** | Windows Server 2022 administration over RDP; Ubuntu 24.04 administration over SSH; firewall, service, and network diagnostics |
| **Internal application** | Nginx website hosted on Ubuntu and accessed from Windows over the private network |
| **Storage** | Private Blob container, read-only User Delegation SAS, Azure Files SMB share mapped to Windows, soft-delete recovery |
| **Storage security** | Selected-network firewall rules, approved server subnet via service endpoint, and restricted client IP access |
| **Operations** | VM allocation troubleshooting, connectivity diagnosis, recovery validation, and cost-conscious resource management |

## Selected Implementation Highlights

### 01 — Built an Interconnected Azure Environment

Created a structured resource group and network foundation, then deployed Windows and Ubuntu VMs into the server subnet. Verified private connectivity between the machines and hosted an **internal Nginx website** on Ubuntu, reachable from Windows at `http://10.10.2.5`.

**Why it matters:** This tests how compute, network security, guest operating systems, and application services interact—not just whether individual resources can be deployed.

[Explore VM deployment and validation](phase-05-azure-virtual-machines/azure-virtual-machines-deployment.md)

### 02 — Resolved Azure Blob SAS Authorization Failure

Generated a short-lived, read-only User Delegation SAS for a private blob. After addressing an expiry-setting error, the download returned `AuthorizationPermissionMismatch`. I investigated the distinction between subscription-level **Owner** permissions and Blob **data-plane** access, assigned **Storage Blob Data Reader** at storage account scope, and successfully retested the SAS.

**Why it matters:** The resolution targeted the permission boundary rather than exposing the container publicly or granting unnecessary write access.

[Explore storage security and troubleshooting](phase-06-azure-storage/azure-storage-administration.md)

### 03 — Integrated Azure Files with Windows Server

Created `smakonnect-it-share` and mapped it to Windows Server as drive `Z:`. Confirmed TCP connectivity on **port 445**, verified the mapped drive, and successfully wrote and read an IT operations test file. I also enabled and tested **Blob soft delete**, recovering a deliberately deleted object.

**Why it matters:** This demonstrates service integration, command-line validation, and practical recovery testing.

[Explore Azure Files and data protection](phase-06-azure-storage/azure-storage-administration.md)

## Troubleshooting in Practice

| Issue | Investigation and outcome |
|---|---|
| **Ubuntu could not ping Windows** | Checked routing and TCP connectivity, then isolated the issue to Windows Firewall blocking inbound ICMP. A temporary source-restricted rule confirmed the diagnosis. |
| **Blob SAS returned `AuthorizationPermissionMismatch`** | Reviewed management-plane versus data-plane permissions; assigned an appropriate Blob data role and verified a new read-only SAS. |
| **Windows VM failed to start with `ZonalAllocationFailed`** | Identified an Azure compute capacity issue and resized the VM to an available compatible size, restoring access without rebuilding the OS. |

I document the **symptom, diagnostic approach, root cause or finding, corrective action, and validation** wherever the lab provides a meaningful troubleshooting scenario.

## Current Azure Environment

| Component | Configuration |
|---|---|
| Subscription | Visual Studio Professional Azure subscription |
| Region | Canada Central |
| Resource group | `rg-smakonnect-lab-01` |
| Virtual network | `vnet-smakonnect-lab-01` — `10.10.0.0/16` |
| Network security group | `nsg-smakonnect-servers-01`, associated with the Servers subnet |
| Windows VM | `vm-smakonnect-win-01` — Windows Server 2022 — `10.10.2.4` |
| Linux VM | `vm-smakonnect-linux-01` — Ubuntu Server 24.04 — `10.10.2.5` |
| Storage account | `stsmakonnectlab01` — Standard StorageV2, LRS |
| Blob container | `smakonnect-documents` — private |
| Azure file share | `smakonnect-it-share` — SMB |

**Subnet design**

| Subnet | Address range | Purpose |
|---|---|---|
| `snet-management` | `10.10.1.0/24` | Management workloads |
| `snet-servers` | `10.10.2.0/24` | Windows and Linux servers |
| `snet-applications` | `10.10.3.0/24` | Application workloads |
| `snet-private-endpoints` | `10.10.4.0/24` | Reserved for future private endpoint deployment |

The storage account currently uses **selected-network public endpoint access**, including an approved client IP and server subnet service endpoint. A **private endpoint has not been deployed**.

## Project Phases and Evidence

| Phase | Focus | Status | Documentation |
|---|---|---|---|
| 01 | Environment assessment and architecture | Completed | [View](phase-01-environment-assessment) |
| 02 | Governance and resource organization | Completed | [View](phase-02-azure-governance) |
| 03 | RBAC and access management | Completed | [View](phase-03-azure-rbac/azure-rbac-access-management.md) |
| 04 | Virtual networking and NSGs | Completed | [View](phase-04-azure-virtual-networking/azure-virtual-networking-configuration.md) |
| 05 | Windows and Linux VM administration | Completed | [View](phase-05-azure-virtual-machines/azure-virtual-machines-deployment.md) |
| 06 | Azure Storage and data protection | **Completed** | [View](phase-06-azure-storage/azure-storage-administration.md) |
| 07 | Advanced networking and connectivity | Next | — |
| 08 | Monitoring and diagnostics | Planned | — |
| 09 | Backup and disaster recovery | Planned | — |
| 10 | PowerShell, Azure CLI, and Bicep | Planned | — |
| 11 | MSP troubleshooting scenarios | Planned | — |
| 12 | Final infrastructure review and interview preparation | Planned | — |

**Progress: 6/12 phases (50%).** Screenshots and configuration evidence are organized within the completed phase folders.

## Security, Constraints, and Next Improvements

The lab uses **private Blob access, Azure RBAC, NSGs, restricted administrative access, SSH public-key authentication, storage network rules, and soft-delete protection**. I also use tagging, budget alerts, and VM deallocation to manage limited Azure credits.

The school-managed Microsoft Entra tenant limits tenant-level identity administration. I documented this boundary rather than claiming administrative access I do not have. Azure Files uses storage account key authentication in this lab; identity-based SMB authentication is a potential future improvement.

**Next areas of work:** Advanced network diagnostics and connectivity, monitoring and alerting, backup and recovery, automation with PowerShell/Azure CLI/Bicep, and realistic MSP support scenarios. Additional hardening options—including private endpoints, more restrictive administrative access, and keyless storage authentication—remain future considerations, not completed implementations.

## Why This Portfolio Exists

I maintain this repository as a practical record of **what I configured, why I chose it, how I verified it, and how I handled problems**. My aim is to demonstrate transferable skills for **Azure Administrator, Systems Administrator, Cloud Support, and MSP operations** roles, while continuing to develop my technical judgment and AZ-104 knowledge.
