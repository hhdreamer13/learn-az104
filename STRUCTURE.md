# AZ-104 Structure — Source of Truth

Two views of the same content. **Exam domains** = how you're scored. **Learning paths** = how MS Learn organizes the material (and where the raw `.md` files live). They don't line up 1:1 — that mismatch is itself a "placement" trap worth seeing.

Verified against MS Learn, June 2026. Module slugs are exact (= the `<module-slug>` in the raw-file URL).

---

## A. Exam domains (how you're scored)

| # | Domain | Weight |
|---|--------|--------|
| 1 | Manage Azure identities and governance | 20–25% |
| 2 | Implement and manage storage | 15–20% |
| 3 | Deploy and manage Azure compute resources | 20–25% |
| 4 | Implement and manage virtual networking | 15–20% |
| 5 | Monitor and maintain Azure resources | 10–15% |

Note: there is **no "prerequisites" domain** — Cloud Shell + ARM templates are scored *inside* compute/governance, not separately. (Trap #1: where things are taught ≠ where they're tested.)

---

## B. Learning paths → modules (where the raw `.md` lives)

6 paths, 28 modules total.

### Path 0 — Prerequisites for Azure administrators (2 modules)
`az-104-administrator-prerequisites`
1. Introduction to Azure Cloud Shell — `intro-to-azure-cloud-shell`
2. Deploy Azure infrastructure by using JSON ARM templates — `create-azure-resource-manager-template-vs-code`

### Path 1 — Manage identities and governance (6 modules)
`az-104-manage-identities-governance`
1. Understand Microsoft Entra ID — `understand-azure-active-directory`
2. Create, configure, and manage identities — `create-configure-manage-identities`
3. Describe the core architectural components of Azure — `describe-core-architectural-components-of-azure`
4. Azure Policy initiatives — `sovereignty-policy-initiatives`
5. Secure your Azure resources with Azure RBAC — `secure-azure-resources-with-rbac`
6. Allow users to reset their password with Entra SSPR — `allow-users-reset-their-password`

### Path 2 — Implement and manage storage (4 modules)
`az-104-manage-storage`
1. Configure storage accounts — `configure-storage-accounts`
2. Configure Azure Blob Storage — `configure-blob-storage`
3. Configure Azure Storage security — `configure-storage-security`
4. Configure Azure Files — `configure-azure-files-file-sync`

### Path 3 — Deploy and manage compute resources (5 modules)
`az-104-manage-compute-resources`
1. Introduction to Azure virtual machines — `intro-to-azure-virtual-machines`
2. Configure virtual machine availability — `configure-virtual-machine-availability`
3. Configure Azure App Service plans — `configure-app-service-plans`
4. Configure Azure App Service — `configure-azure-app-services`
5. Configure Azure Container Instances — `configure-azure-container-instances`

### Path 4 — Configure and manage virtual networks (8 modules)
`az-104-manage-virtual-networks`
1. Configure virtual networks — `configure-virtual-networks`
2. Configure network security groups — `configure-network-security-groups`
3. Host your domain on Azure DNS — `host-domain-azure-dns`
4. Configure Azure Virtual Network peering — `configure-vnet-peering`
5. Manage and control traffic flow with routes — `control-network-traffic-flow-with-routes`
6. Introduction to Azure Load Balancer — `intro-to-azure-load-balancer`
7. Introduction to Azure Application Gateway — `intro-to-azure-application-gateway`
8. Introduction to Azure Network Watcher — `intro-to-azure-network-watcher`

### Path 5 — Monitor and back up resources (3 modules)
`az-104-monitor-backup-resources`
1. Introduction to Azure Backup — `intro-to-azure-backup`
2. Protect your virtual machines by using Azure Backup — `protect-virtual-machines-with-azure-backup`
3. Monitor your Azure VMs with Azure Monitor — `monitor-azure-vm-using-diagnostic-data`

---

## C. Placement gaps to watch (the "I mix things up" problem)

MS Learn's 28 modules do **not** fully cover the scored exam objectives. Known gaps the modules under-teach but the exam tests — candidates for our own notes later:
- **Containers**: Learn has ACI only; exam also tests **AKS** and **Azure Container Apps**.
- **IaC**: Learn teaches ARM/JSON; exam also tests **Bicep** and **deployment stacks**.
- **Governance**: Management groups, **resource locks**, tags, **Azure Policy remediation**, Cost Management — thin or scattered across modules 1.3/1.4.
- **Networking**: **VPN Gateway**, **ExpressRoute**, **Azure Firewall**, **Bastion**, **Azure Virtual Desktop** appear in the path's product list but have no dedicated module — Learn under-covers them vs. the exam.
- **Monitor**: **Log Analytics / KQL**, **action groups**, **alert rules** live mostly inside the single VM-monitoring module, not broken out.

So: raw `.md` = complete coverage of *what MS teaches*, not of *what the exam tests*. The delta is where our value-add notes go.

---

## Raw-file URL convention
```
https://raw.githubusercontent.com/MicrosoftDocs/learn/main/learn-pr/wwl-azure/<module-slug>/includes/<unit-slug>.md
```
Unit slugs aren't known until we fetch each module's index — `1-introduction.md` first, last is usually knowledge-check/summary.
