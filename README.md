# Smakonnect Tech Innovation | Azure Infrastructure Administration

### Microsoft Azure Infrastructure Portfolio | AZ-104 | Systems Administration | Cloud Operations

## Project Overview

This repository documents the design, deployment, administration, and ongoing improvement of a Microsoft Azure infrastructure environment built around a fictional organization, **Smakonnect Tech Innovation**.

I developed this project to maintain a practical record of my work with Azure infrastructure and to demonstrate the technical skills involved in cloud and systems administration.

The environment brings together several areas of infrastructure management, including resource governance, role-based access control, virtual networking, Windows and Linux administration, storage, monitoring, security, and troubleshooting.

Rather than treating each Azure service as a separate exercise, I am building an interconnected environment where the resources support realistic administrative and operational scenarios.

My focus is on the decisions behind each configuration, how services work together, and how I investigate and resolve issues when something does not behave as expected.

The repository also serves as an ongoing technical reference as I continue developing my Azure administration skills and preparing for the Microsoft AZ-104 certification.

> **Environment:** This is a self-managed technical lab based on a simulated business environment. It is not presented as a production deployment.

---

## Technical Focus

The project covers infrastructure responsibilities commonly associated with Azure Administrator, Systems Administrator, Cloud Support, and Managed Service Provider (MSP) roles.

| Area | Technologies and Responsibilities |
|---|---|
| Azure Administration | Resource groups, subscriptions, resource deployment, configuration management |
| Governance | Resource tagging, Azure Policy, resource locks, cost management |
| Identity and Access | Azure RBAC, role assignments, permission scopes, access troubleshooting |
| Networking | Virtual Networks, subnet design, NSGs, private IP addressing, connectivity testing |
| Windows Administration | Windows Server 2022, RDP, services, Event Viewer, Windows Firewall |
| Linux Administration | Ubuntu Server 24.04, SSH, systemd, logs, network diagnostics |
| Application Infrastructure | Nginx, internal HTTP hosting, service connectivity |
| Storage | Azure Storage Accounts, Blob Storage, Azure Files, access management |
| Monitoring | Azure Monitor, Activity Logs, alerts, Log Analytics |
| Automation | PowerShell, Azure CLI, Bicep |
| Operations | Troubleshooting, security reviews, documentation, cost optimization |

Some technologies listed above are part of upcoming project phases and have not yet been implemented.

---

## Infrastructure Scenario

Smakonnect Tech Innovation is a simulated organization requiring a structured Azure environment to support internal servers, applications, and shared data.

For this project, I am responsible for planning and administering the environment, establishing appropriate access controls, deploying infrastructure resources, validating connectivity, and documenting operational decisions.

The environment is designed to support several practical objectives:

- Maintain an organized and manageable Azure resource structure.
- Separate infrastructure workloads through planned subnet allocation.
- Apply governance and network security controls.
- Support both Windows Server and Linux workloads.
- Enable private communication between Azure resources.
- Provide infrastructure for internal application hosting.
- Introduce storage, monitoring, and recovery capabilities.
- Investigate operational issues using appropriate diagnostic tools.
- Manage resource costs within a limited lab budget.

The project is delivered in phases, allowing each completed component to become part of the wider infrastructure.

---

## Azure Environment

| Configuration | Details |
|---|---|
| Cloud platform | Microsoft Azure |
| Subscription | Visual Studio Professional Azure subscription |
| Primary region | Canada Central |
| Resource group | `rg-smakonnect-lab-01` |
| Virtual network | `vnet-smakonnect-lab-01` |
| VNet address space | `10.10.0.0/16` |
| Server subnet | `snet-servers` — `10.10.2.0/24` |
| Server network security group | `nsg-smakonnect-servers-01` |
| Windows VM | `vm-smakonnect-win-01` |
| Linux VM | `vm-smakonnect-linux-01` |

### Governance and Resource Management

The environment includes a consistent resource organization and governance approach.

I configured resource tags to identify the project, environment, department, and management ownership.

I also established a monthly Azure budget with alerts at 50%, 80%, and 100% of the configured threshold.

Additional governance work includes reviewing Azure Policy assignments, configuring a resource group Delete lock, and investigating Azure RBAC permissions.

The subscription is associated with a school-managed Microsoft Entra tenant. While I have the Owner role at subscription scope, tenant-level identity administration is restricted.

I documented this limitation during the access management phase and kept the implementation within the permissions available to my account.

---

## Network Architecture

The virtual network was designed with separate address ranges for management, server, application, and private endpoint workloads.

| Subnet | Address Range | Intended Purpose |
|---|---|---|
| `snet-management` | `10.10.1.0/24` | Administrative and management workloads |
| `snet-servers` | `10.10.2.0/24` | Windows and Linux servers |
| `snet-applications` | `10.10.3.0/24` | Application workloads |
| `snet-private-endpoints` | `10.10.4.0/24` | Private connectivity to Azure services |

This layout provides a foundation for organizing workloads and applying more specific network controls as the project expands.

The Servers subnet is associated with `nsg-smakonnect-servers-01`.

The NSG contains a custom rule allowing RDP from the Management subnet, along with temporary source-restricted administrative access rules used during the VM deployment and testing phase.

I also reviewed Azure's default NSG rules to understand how traffic is evaluated and why an Allow rule does not automatically isolate a workload.

### Current Server Architecture

Two virtual machines are deployed in the Servers subnet.

| Server | Operating System | Private IP | Primary Function |
|---|---|---|---|
| `vm-smakonnect-win-01` | Windows Server 2022 | `10.10.2.4` | Windows administration and internal connectivity testing |
| `vm-smakonnect-linux-01` | Ubuntu Server 24.04 LTS | `10.10.2.5` | Linux administration and Nginx web hosting |

Both servers communicate over the Azure private network.

The Ubuntu VM hosts a Smakonnect internal website using Nginx. I verified that the Windows VM can access the application through Ubuntu's private IP address on TCP port 80.

I also tested access from outside the virtual network and confirmed that the public HTTP connection timed out, consistent with the configured NSG restrictions.

This deployment provides a working foundation for the storage, networking, monitoring, and troubleshooting phases that follow.

---

## Project Roadmap

The project is organized into 12 phases covering Azure infrastructure deployment, administration, security, and operations.

**Current progress: 5 of 12 phases completed (42%).**

| Phase | Scope | Status |
|---|---|---|
| 01 | Azure Subscription Assessment and Architecture Planning | Completed |
| 02 | Resource Groups, Tags, and Azure Governance | Completed |
| 03 | Azure RBAC and Access Management | Completed |
| 04 | Virtual Networks, Subnets, and Network Security | Completed |
| 05 | Windows Server and Linux Virtual Machines | Completed |
| 06 | Azure Storage and Data Protection | In Progress |
| 07 | Advanced Networking and Hybrid Connectivity | Planned |
| 08 | Azure Monitoring and Troubleshooting | Planned |
| 09 | Azure Backup and Disaster Recovery | Planned |
| 10 | PowerShell, Azure CLI, and Bicep | Planned |
| 11 | IT Support and MSP Troubleshooting Scenarios | Planned |
| 12 | Final Project Review and AZ-104 Interview Preparation | Planned |

---

## Phase Documentation

Each completed phase includes technical documentation covering the implementation, configuration decisions, validation, troubleshooting, and supporting evidence.

| Phase | Documentation | Status |
|---|---|---|
| 01 — Environment Assessment and Architecture Planning | [View Documentation](phase-01-environment-assessment) | Completed |
| 02 — Azure Governance and Resource Organization | [View Documentation](phase-02-azure-governance) | Completed |
| 03 — Azure RBAC and Access Management | [View Documentation](phase-03-azure-rbac/azure-rbac-access-management.md) | Completed |
| 04 — Azure Virtual Networking and Connectivity | [View Documentation](phase-04-azure-virtual-networking/azure-virtual-networking-configuration.md) | Completed |
| 05 — Windows Server and Linux Virtual Machines | [View Documentation](phase-05-azure-virtual-machines/azure-virtual-machines-deployment.md) | Completed |
| 06 — Azure Storage and Data Protection | Documentation in progress | In Progress |
| 07 — Advanced Networking and Hybrid Connectivity | Planned | Not Started |
| 08 — Azure Monitoring and Troubleshooting | Planned | Not Started |
| 09 — Azure Backup and Disaster Recovery | Planned | Not Started |
| 10 — PowerShell, Azure CLI, and Bicep | Planned | Not Started |
| 11 — IT Support and MSP Troubleshooting Scenarios | Planned | Not Started |
| 12 — Final Project Review and AZ-104 Interview Preparation | Planned | Not Started |

---

## Implementation Highlights

### Phase 01 — Azure Environment Assessment and Architecture Planning

I began by assessing the available Azure environment and establishing the requirements for the infrastructure.

The work included:

- Reviewing the subscription, available credits, and regional resource quotas.
- Verifying subscription-level administrative permissions.
- Reviewing existing Azure Policy assignments.
- Planning the resource organization and naming conventions.
- Designing the virtual network address space and subnet structure.
- Evaluating VM deployment options and their cost implications.
- Establishing an implementation roadmap covering governance, networking, compute, storage, monitoring, and automation.

This phase provided the technical foundation for the deployments that followed.

### Phase 02 — Azure Governance and Resource Organization

I configured the core resource management controls for the Smakonnect environment.

Key activities included:

- Creating `rg-smakonnect-lab-01` in Canada Central.
- Applying standardized tags for resource identification and organization.
- Configuring a monthly Azure budget with alerts at 50%, 80%, and 100%.
- Applying a resource group Delete lock as protection against accidental deletion.
- Assigning an Azure Policy to audit the presence of the `Environment` tag.
- Reviewing the resulting governance configuration in the Azure Portal.

The Policy assignment was configured without enforcement, allowing me to examine policy behavior without introducing deployment restrictions during the lab.

This phase established a more structured approach to managing Azure resources, controlling costs, and reducing administrative risk.

### Phase 03 — Azure RBAC and Access Management

This phase focused on subscription and resource-level authorization.

I investigated how Azure RBAC uses role definitions, security principals, and assignment scopes to control administrative access.

My work included:

- Verifying the Owner role assigned at subscription scope.
- Comparing the permissions associated with Owner, Contributor, and Reader.
- Reviewing role assignment inheritance and resource group scope.
- Exploring the role assignment workflow without creating unnecessary assignments.
- Examining the relationship between active and eligible role assignments.
- Reviewing Activity Logs and access-related information.
- Working through a troubleshooting scenario involving a user who could view resources but could not modify them.

I also investigated the distinction between Azure RBAC permissions and Microsoft Entra tenant administration.

The school-managed tenant restricted certain identity administration operations, so I documented the limitation and continued with the Azure RBAC scope available to me.

**Technical takeaway:** Subscription ownership does not automatically provide tenant-level administrative privileges. Effective access troubleshooting requires identifying the relevant scope, role assignment, and service boundary.

[Read Phase 03 Documentation](phase-03-azure-rbac/azure-rbac-access-management.md)

### Phase 04 — Azure Virtual Networking and Security

I implemented the network architecture planned during the initial assessment.

The deployment included:

- Creating `vnet-smakonnect-lab-01` with the address space `10.10.0.0/16`.
- Configuring Management, Servers, Applications, and Private Endpoints subnets.
- Creating `nsg-smakonnect-servers-01`.
- Associating the NSG with the Servers subnet.
- Configuring an inbound RDP rule from the Management subnet.
- Reviewing NSG rule priorities, default rules, and traffic evaluation.
- Validating the address ranges, subnet configuration, and NSG association.

One important design consideration was understanding that an NSG association determines where rules are applied, while each rule's source and destination settings determine which traffic it matches.

I also verified that allowing a specific connection does not automatically deny all other internal connections.

This network became the foundation for the Windows and Linux workloads deployed in Phase 5.

[Read Phase 04 Documentation](phase-04-azure-virtual-networking/azure-virtual-networking-configuration.md)

### Phase 05 — Windows Server and Linux Virtual Machines

In Phase 5, I deployed two virtual machines into the existing Servers subnet and used them to validate operating system administration, private connectivity, and internal application hosting.

**Windows Server deployment**

I deployed Windows Server 2022 Datacenter: Azure Edition on a `Standard_B4als_v2` VM with a Standard SSD OS disk and Trusted Launch security.

I connected through RDP and performed administration tasks using Server Manager, Task Manager, Event Viewer, Windows services, Windows Firewall, and PowerShell.

**Ubuntu Linux deployment**

I deployed Ubuntu Server 24.04 LTS on a `Standard_B2als_v2` VM with a 30 GiB Standard SSD OS disk and Trusted Launch.

I configured SSH public-key authentication and administered the server through macOS Terminal.

I used Linux commands to inspect network interfaces, routing, storage, CPU, memory, running services, and system logs.

**Private connectivity and troubleshooting**

Both VMs were deployed into `snet-servers`:

- Windows Server: `10.10.2.4`
- Ubuntu Server: `10.10.2.5`

During connectivity testing, I encountered an issue where Ubuntu could not ping Windows, even though Windows could communicate with Ubuntu.

I investigated the problem by checking Ubuntu's routing table, testing TCP connectivity, confirming SSH service availability, and verifying outbound HTTPS access.

The issue was traced to Windows Firewall filtering inbound ICMP echo requests.

I created a temporary rule restricted to Ubuntu's private IP, confirmed that ping then succeeded, and removed the diagnostic rule afterward.

**Internal application hosting**

I installed Nginx on Ubuntu and configured a static internal website for Smakonnect Tech Innovation.

I validated that:

- Nginx was active and listening on TCP port 80.
- Ubuntu returned a successful HTTP response locally.
- Windows could establish a TCP connection to Ubuntu on port 80.
- Microsoft Edge on Windows could display the website using `http://10.10.2.5`.

**Network security validation**

I reviewed the Servers subnet NSG and confirmed that no custom inbound rule allowed public HTTP traffic.

An HTTP connection attempt from my Mac to Ubuntu's public IP timed out, consistent with the intended NSG restrictions.

This phase brought together compute, operating system administration, private networking, service configuration, and practical troubleshooting in one working environment.

[Read Phase 05 Documentation and View Screenshots](phase-05-azure-virtual-machines/azure-virtual-machines-deployment.md)

---

## Troubleshooting and Operational Experience

One of the main purposes of this repository is to demonstrate how I approach infrastructure issues, not just successful deployments.

The Phase 5 connectivity problem is a good example.

**Issue:** Ubuntu could not ping Windows over the private Azure network.

**Investigation:** I checked Linux routing, verified TCP connectivity, confirmed that SSH was running, and tested external HTTPS access.

**Finding:** Private networking was functioning, but Windows Firewall was preventing inbound ICMP echo requests.

**Resolution:** I applied a temporary, source-restricted firewall rule, retested connectivity successfully, and removed the rule after confirming the cause.

**Operational lesson:** Troubleshooting should isolate the failing component rather than changing multiple configurations at once. A failed ICMP test does not necessarily indicate a routing or server availability problem.

As the project develops, I will continue documenting relevant configuration issues, diagnostic commands, findings, and resolutions.

---

## Security and Operational Considerations

Security is part of the infrastructure design and validation process throughout this project.

The controls and practices addressed so far include:

- Resource organization and governance through tags, Azure Policy, and resource locks.
- Azure RBAC review and permission-scope analysis.
- Network traffic management through subnet-associated NSGs.
- Restricted temporary RDP and SSH access for administration.
- SSH public-key authentication for Ubuntu.
- Guest operating system firewall troubleshooting.
- Private IP communication between servers.
- Validation of internal website access and public HTTP restrictions.
- Awareness of cost and resource lifecycle management.

The current environment is a lab, so some administrative access methods are intentionally temporary.

Future hardening work may include Azure Bastion, private connectivity, tighter destination-specific NSG rules, HTTPS, additional monitoring, and improved automation.

These are planned improvements rather than features already deployed.

---

## Cost Management

The environment runs under a limited-credit Azure subscription, so resource selection and operating costs are part of my deployment decisions.

I use appropriately sized virtual machines, Standard SSD storage, and Azure budget alerts to help control spending.

VM automatic shutdown has also been configured, and I stop and deallocate compute resources when they are not required for active lab work.

I understand that deallocating a VM stops compute billing but does not necessarily eliminate charges for attached disks, public IP addresses, and other retained resources.

The objective is to maintain a useful working environment without provisioning unnecessary services.

---

## Documentation Approach

I maintain this repository as an ongoing technical record.

Each phase is documented with an emphasis on:

- Infrastructure requirements and objectives.
- Resources deployed and their configurations.
- Technical decisions and the reasoning behind them.
- Validation commands and observed results.
- Issues encountered and troubleshooting performed.
- Security and cost considerations.
- Screenshots and supporting evidence.
- Improvements identified for future implementation.

I aim to keep the documentation clear enough for another administrator to understand the environment and follow the technical decisions.

Screenshots and configuration details are included where they provide useful evidence, while credentials, private keys, and other sensitive information are excluded.

---

## Current Status and Next Steps

**Completed: 5 of 12 phases — 42%**

The environment currently includes:

- An established Azure resource group and governance configuration.
- A four-subnet virtual network.
- A Network Security Group associated with the Servers subnet.
- A deployed Windows Server 2022 virtual machine.
- A deployed Ubuntu Server 24.04 virtual machine.
- Validated private connectivity between the servers.
- An Nginx-hosted internal website accessible from Windows.
- Documented firewall troubleshooting and network security testing.

### Current Focus — Phase 06: Azure Storage and Data Protection

The next phase will extend the environment with Azure Storage services.

Planned work includes storage account configuration, Blob Storage, Azure Files, access management, data protection, and connectivity testing from existing infrastructure.

Later phases will build on this foundation with advanced networking, monitoring, backup and recovery, automation, and operational troubleshooting scenarios.

---

## Portfolio Purpose

This repository is a practical demonstration of how I approach Azure infrastructure administration.

It brings together resource deployment, systems administration, network configuration, security controls, service validation, troubleshooting, and technical documentation.

The goal is to maintain a credible record of implemented work that I can discuss in technical interviews, use as a reference for future projects, and continue improving as Azure services and operational practices evolve.

**Project:** Smakonnect Tech Innovation — Azure Infrastructure Administration  
**Focus:** Azure Administrator | Systems Administrator | Cloud Support | MSP Operations  
**Status:** Active development — Phase 06 in progress

