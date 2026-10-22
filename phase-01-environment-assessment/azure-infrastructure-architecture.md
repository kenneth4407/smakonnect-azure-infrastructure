# Phase 1 — Azure Infrastructure Architecture Design

## 1. Project Overview

For the Smakonnect Tech Innovation project, I'm designing a small Azure environment where I can practice managing cloud infrastructure.

The environment will include virtual networking, Windows and Linux servers, cloud storage, and monitoring.

My goal is to build something that is organized, secure, and manageable while keeping the cost low enough for a student lab.

This document covers the architecture I've planned before starting the deployment.

## 2. Azure Environment

I've selected **Canada Central** as the main region for the project.

I plan to organize the lab resources under the following resource group:

`rg-smakonnect-lab-01`

Using a dedicated resource group will make it easier to manage related resources, monitor costs, and clean up the environment after completing labs.

I'll also use consistent naming conventions to make resources easier to identify.

## 3. Network Design

The main virtual network will be:

**Virtual network:** `vnet-smakonnect-lab-01`

**Address space:** `10.10.0.0/16`

I've divided the network into four subnets:

| Subnet | Address Range | Purpose |
|---|---|---|
| Management | `10.10.1.0/24` | Management and administrative resources |
| Servers | `10.10.2.0/24` | Windows Server workloads |
| Applications | `10.10.3.0/24` | Linux and application workloads |
| Private Endpoints | `10.10.4.0/24` | Private connectivity to supported Azure services |

I chose this layout to keep different workloads organized and to practice controlling network traffic between them.

During deployment, I'll configure and test network security rules to understand how access between resources can be restricted.

## 4. Virtual Machine Design

I plan to deploy two virtual machines during the compute phase.

### Windows Server

**Planned name:** `vm-smk-win01`

This VM will be used for Windows Server administration, remote access, disk management, monitoring, and troubleshooting.

It will be placed in the Servers subnet.

### Ubuntu Linux

**Planned name:** `vm-smk-linux01`

This VM will be used for Linux administration, SSH, networking, and basic application-related exercises.

It will be placed in the Applications subnet.

To reduce costs, I'll deploy the virtual machines one at a time rather than keeping both running.

I'll choose the VM sizes and disk configurations after checking availability and pricing.

## 5. Storage Design

I plan to create an Azure Storage account to practice managing files and storage security.

**Proposed storage account name:** `stsmakonnectlab01`

The planned configuration includes:

- Standard performance
- Locally redundant storage (LRS)
- Hot access tier
- Secure transfer required
- Anonymous blob access disabled
- Blob soft delete enabled

I'll create private containers for sample company documents and application logs.

The storage account name may need to change if it is already in use, since Azure Storage account names must be globally unique.

I'll use test data only, not real company or personal information.

## 6. Monitoring and Cost Management

I'll use Azure Monitor to check resource performance and investigate activity in the environment.

My initial monitoring plan includes built-in metrics and Azure Activity Logs.

As the project progresses, I also plan to practice creating alerts and using Log Analytics and KQL, provided the additional costs are manageable.

Because my subscription has limited credits, I'll check estimated costs before deploying services and remove resources that are no longer needed.

## 7. Security Considerations

Security will be part of the project from the beginning.

My initial approach includes:

- Using Azure RBAC to manage resource permissions.
- Applying network security rules to control traffic.
- Avoiding unnecessary public access to virtual machines.
- Keeping storage containers private.
- Using secure connections when accessing Azure services.
- Reviewing configurations before deploying resources.

Some identity administration features are restricted because the Microsoft Entra tenant is managed by my school. I'll work within those limits throughout the project.

## 8. Implementation Plan

The environment will be built in stages rather than deploying everything at once.

I'll start with resource organization and governance, followed by networking, virtual machines, storage, and monitoring.

For each phase, I'll document the configuration steps, verification results, issues encountered, and lessons learned.

This will allow me to compare the original design with what I actually deploy.

## 9. Next Steps

The architecture planning is complete.

My next task will be to start the Azure governance phase, where I'll create the resource group, apply naming and tagging standards, and review basic cost-management controls.

---

**Status:** Architecture design completed. Resources have not yet been deployed.
