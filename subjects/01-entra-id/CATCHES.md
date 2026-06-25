# Entra ID — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
(Covers all three Entra sub-decks: a-fundamentals, b-manage-identities, c-sspr.)
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Licenses vs roles (the classic mix-up)
- ✓ **A license is NOT a role** — to give users **P1/P2 features**, assign a **license** in the **Licenses blade → Assignments** (per user, or per group). Assigning a *directory role* (e.g. User Administrator) grants permissions, not features — it does nothing for feature access.
- **Features need an *active* license** — only users with an assigned, active license can use the licensed Entra services.
- **Licenses are per-tenant and non-transferable** — you can't move licenses to another tenant.
- **Bulk-create users ≠ licensed users** — bulk create only provisions accounts; each user still needs a license assigned explicitly.
- **External collaboration settings ≠ internal feature access** — those control guest/B2B access, not whether your own users get P1 features.

## Group-based licensing
- **Assigning a license to a group requires P1** — group-based licensing is itself a P1 feature (auto-applies the license to all members). Manual per-user assignment works on any tier.

## Admin roles & assignment
- ✓ **To assign an admin role: user profile → Assigned roles → Add assignments** — the direct path. Adding the user to an "admin group" does NOT auto-grant a role (unless it's a role-assignable group that already has the role).
- **Administrative Unit (AU) ≠ role assignment** — an AU *scopes/restricts* a role to part of the org (the "where"), it doesn't grant the role (the "what"). Decoy when the task is just "assign role X."
- **My Staff** — delegates limited user management (e.g. password reset, phone) to a non-IT authority like a store manager / team lead for their people. Nothing to do with admin roles.
- **Application Administrator vs Cloud Application Administrator** — same permissions EXCEPT App Admin can manage **application proxy**, Cloud App Admin can't. Neither is auto-added as owner of apps they create.

## Groups — security vs Microsoft 365
- ✓ **Expiration policy (auto-delete after N days) is Microsoft 365 groups ONLY** — security groups don't support it. So "group must auto-delete after 180 days" forces an **M365 group** (assigned *or* dynamic — membership type doesn't matter, group type does).
- **Security group vs M365 group** — security group = access/permissions to resources. M365 group = collaboration (SharePoint, Teams, Outlook mailbox) + lifecycle/expiration policy. When a M365 group expires, its resources (SharePoint library, Team, mailbox) are deleted too.

## Devices
- ⚠ **There's a max-devices-per-user limit** (5 / 10 / 20 / 50 / unlimited) in the device settings blade — hit the cap and the user **can't register/join any more device** until one is removed or the limit is raised. *(New to me, but reasoned to it.)*
- **Reasoning clue for "one user, new device, fails":** if she registered devices *before* (role is fine) and *other users* can still join (tenant settings fine), the issue is specific to her → she hit her **per-user device limit**. Not a role, group, or domain problem.
