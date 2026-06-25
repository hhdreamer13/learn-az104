# Governance — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Resource locks
- ✓ **A `ReadOnly` lock blocks a resource move** — moving a resource into (or out of) an RG that has a ReadOnly lock fails with "Moving resources failed"; you must remove the lock first. `ReadOnly` = read only, no update/delete/move.
- **`CanNotDelete` vs `ReadOnly`** — CanNotDelete = can read + modify, can't delete. ReadOnly = can read only (no modify, no delete). In the portal these show as **Delete** and **Read-only**.
- **Locks inherit downward** — a lock on a parent scope (sub / RG) applies to all resources inside it. And **locks override RBAC** — even an Owner can't delete past a CanNotDelete lock.

## Moving resources
- **Moving a resource does NOT change its region** — move only changes the RG/subscription; the resource stays in its original location. (Region change = a separate operation, Azure Resource Mover.)
- **You can move a resource to any existing RG** in the same tenant — unless a lock (ReadOnly/CanNotDelete on source or target) or an unsupported resource type blocks it.
- ✗ **Default to "it MOVES" for the common resources** — VM, VNet, NIC, **storage account**, **Recovery Services vault**, NSG, public IP, and disks all support move to another RG/subscription. The "only some types support it" caveat applies to a *narrow* set of genuinely unsupported types — NOT the everyday resources the exam lists. The exam trap is usually the reverse: it baits you to exclude storage/vault, but those DO move. *(Note to self: I wrongly assumed storage + Recovery Services vault can't move — they can.)*
- ✗ **On a move, region stays but policy changes** — two variables, don't flip the wrong one: the resource keeps its **region** (never moves physically), but it now **inherits the destination RG's policy**, not the old one. (App Service plan region can't change either; to run in a new region → app cloning.)
