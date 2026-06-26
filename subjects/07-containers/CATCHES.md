# Containers — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
(Covers ACI, Azure Container Apps, AKS, ACR.)
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Azure Container Apps (ACA)
- ✗ **Container Apps reachability = INGRESS, not a public IP.** With ingress disabled there is **no external endpoint at all** — handing out a "public IP" exposes nothing. Enabling **ingress + target port generates the public HTTPS URL** users hit (ACA replaces the load-balancer/public-IP model). So "can't reach my Container App" → **enable ingress**. IP restrictions only filter traffic that already reaches an endpoint, so disabling them does nothing if there's no ingress. Also: **a Container App can't be moved to another region** (region is fixed at creation). *(Missed — chose wrong fix.)*
