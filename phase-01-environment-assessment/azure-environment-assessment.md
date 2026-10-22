# Phase 1 — Azure Environment Assessment

## 1. Purpose of the Assessment

Before deploying resources for the Smakonnect Tech Innovation project, I carried out an initial assessment of my Azure environment.

The goal was to understand what resources and permissions were available, identify any limitations, and plan how to use the subscription without unnecessary costs.

I wanted to make sure I understood the environment before moving into the actual deployment.

## 2. Environment Overview

For this project, I'm using a school-provided Visual Studio Professional Azure subscription for hands-on learning.

I reviewed the subscription in the Azure Portal and confirmed that it was active and available for lab activities.

I selected **Canada Central** as the main region for the project.

I also checked the available compute quotas to make sure the subscription could support the virtual machines I plan to deploy later.

At the time of the assessment, I had not deployed any resources specifically for the Smakonnect project.

## 3. Cost and Resource Planning

Since I'm working with limited Azure credits, cost management is an important part of this project.

Before deploying resources, I reviewed the available credits and considered the costs of running virtual machines, storage, and other Azure services.

To manage costs, I plan to:

- Use smaller virtual machine sizes where appropriate.
- Deploy Windows Server and Linux virtual machines one at a time.
- Deallocate virtual machines when they are not in use.
- Remove resources that are no longer needed.
- Review estimated costs before deploying new services.

I also plan to use Azure Cost Management to track spending throughout the project.

## 4. Access and Environment Limitations

I checked my permissions through **Access control (IAM)** in the Azure Portal.

The assessment confirmed that I have the **Owner** role at the subscription level.

This gives me permission to manage Azure resources within the subscription, subject to any existing restrictions.

However, I also learned that subscription-level Owner access is different from having administrative access to Microsoft Entra ID.

Because the Microsoft Entra tenant is managed by my school, I don't have full access to tenant-level identity administration.

I'll work within the permissions available to me and use alternative learning approaches for tasks that require additional access.

## 5. Key Findings and Next Steps

This assessment helped me understand why it's important to review an Azure environment before starting a deployment.

My main takeaways were:

- Confirming subscription access and permissions should be one of the first steps.
- Resource availability and regional quotas need to be considered when planning virtual machines.
- Cost management is especially important when working with a limited lab budget.
- Azure subscription permissions and Microsoft Entra ID permissions serve different purposes.

With the initial assessment completed, my next step is to document the infrastructure architecture for Smakonnect Tech Innovation.

This will include the virtual network design, subnet layout, virtual machine plans, storage configuration, and monitoring approach.

Once the architecture documentation is complete, I'll move on to configuring the Azure environment.

---

**Phase 1 Status:** Environment assessment completed. Infrastructure deployment has not started.
