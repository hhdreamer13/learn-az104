# RBAC — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid, not a weak point) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

- ✓ **Logic App Operator can't create Logic Apps** — it only reads, enables, and disables them. To *create*, you need **Logic App Contributor** (least-privilege) or **Contributor**. Same pattern for any `*-Operator` role: operate ≠ create. (Also tested with the wrong role being User Access Administrator — same answer: No, UAA grants access, can't create.)
- ⚠ **Assigning an RBAC role to an Entra *group* at a *resource-group* scope is normal, not a mix-up** — every role assignment is principal (who) × role (what) × scope (where). A group as the principal and an RG as the scope is the textbook shape; assign to groups, not individual users.
- ⚠ **Scope inherits downward only** — a role at a resource group covers everything in that RG, but not the subscription above it. Sub-level assignment flows down to its RGs; RG-level does not flow up.
- ✓ **"Assign a role to other users" → only Owner or User Access Administrator can do it** — the trigger phrase is *grant/assign access to others*. Resource roles (Contributor, VM Contributor, Backup Reader, Storage Blob Data Contributor, etc.) can manage things but can't hand out role assignments.
- ✓ **"Security" roles don't grant access** — Security Admin / Security Reader are Defender-for-Cloud roles; name-bait when the task is assigning RBAC roles. Same for any `*-Contributor` (manages resources, not access).
- ✗ **Managed identity = TWO steps; enabling the identity comes FIRST** — to let a VM's services access another resource: (1) **enable the managed identity** on the VM, then (2) assign it a role in the target's IAM. If the question asks what to do *first*, it's step 1 — you can't role-assign an identity that doesn't exist yet. (Real-world habit jumps to step 2; the word "first" is the trap.)
- **System-assigned vs user-assigned identity** — system-assigned is tied to one resource and deleted with it; user-assigned is a standalone resource reusable across many. Permissions granted via RBAC, no credentials stored.
