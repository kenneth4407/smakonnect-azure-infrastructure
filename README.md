# Smakonnect Tech Innovation | Azure Infrastructure Project

### Building and Managing an Azure Environment — AZ Hands-On Project

## About This Project

I'm working on this project to build my practical skills in Microsoft Azure and systems administration.

I've been studying Azure administration, and I wanted to do more than just read documentation or watch tutorials. I decided to build an environment where I could practice the kind of work an Azure Administrator or IT Support Engineer might handle on the job.

For this project, I'm using **Smakonnect Tech Innovation** as a sample company that needs its IT infrastructure set up and managed in Azure.

I'll be working through everything in stages, from planning the environment to deploying servers, configuring networking, managing storage, monitoring resources, and troubleshooting problems.

I'm also documenting what I do along the way, including things that don't work the first time and how I fix them.

This project is part of my preparation for the **Microsoft AZ-104 certification** and my goal of working in cloud and systems administration.

## What I Want to Achieve

By the end of this project, I want to be comfortable with:

- Setting up and managing Azure resources.
- Creating virtual networks and configuring subnets.
- Deploying and managing Windows Server and Linux virtual machines.
- Working with Azure Storage and controlling access to data.
- Understanding Azure permissions and security.
- Monitoring resources and investigating issues.
- Using PowerShell, Azure CLI, and Bicep to automate tasks.
- Troubleshooting common problems that come up in an Azure environment.
- Writing clear technical documentation that another IT administrator could follow.

I also want to understand *why* certain configurations are used, instead of just memorizing steps.

## The Scenario

For this lab, I'm acting as the Azure Administrator for Smakonnect Tech Innovation.

The company needs a basic Azure environment that can support servers, applications, and file storage.

My responsibility is to plan the environment, build it, secure it, and make sure it can be monitored and maintained.

This is a simulated company environment, not a live production system.

## Tools and Technologies

I'll be working with the following technologies throughout the project:

| Area | Tools and Services |
|---|---|
| Cloud | Microsoft Azure |
| Networking | Virtual Networks, Subnets, NSGs, DNS |
| Servers | Windows Server, Ubuntu Linux |
| Storage | Azure Blob Storage, Storage Accounts |
| Security | Azure RBAC, Azure Policy |
| Monitoring | Azure Monitor, Activity Logs, Log Analytics |
| Automation | PowerShell, Azure CLI, Bicep |
| Documentation | GitHub, Markdown, Network Diagrams |

Some of these are planned for later phases and haven't been deployed yet.

## My Azure Lab Setup

I'm using a school-provided **Visual Studio Professional Azure subscription** for hands-on learning.

I've confirmed that I have the Owner role on the Azure subscription, which gives me access to manage Azure resources within the subscription's limits.

However, the Microsoft Entra tenant is managed by my school, so I don't have access to all the identity administration features. For those areas, I'll document what I learn and use alternative labs where possible.

I'm also working with limited Azure credits, so I need to pay attention to costs and clean up resources when I'm finished with them.

## Planned Network Setup

I've decided to use **Canada Central** as my main Azure region.

My planned virtual network is:

`vnet-smakonnect-lab-01`

Address space: `10.10.0.0/16`

| Subnet | IP Range | What I'll Use It For |
|---|---|---|
| Management | 10.10.1.0/24 | Administration and management |
| Servers | 10.10.2.0/24 | Windows Server |
| Applications | 10.10.3.0/24 | Linux and application workloads |
| Private Endpoints | 10.10.4.0/24 | Private connections to Azure services |

I chose separate subnets so I can practice network organization, security rules, and troubleshooting.

The network is still in the planning stage. I'll deploy and test it during the networking phase.

## Virtual Machines

I plan to work with two virtual machines:

**Windows Server:** I'll use this to practice server administration, remote access, disk management, monitoring, and troubleshooting.

**Ubuntu Linux:** I'll use this to practice SSH, Linux administration, networking, and basic application hosting.

Because my Azure credits are limited, I'll deploy the servers one at a time instead of leaving both running.

## Storage and Monitoring

For storage, I plan to use Azure Blob Storage with private containers, locally redundant storage (LRS), and soft delete.

I want to practice uploading files, managing access, and recovering accidentally deleted data.

For monitoring, I'll start with Azure Monitor metrics and Activity Logs. Later, I'll explore alerts, Log Analytics, and KQL where my subscription budget allows.

## Project Roadmap

I'll complete the project in 12 phases and update this repository as I go.

| Phase | What I'll Be Working On | Status |
|---|---|---|
| 01 | Azure Subscription Assessment and Architecture Planning | In Progress |
| 02 | Resource Groups, Tags, and Azure Governance | Not Started |
| 03 | Microsoft Entra ID Concepts and Azure RBAC | Not Started |
| 04 | Virtual Networks, Subnets, and Network Security | Not Started |
| 05 | Windows Server and Linux Virtual Machines | Not Started |
| 06 | Azure Storage and Data Protection | Not Started |
| 07 | Advanced Networking and Hybrid Connectivity | Not Started |
| 08 | Azure Monitoring and Troubleshooting | Not Started |
| 09 | Azure Backup and Disaster Recovery | Not Started |
| 10 | PowerShell, Azure CLI, and Bicep | Not Started |
| 11 | IT Support and MSP Troubleshooting Scenarios | Not Started |
| 12 | Final Project Review and AZ-104 Interview Preparation | Not Started |

## How I'm Documenting My Work

After each phase, I'll add documentation showing:

- What I was trying to accomplish.
- The steps I followed.
- Screenshots or command outputs showing the results.
- Problems I ran into and how I solved them.
- What I learned and what I would do differently.

I want the documentation to be useful both as a record of my learning and as something I can refer back to when preparing for technical interviews.

## Cost and Security

Since this is a student lab, I have to be careful with my Azure credits.

My plan is to check costs before deploying resources, avoid leaving virtual machines running unnecessarily, and remove resources I no longer need.

I'll also make sure not to upload passwords, access keys, personal information, or sensitive Azure account details to GitHub.

## Current Progress

So far, I've:

- Checked my Azure subscription and available credits.
- Confirmed my Azure subscription Owner role.
- Reviewed some of the Azure Policy assignments.
- Checked the available regional compute quota.
- Planned the virtual network and subnet layout.
- Decided how I'll deploy my Windows and Linux virtual machines.
- Planned the storage, monitoring, and cost-management approach.

**Next up:** Finish documenting Phase 1, then start creating and configuring Azure resources.

---

*This is an ongoing hands-on learning project. I'll update the repository as I complete each phase and gain more practical experience.*
