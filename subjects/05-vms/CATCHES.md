# VMs / Compute — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
(Covers VMs, availability, scale sets, and cross-compute placement.)
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Downtime: which VM changes need a reboot / stop
- ✗ **Downtime-causing ops = resize the VM (reboots) + detach a NIC (needs stop/deallocate).** Hot (no downtime): **attach a data disk**, **add a DSC/extension** (applies in background), **move resource group** (logical-only). Anchor: resize = reboot · NIC detach = must deallocate · everything else here is hot. *(Missed — got one of the two.)*

## Host / redeploy
- ✓ **"Move the VM to a new Azure host" = Redeploy** (Support + Troubleshooting) — it moves the VM to a new node and powers it back on. **Moving it to a new resource group does NOTHING physical** — an RG is just a logical container; the VM stays on the same host. (Same logical-vs-physical trap as PPG/storage: don't let "resource group" imply a hardware move.)

## Availability Set — update vs fault domains
- ✓ **KEY INSIGHT: UD/FD numbers = how many GROUPS the VMs are split into, NOT a VM count.** Azure reboots **one update domain at a time** during planned maintenance, so **UD=3 → only 1 of 3 groups is down at once → the other 2 groups stay available** (that's how "keep ≥2 available" is met). More groups = fewer VMs down per step.
  - **Planned maintenance → Update Domains; unplanned hardware/rack failure → Fault Domains.** FD = how many separate racks (power+switch) the VMs spread across (FD=2 → a single rack failure loses at most half). FD is for *unplanned*, so it's not the keyword in a "planned maintenance" question.
  - **Gotcha: FD can't be 1 when UD>1** (Azure errors "update domain count must be 1 when fault domain count is 1"), so UD=3 forces **FD≥2**. Answer pattern: **UD=3, FD=2.** Anchor: **planned=UD · unplanned=FD · the number = groups, not VMs.**

## Scale sets (VMSS)
- ✓ **"Automatically increase the number of VMs" = VM Scale Sets, full stop** — it's the ONLY option that autoscales. Availability Set, Availability Zones, and a plain ARM template all give a **fixed** VM count. Bundled catch (HA scope): **Availability Set = spreads VMs across update/fault domains within ONE datacenter** (survives rack/host failure, not a datacenter outage); **Availability Zones = spreads across separate datacenters in a region** (survives a datacenter failure) — but **neither autoscales**. Anchor: **autoscale → VMSS · AS = 1 datacenter · AZ = multi-datacenter.**
- ⚠ **VMSS orchestration: Uniform vs Flexible — set at creation, can NEVER be changed later.** **Uniform** = identical VMs from one model, Azure auto-creates them → **large-scale stateless** (keywords: stateless, identical, at scale). **Flexible** = mixed VM types (up to 1000), you add VMs manually → **stateful / quorum-based**, more control. Map the workload word: *stateless/identical* → Uniform; *stateful/quorum/mixed* → Flexible. *(New fork — reasoned from "stateless".)*

## Extensions / software install
- ✗ **Install software on a VM/VMSS → Custom Script Extension** = two parts: (1) a **configuration script** (what to install) + (2) the **`extensionProfile` section of the ARM template** (attaches it so every instance runs it on deploy). NOT an **Automation Account** (that's runbooks / State Config — the tempting wrong pick), not a new scale set, not the VPN client package. Anchor: "run a script / install software on each VM" = **Custom Script Extension**. *(Missed — picked Automation Account.)*

## Placement & proximity
- ⚠ **Proximity Placement Group (PPG) = keep compute in ONE datacenter for lowest/deterministic latency** — opposite of availability zones (which spread VMs apart for resilience). A PPG and the VM/VMSS using it **must be in the same region**; match by **region (location)**, NEVER by resource group. The shared-RG / RG-location detail is a red herring. *(New concept to me — reasoned it from the region rule.)*
