
# Smakonnect Tech Innovation | Azure Infrastructure Administration

**Hands-on Microsoft Azure portfolio | Systems Administration | Cloud Operations | MSP Support**

**Project status:** Active development · **8 of 12 phases completed (67%)** · **Next: Backup and Disaster Recovery**

## At a Glance

I designed and administer a connected Microsoft Azure lab for **Smakonnect Tech Innovation**, a fictional organization. The project demonstrates how I approach infrastructure work: plan the environment, deploy services, apply access and security controls, validate functionality, troubleshoot failures, and document the outcome.

The environment includes **Azure governance and RBAC, a four-subnet virtual network, Windows Server 2022, Ubuntu Server 24.04, an internal Nginx website, Azure Blob Storage, Azure Files, and centralized Azure monitoring**.

I have also implemented **Log Analytics, Azure Monitor Agent, Data Collection Rules, Windows Event Log and Linux Syslog collection, KQL queries, metric alerts, Activity Log monitoring, and email notifications**.

Each completed phase includes technical documentation and supporting screenshots.

> **Scope:** This is a self-managed technical lab, not a production deployment. Implemented controls and verified outcomes are distinguished from planned improvements.

**Explore the work:** [Implementation highlights](#selected-implementation-highlights) · [Troubleshooting examples](#troubleshooting-in-practice) · [Phase documentation](#project-phases-and-evidence) · [Architecture](#current-azure-environment)

## What I've Built and Validated

| Area | Practical work completed |
|---|---|
| **Governance** | Resource organization, standardized tags, Azure Policy audit, resource group Delete lock, and budget alerts |
| **Identity & access** | Azure RBAC scope and inheritance review; Blob data-plane role assignment; SAS authorization troubleshooting |
| **Networking** | `10.10.0.0/16` VNet, four subnets, subnet-associated NSG, source-restricted administration, private connectivity tests |
| **DNS & routing** | Azure-provided DNS, private VM hostname resolution, Windows hosts-file troubleshooting, Azure effective routes, Windows routing table inspection |
| **Network diagnostics** | Network Watcher IP flow verify, temporary NSG deny-rule simulation, connectivity recovery, public HTTP denial verification |
| **Windows & Linux** | Windows Server 2022 administration over RDP; Ubuntu 24.04 administration over SSH; firewall, service, and network diagnostics |
| **Internal application** | Nginx website hosted on Ubuntu and accessed from Windows over the private network |
| **Storage** | Private Blob container, read-only User Delegation SAS, Azure Files SMB share mapped to Windows, soft-delete recovery |
| **Storage security** | Selected-network firewall rules, approved server subnet via service endpoint, and restricted client IP access |
| **Azure monitoring** | Azure Monitor metrics, Log Analytics, KQL, Windows Event Logs, Linux Syslog, and Data Collection Rules |
| **Alerting** | CPU threshold monitoring, VM deallocation Activity Log alert, Action Group email notifications, and successful live alert tests |
| **Administrative auditing** | Subscription Activity Log diagnostic export, AzureActivity queries, and investigation of VM operations |
| **Operations** | VM allocation troubleshooting, connectivity diagnosis, recovery validation, Resource Health checks, and cost-conscious resource management |

## Selected Implementation Highlights

### 01 — Built an Interconnected Azure Environment

Created a structured resource group and network foundation, then deployed Windows and Ubuntu VMs into the server subnet.

Verified private connectivity between the machines and hosted an **internal Nginx website** on Ubuntu, reachable from Windows at `http://10.10.2.5`.

**Why it matters:** This tests how compute, network security, guest operating systems, and application services interact—not just whether individual resources can be deployed.

[Explore VM deployment and validation](phase-05-azure-virtual-machines/azure-virtual-machines-deployment.md)

### 02 — Resolved Azure Blob SAS Authorization Failure

Generated a short-lived, read-only User Delegation SAS for a private blob.

After addressing an expiry-setting error, the download returned `AuthorizationPermissionMismatch`.

I investigated the distinction between subscription-level **Owner** permissions and Blob **data-plane** access, assigned **Storage Blob Data Reader** at storage account scope, and successfully retested the SAS.

**Why it matters:** The resolution targeted the permission boundary rather than exposing the container publicly or granting unnecessary write access.

[Explore storage security and troubleshooting](phase-06-azure-storage/azure-storage-administration.md)

### 03 — Integrated Azure Files with Windows Server

Created `smakonnect-it-share` and mapped it to Windows Server as drive `Z:`.

Confirmed TCP connectivity on **port 445**, verified the mapped drive, and successfully wrote and read an IT operations test file.

I also enabled and tested **Blob soft delete**, recovering a deliberately deleted object.

**Why it matters:** This demonstrates service integration, command-line validation, and practical recovery testing.

[Explore Azure Files and data protection](phase-06-azure-storage/azure-storage-administration.md)

### 04 — Diagnosed and Recovered from an NSG-Blocked Connection

Simulated an internal application outage by creating a temporary NSG rule denying TCP port 80 from the Windows VM (`10.10.2.4`) to the Ubuntu Nginx server (`10.10.2.5`).

Confirmed the connection failed, used **Network Watcher IP flow verify** to identify the matching deny rule, removed the temporary rule, and verified that HTTP connectivity was restored.

Also inspected **Azure effective routes and Windows routing tables**, diagnosed an intentionally incorrect Windows hosts-file entry, verified public HTTP was denied by the NSG, and restricted an existing SSH allow rule to **TCP port 22**.

**Why it matters:** The exercise demonstrates a repeatable troubleshooting method: reproduce the symptom, isolate the control responsible, remediate, and retest—without confusing DNS, routing, and NSG failures.

[Explore Phase 7 networking and troubleshooting](phase-07-advanced-networking/azure-advanced-networking-troubleshooting.md)

### 05 — Implemented Centralized Windows and Linux Monitoring

Configured a Log Analytics workspace and Azure Monitor Agent to collect selected operating system events from Windows Server 2022 and Ubuntu Server 24.04.

Created separate Data Collection Rules for Windows Event Logs and Linux Syslog.

Generated a Windows Print Spooler service event and verified **Event ID 7036** in Log Analytics.

Generated a Linux Syslog Warning event using `logger` and verified its collection using KQL.

**Why it matters:** Centralized logging provides administrators with a practical way to investigate operating system events without relying entirely on individual server consoles.

[Explore Azure Monitoring and Diagnostics](phase-08-monitoring-and-diagnostics/phase-08-azure-monitoring-and-diagnostics.md)

### 06 — Configured and Validated Azure Monitor Alerts

Created a CPU metric alert for the Ubuntu VM using a threshold of **greater than 20% average CPU utilization**.

Generated controlled CPU activity using `stress-ng`, observed the threshold being exceeded, and received an actual Azure Monitor email notification.

Also configured an Activity Log alert for successful Ubuntu VM deallocation.

Stopped and deallocated the VM, received the alert email, and verified the administrative operation in Log Analytics.

**Why it matters:** These tests demonstrate the full monitoring workflow—from detecting a condition to notifying an administrator and validating the corresponding evidence.

[Explore monitoring alerts and validation](phase-08-monitoring-and-diagnostics/phase-08-azure-monitoring-and-diagnostics.md)

## Troubleshooting in Practice

| Issue | Investigation and outcome |
|---|---|
| **Ubuntu could not ping Windows** | Checked routing and TCP connectivity, then isolated the issue to Windows Firewall blocking inbound ICMP. A temporary source-restricted rule confirmed the diagnosis. |
| **Blob SAS returned `AuthorizationPermissionMismatch`** | Reviewed management-plane versus data-plane permissions; assigned an appropriate Blob data role and verified a new read-only SAS. |
| **Windows VM failed to start with `ZonalAllocationFailed`** | Identified an Azure compute capacity issue and resized the VM to an available compatible size, restoring access without rebuilding the OS. |
| **Internal Linux hostname resolved to the wrong IP** | Introduced a temporary incorrect Windows hosts-file mapping, compared `ping` with `nslookup`, removed the mapping, flushed the DNS cache, and verified TCP port 80 connectivity. |
| **Windows-to-Ubuntu HTTP blocked by an NSG** | Applied a temporary targeted deny rule, confirmed the connection failure, identified the rule with Network Watcher IP flow verify, removed it, and retested successfully. |
| **Public HTTP access denied** | Used Network Watcher to confirm that inbound internet-originated TCP port 80 traffic matched the default `DenyAllInBound` rule. |
| **Ubuntu VM start failed with `AllocationFailed`** | Inspected Azure Activity Log details and identified insufficient compute capacity for the requested VM size. The VM was subsequently observed running; the precise recovery action was not established. |
| **AzureActivity query initially returned no records** | Identified the missing subscription diagnostic export, configured Administrative Activity Log collection, and verified records in Log Analytics. |
| **Windows and Linux monitoring validation** | Generated controlled Windows service and Linux Syslog events and confirmed successful ingestion through KQL. |
| **CPU and VM deallocation alerts** | Generated test conditions, received email notifications, and verified supporting monitoring data. |

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
| VNet DNS | Azure-provided DNS (default) |
| Routing | Azure system routes; no user-defined route table attached to the tested Windows VM NIC |
| Storage account | `stsmakonnectlab01` — Standard StorageV2, LRS |
| Blob container | `smakonnect-documents` — private |
| Azure file share | `smakonnect-it-share` — SMB |
| Log Analytics | `law-smakonnect-monitor-01` |
| Windows DCR | `dcr-smakonnect-windows-events-01` |
| Linux DCR | `dcr-smakonnect-linux-syslog-01` |
| Monitoring Action Group | `ag-smakonnect-monitor-email-01` |
| CPU alert | `alert-smakonnect-linux-high-cpu-01` |
| Activity Log alert | `alert-smakonnect-linux-deallocate-01` |
| Activity Log diagnostic setting | `diag-smakonnect-activity-logs-01` |

### Subnet Design

| Subnet | Address range | Purpose |
|---|---|---|
| `snet-management` | `10.10.1.0/24` | Management workloads |
| `snet-servers` | `10.10.2.0/24` | Windows and Linux servers |
| `snet-applications` | `10.10.3.0/24` | Application workloads |
| `snet-private-endpoints` | `10.10.4.0/24` | Reserved for future private endpoint deployment |

The storage account currently uses **selected-network public endpoint access**, including an approved client IP and server subnet service endpoint.

A **private endpoint has not been deployed**.

## Project Phases and Evidence

| Phase | Focus | Status | Documentation |
|---|---|---|---|
| 01 | Environment assessment and architecture | Completed | [View](phase-01-environment-assessment) |
| 02 | Governance and resource organization | Completed | [View](phase-02-azure-governance) |
| 03 | RBAC and access management | Completed | [View](phase-03-azure-rbac/azure-rbac-access-management.md) |
| 04 | Virtual networking and NSGs | Completed | [View](phase-04-azure-virtual-networking/azure-virtual-networking-configuration.md) |
| 05 | Windows and Linux VM administration | Completed | [View](phase-05-azure-virtual-machines/azure-virtual-machines-deployment.md) |
| 06 | Azure Storage and data protection | Completed | [View](phase-06-azure-storage/azure-storage-administration.md) |
| 07 | Advanced networking and connectivity troubleshooting | **Completed** | [View](phase-07-advanced-networking/azure-advanced-networking-troubleshooting.md) |
| 08 | Azure Monitoring and Diagnostics | **Completed** | [View](phase-08-monitoring-and-diagnostics/phase-08-azure-monitoring-and-diagnostics.md) |
| 09 | Backup and disaster recovery | **Next** | — |
| 10 | PowerShell, Azure CLI, and Bicep | Planned | — |
| 11 | MSP troubleshooting scenarios | Planned | — |
| 12 | Final infrastructure review and interview preparation | Planned | — |

**Progress: 8/12 phases (67%).**

Screenshots, configuration evidence, troubleshooting records, and validation results are organized within the completed phase folders.

## Security, Constraints, and Next Improvements

The lab uses **private Blob access, Azure RBAC, NSGs, restricted administrative access, SSH public-key authentication, storage network rules, and soft-delete protection**.

I also use tagging, budget alerts, and VM deallocation to manage limited Azure credits.

For monitoring, I configured:
- Log Analytics with 30-day retention
- A daily ingestion cap of 0.023 GB
- Filtered Windows Event Log collection
- Selected Linux Syslog facilities and severity levels
- Reusable email notifications through Azure Monitor Action Groups
- Administrative Activity Log export for auditing

Temporary alert rules and diagnostic settings are subject to ongoing cost review.

The school-managed Microsoft Entra tenant limits tenant-level identity administration. I documented this boundary rather than claiming administrative access I do not have.

Azure Files uses storage account key authentication in this lab; identity-based SMB authentication is a potential future improvement.

### Next Areas of Work

**Phase 09 — Backup and Disaster Recovery**

Planned activities include:
- Azure Backup concepts and recovery services
- Recovery Services vault configuration
- VM backup policy evaluation
- Backup and restore validation
- Recovery objectives and disaster recovery planning
- Cost and security considerations

Future phases will address infrastructure automation with **PowerShell, Azure CLI, and Bicep**, realistic MSP troubleshooting scenarios, and a final infrastructure review.

Additional hardening options—including private endpoints, more restrictive administrative access, and keyless storage authentication—remain future considerations, not completed implementations.

## Why This Portfolio Exists

I maintain this repository as a practical record of **what I configured, why I chose it, how I verified it, and how I handled problems**.

My aim is to demonstrate transferable skills for **Azure Administrator, Systems Administrator, Cloud Support, and MSP operations** roles while continuing to develop my technical judgment.

This project reflects hands-on infrastructure administration, troubleshooting, operational monitoring, security awareness, and technical documentation.

---

**Smakonnect Tech Innovation — Azure Infrastructure Administration Portfolio**

**Current progress: 8 of 12 phases completed (67%)**
