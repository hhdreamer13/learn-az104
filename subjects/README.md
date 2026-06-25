# Subjects — our mental model (flat, no mixing)

11 clean single-subject folders + 1 tooling folder. We deliberately **dropped the 5-domain grouping as structure** — it lumped Entra+RBAC+Policy together and Monitor+Backup together, which is what caused the mixing. Domains survive only as a tiny weight tag below, never as folders.

Raw `.md` files keep their origin in the filename: `<source-module>__<unit>.md`. The untouched MS mirror still lives in `../library/` for provenance.

## The 11 subjects

| Folder | What lives here | Exam domain (weight) |
|---|---|---|
| `00-tooling-cloud-shell` | Cloud Shell (cross-cutting tool, not a scored subject) | — |
| `01-entra-id` | Tenants, users, groups, licenses, external users, SSPR, devices | Identity (20–25%) |
| `02-rbac` | Roles, scopes, assignments, access to Azure resources | Identity (20–25%) |
| `03-governance` | Policy, resource locks, tags, mgmt groups, subscriptions, cost | Governance (20–25%) |
| `04-storage` | Accounts, redundancy, blob, files, SAS, encryption | Storage (15–20%) |
| `05-vms` | VMs, availability sets/zones, scale sets, autoscale | Compute (20–25%) |
| `06-app-service` | App Service plans, web apps, slots, scaling | Compute (20–25%) |
| `07-containers` | ACI, container groups (+ ACR, Container Apps = our notes) | Compute (20–25%) |
| `08-iac` | ARM templates (+ Bicep, deployment stacks = our notes) | Compute (20–25%) |
| `09-networking` | VNets, subnets, NSG/ASG, peering, routes, DNS, LB, App Gateway | Networking (15–20%) |
| `10-monitor` | Azure Monitor, metrics, logs, VM Insights, Network Watcher | Monitor (10–15%) |
| `11-backup-recovery` | Azure Backup, vaults, VM backup/restore (+ ASR = our notes) | Monitor (10–15%) |

## Audit-driven placements (the fixes that stop the mixing)
These are the deliberate moves where the raw MS placement was wrong for the exam:
- **Backup pulled OUT of compute** → `01-intro-vms__6-backup-services.md` now lives in `11-backup-recovery`, not with VMs.
- **`03-azure-architecture` split** → only its mgmt-infrastructure unit (RGs/subs/mgmt groups) went to `03-governance`. The AZ-900 fundamentals units (what-is-Azure, accounts, regions) are parked in `03-governance/_skip-az900/` — **not exam-scored, skip them**. Intro/KC/summary of that module were dropped entirely.
- **Network Watcher placed in `10-monitor`**, not networking — the exam scores it as a Monitor objective ("Use Network Watcher and Connection Monitor"). Cross-reference from networking.
- **App Service plans + App Service merged** into one `06-app-service` subject (they're one mental unit).
- **SSPR folded into `01-entra-id`** (it's an Entra identity feature, not separate).

## Cross-links to remember (the "where did I see this?" answers)
- **Activity Log** → taught in RBAC (`02-rbac`, view-activity-logs) but it's a **Monitor** tool. Answer when confused: it's a Monitor feature used for access auditing.
- **Application Insights** → taught in App Service but it's a Monitor tool.
- **Network Watcher** → in `10-monitor`, but it's a networking diagnostic. Lives in both worlds.

## Gaps = our-notes backlog (no raw coverage, but scored)
Monitor-first priority. Full list in `../AUDIT-placement.md`. Headline gaps: action groups, KQL/Log Analytics, alert rules, Site Recovery (Monitor); Bicep, Container Apps, ACR, Bastion, private/service endpoints (Compute/Net); tags, cost mgmt, external users (Identity/Gov).
