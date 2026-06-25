# Library vs. Exam — Placement Audit

Comparing the `library/` module/unit layout against the **official exam objectives** (Skills Measured, as of April 17, 2026). The exam taxonomy is the authority; MS Learn's module grouping is not. Below are the real mismatches — places where a unit's home in the library doesn't match where the *exam* scores it.

Legend: 🔴 move/re-tag · 🟡 split or flag · 🟢 fine as-is but worth a note

---

## 🔴 1. `1-identity-governance/03-azure-architecture` is split-brained
Units: `2-what-microsoft-azure`, `3-azure-accounts`, `5-physical-infrastructure`, `6-management-infrastructure`.
- **Off-syllabus (AZ-900, not scored on AZ-104):** units 2, 3, 5 — "what is Azure," accounts, regions/AZs/datacenters. This is fundamentals padding. *Don't waste map space on it.*
- **Core governance (scored):** unit **6 management-infrastructure** = resource groups, subscriptions, **management groups**. Confirmed by reading it. This maps straight to exam objective *"Manage subscriptions and governance → Manage resource groups / Manage subscriptions / Configure management groups."*
- **Action:** treat unit 6 as a **governance** unit (belongs with `04-policy-resource-locks`), and mark units 2/3/5 as skip-for-exam. The module title hides that 3/4 of it is AZ-900.

## 🔴 2. Backup content living inside a Compute module
`3-compute/01-intro-vms/6-backup-services.md` — confirmed: it's a general Azure Backup overview. Backup is a **Domain 5 (Monitor & maintain → Implement backup and recovery)** objective, not compute. The VM module just name-drops it.
- **Action:** when building maps, pin this fact to the **monitor-backup** map, not compute. Same for `01-intro-vms/5-high-availability` — that concept is really the `02-vm-availability` module's job.

## 🟡 3. RBAC "view activity logs" straddles two domains
`1-identity-governance/05-rbac/6-view-activity-logs.md` — confirmed: it's about the **Azure Activity Log** (a Monitor feature) used to audit RBAC changes. It sits in identity but the *mechanism* (Activity Log) is Domain 5 Monitor.
- **Action:** keep it in RBAC (the use-case is access auditing), but cross-link to Monitor. This is exactly the "where did I see Activity Log?" confusion — answer: it's a Monitor tool that shows up in the RBAC context.

## 🟡 4. Routes module over-weights NVAs
`4-networking/05-routes` units 4/5/6 are about **Network Virtual Appliances**. Exam objective is just *"Configure user-defined routes (UDRs)."* NVA is the *why* (forced tunneling through an appliance), not a scored skill on its own.
- **Action:** compress NVA to a footnote on the routes map; spend the space on UDR mechanics + system routes.

---

## Bigger structural gaps — exam objectives with NO matching module
These are scored but the library has **no raw content** (MS Learn doesn't teach them in the AZ-104 paths). Flagged in STRUCTURE.md too; this is the authoritative list mapped to exact objectives:

| Domain | Exam objective with no library coverage |
|---|---|
| 1 Governance | Apply/manage **tags**; **Cost Management** (alerts, budgets, **Azure Advisor**); **management groups** (only lightly in arch unit 6) |
| 1 Identity | **Manage external users** (guest/B2B) — not in `02-manage-identities` |
| 2 Storage | **Stored access policies**; **manage access keys**; **identity-based access for Azure Files**; **soft delete** & **blob versioning** (blob module has tiers/lifecycle but check soft-delete); **Storage Explorer / AzCopy** |
| 3 Compute | **Bicep** (only ARM/JSON taught); **encryption at host**; **move VM across RG/sub/region**; **manage VM disks**; **Azure Container Registry**; **Azure Container Apps** (only ACI taught) |
| 4 Networking | **Azure Bastion**; **service endpoints**; **private endpoints**; **internal/public Load Balancer config + troubleshoot** (only the intro "what is LB" is taught); **troubleshoot connectivity** |
| 5 Monitor | **Log Analytics workspace / KQL queries**; **action groups**; **alert processing rules**; **Recovery Services vault** vs **Backup vault**; **Azure Site Recovery** + failover; **Connection Monitor** |

→ The single biggest under-coverage is **Domain 5 Monitor**: action groups, KQL, alert rules, Site Recovery — all scored, almost none in the raw library (only the one VM-monitoring module). This is also where you said you mix things up. High-priority for our own notes.

---

## Mismatches that are NOT problems (don't "fix" these)
- 🟢 **App Service "use Application Insights"** (`04-app-service/10`) — App Insights is a Monitor tool but it's correctly taught in the App Service context; the exam tests it under App Service config. Leave it.
- 🟢 **SSPR + directory branding** (`06-sspr`) — branding unit feels off but SSPR customization legitimately includes it. Fine.
- 🟢 **Network Watcher** under networking — it's *also* a Monitor-domain objective ("Use Network Watcher and Connection Monitor"). It's taught in networking but scored in BOTH. Cross-link, don't move.

---

## Proposed adjustments (minimal — don't over-engineer the folders)
The raw library folders should **stay as-is** (they mirror MS Learn = useful provenance). The adjustments live as **metadata**, not folder moves:
1. Add a per-unit tag of which **exam objective** it serves (some units → 0 objectives = skip; some → an objective in a *different* domain).
2. The cross-domain pins (backup-in-compute, activity-log-in-rbac, network-watcher-in-both) become **explicit links on the maps** — turning your "where did I see this?" problem into the actual answer.
3. The gap table above = the backlog of **our-own-notes** topics, prioritized with Monitor first.
