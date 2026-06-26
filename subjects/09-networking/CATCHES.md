# Networking — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
(Covers VNets, connectivity/NSG/peering/routes, DNS, and load balancing.)
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Load Balancer — session persistence
- ✓ **Load Balancer session persistence (sticky sessions) has THREE modes — pick by the EXACT words.** **None** = any VM. **Client IP** = sticks by source IP only. **Client IP and protocol** = sticks by source IP **+ protocol**. Scenario says "same client IP **and protocol**" → the two-part mode (plain "Client IP" is the skim-past trap). Unrelated decoys: **Floating IP** = backend IP mapping for failover/multi-instance, NOT stickiness; **Frontend IP config** = routing (public/private), not persistence.
