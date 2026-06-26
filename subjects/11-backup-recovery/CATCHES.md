# Backup & Recovery — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Azure Backup — what blocks a VM backup
- ✓ **Azure Backup snapshots the VM's DISKS, so power state is irrelevant — a scheduled backup runs even if the VM is stopped/deallocated.** Auto-shutdown times and OS type (Linux vs Windows) are red herrings; both back up fine regardless of when they're powered off. The real requirement: **the VM must be in the same region as the Recovery Services vault**. Anchor: **backup = disk snapshot → power state doesn't matter; same-region does.**
