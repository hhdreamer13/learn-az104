# Backup & Recovery — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Azure Backup — what blocks a VM backup
- ✓ **Azure Backup snapshots the VM's DISKS, so power state is irrelevant — a scheduled backup runs even if the VM is stopped/deallocated.** Auto-shutdown times and OS type (Linux vs Windows) are red herrings; both back up fine regardless of when they're powered off. The real requirement: **the VM must be in the same region as the Recovery Services vault**. Anchor: **backup = disk snapshot → power state doesn't matter; same-region does.**
- **Only SAME-REGION VMs can back up to a vault — resource group and OS are IRRELEVANT.** "Which VMs can back up to a vault in region X?" → keep only the VMs in region X; ignore RG, OS (Windows/Ubuntu/RHEL all fine), and any monitoring-agent noise in the stem. (Vault in SE Asia → only the SE-Asia VMs qualify, even across different RGs.)

## Restore options (file-level vs full VM)
- ✗ **File-level recovery only works to an OS-version-COMPATIBLE machine** — you mount a script that exposes the recovery point's disk, and the host machine must run the **same (or compatible) OS version**. Can't restore a file from a Server 2016 VM onto Server 2012/2019. So "recover a file from TD2 (2016)" → **TD2 only** (different OS versions = incompatible). *(The "why OS matters" — it's a file-recovery-only constraint, easy to miss.)*
- **Full VM restore has 3 options: Create new VM · Restore disk · Replace existing disk (OLR).** Replace-disk (OLR) needs the **original VM to still exist** (it snapshots the current VM first, keeps the old disk in the RG). Restoring a VM goes back to itself / a new VM — not onto a different existing VM.

## Deleting a vault / its resource group
- ✓ **A Recovery Services vault BLOCKS deletion (of itself or its RG) while it has backup dependencies** — protected items, backup data (even **soft-deleted**), or registered storage accounts. To delete the RG: **stop the backup first** → disable soft delete → then delete. Deleting/restarting the VM or storage does NOT help; only removing the vault's protected data does. (Error if you don't: "Failed to delete resource group.") *(Another "do FIRST?" — but got it: the blocker is the vault's protected backup.)*

## Backup reports → Log Analytics (region exception)
- **Backup reporting data → a Log Analytics workspace in ANY region/subscription** — it does NOT need to match the vault's region. (Contrast the rule above: VM backup needs same region as the *vault*; sending *report data to Log Analytics* does not.) So given vault in Southeast Asia + workspaces in East Asia / SE Asia / Australia Central → **all three** are valid. Backup Reports = Azure Monitor Logs + workbooks; Log Analytics default retention 30 days (raise it for long-term reporting).

## Order of operations
- ✗ **"What to create FIRST?" → Recovery Services Vault** (the container). Order: **Vault → Backup policy → apply policy to protect VMs.** A backup policy and MABS both *require* a vault to exist first. *(Same "which is FIRST?" prerequisite trap as managed-identity #10 — create the container before what goes inside it.)*
- **Recovery Plan ≠ backup** — a Recovery Plan groups machines for **failover** (Azure Site Recovery / DR). If the task says "backup," it's a vault/policy answer; a recovery plan is disaster recovery. DR ≠ backup.
- **Backup policy = schedule + retention** — *how often* recovery points are taken and *how long* kept. Lives inside the vault.
