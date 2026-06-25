# AZ-104 Decks — Factual Accuracy Audit

**Date:** 2026-06-25
**Status: ✅ ALL FINDINGS RESOLVED.** Every confirmed finding below (16 here + the ACI-GPU fix added later) has been **applied to the HTML decks** and the JS re-verified to parse. Folder 01's two fixes were applied separately. This file is now the **proof record** (deck quote → correct fact → `.md` line → live-docs URL), not an open to-do list. The raw `library/` and `subjects/*.md` mirrors were left untouched as provenance.
**Scope:** All 21 HTML decks (folders 00–11) fact-checked against (a) their own MS Learn `.md` source files and (b) current `learn.microsoft.com` docs where the source looked stale.
**Method:** One fact-checker per deck reading the deck + every source `.md`, then an independent adversarial verifier on each flagged claim (to kill false positives). Two contested claims were additionally re-verified against live docs by hand. Voice/coverage/style were **out of scope** — this log is **factual errors only**.

## The headline pattern
Almost every confirmed error is the **same failure mode: the deck faithfully copied an outdated fact from its MS Learn `.md` source.** The deck authors didn't invent mistakes — Microsoft's training markdown is stale on these points, and current `learn.microsoft.com` docs disagree. That validates the "check live docs" instinct: the raw `library/` is a frozen 2024-era mirror.

## Severity legend
🔴 **high** — will likely cause a wrong exam answer · 🟠 **medium** — wrong, occasionally tested · 🟡 **low** — imprecise/stale, rarely the pivot of a question

## Summary

| Folder | Deck | Findings | Worst |
|---|---|---|---|
| 00 | Cloud Shell | 3 | 🔴 |
| 01 | Entra ID | 2 — **already fixed** | 🟠 |
| 02 | RBAC | 1 | 🟡 |
| 03 | Governance | 0 — clean | — |
| 04a | Storage Accounts | 1 | 🟠 |
| 04b | Blob | 0 — clean | — |
| 04c | Azure Files | 2 | 🟠 |
| 04d | Storage Security | 1 | 🟡 |
| 05 | VMs | 1 (+1 note) | 🟡 |
| 06 | App Service | 1 | 🟠 |
| 07 | Containers | 1 (GPU) | 🟠 |
| 08 | IaC | 0 — clean | — |
| 09a | VNets | 2 | 🟠 |
| 09b | Connectivity | 2 | 🟠 |
| 09c | DNS | 2 | 🔴 |
| 09d | Load Balancing | 0 — clean | — |
| 10 | Network Watcher | 0 — clean | — |
| 10 | VM Monitor | 0 — clean | — |
| 11 | Backup & Recovery | 0 — clean | — |

Clean decks (no factual errors found): **03, 04b, 05\*, 08, 09d, 10-network-watcher, 10-vm-monitor, 11.** (\*05 had one internal contradiction, logged below. 07 was clean except the GPU fix — 07-1.)

---

## Folder 00 — Cloud Shell

### 00-1 🔴 "First launch — must pick storage / a storage account is required"
- **Deck (facet 1 facts + exam):** `['First launch','Must pick storage']` and exam answer *"First-time launch — what is required? A storage account…"*
- **Correct:** A storage account is **not required** to launch Cloud Shell. At first launch you choose **with or without** storage; "No storage account required" starts an **ephemeral session** (the fastest way to start). You only mount storage if you want `$HOME/clouddrive` files to **persist** across sessions.
- **`.md` proof (stale source):** `01-cloud-shell__3-how-azure-cloud-shell-works.md:41` only discusses CloudDrive persistence and never mentions ephemeral/no-storage sessions — it predates the feature.
- **Live docs:** https://learn.microsoft.com/en-us/azure/cloud-shell/features (2025-12-03): *"…you have the option of using Cloud Shell with or without an attached storage account. Choosing to continue without storage is the fastest way to start…"* + https://learn.microsoft.com/en-us/azure/cloud-shell/get-started/ephemeral : *"Ephemeral sessions don't require a storage account."*
- **Fix:** change to "storage is **optional** — pick storage only if you want files to persist; otherwise an ephemeral session needs no storage account."

### 00-2 🟠 "No package installs / no custom runtimes / not extensible"
- **Deck (facet 2 gist/facts/trap + facet 3):** *"You can't sudo or install system packages,"* `['Cannot','sudo / install']`, *"No sudo, no package installs, no custom runtimes. Pre-loaded ≠ extensible."*
- **Correct:** The real restriction is **no root/sudo**, not "no installs at all." With a storage account you **can** install user-level tools — Python modules, PowerShell modules, npm packages, most `wget`-installable tools. (The deck's own tool grid even lists `npm`/`pip`.) What's true: no `sudo`, no **system-package** installs (those need root).
- **`.md` proof (stale source):** `01-cloud-shell__4-when-to-use-azure-cloud-shell.md:15` frames any install as requiring a VM/container.
- **Live docs:** https://learn.microsoft.com/en-us/azure/cloud-shell/features (2025-12-03): *"If you configured Cloud Shell to use a storage account, you can install your own tools. You can install any tool that doesn't require root permissions…"*
- **Fix:** qualify every "no installs / not extensible" to **"no root/sudo (system) installs"**; keep the correct exam takeaway (need a sudo/admin install → use a VM).

### 00-3 🟡 "Env: Not extensible / No custom installs" (the "fences" facet)
- **Deck (facet 3 facts/gist):** `['Env','Not extensible']`, *"No custom installs → need more → a VM or container."*
- **Correct:** Same issue as 00-2 — the accurate fence is **"no root/admin installs and no persistence of system changes outside `$HOME`,"** not "no custom installs at all."
- **Proof:** same as 00-2 (`…__4…md:15`; features doc).
- **Severity note:** low — the exam-relevant takeaway (sudo/admin install → VM) still holds; only the blanket wording overstates.

---

## Folder 01 — Entra ID  ✅ ALREADY FIXED (2026-06-25)

These two were found and **corrected in the HTML** before this audit run. Listed for the record.

### 01-1 🟠 "Entra Domain Services is part of the P1/P2 tier" — **FIXED**
- **Was (deck 01a, facets 2 & 3):** Entra DS listed as a P1/P2 feature (gist, facts row, tier diagram).
- **Correct:** Entra DS is a **standalone Azure service billed per hour by SKU**, not unlocked by P1/P2; runs on Free-tier tenants too.
- **`.md` proof:** `01-entra-id__6-examine-azure-domain-services.md:5` carries the stale "runs as part of the P1 or P2 tier" claim — and **contradicts itself** at line 28 ("charges per hour based on the size of your directory").
- **Live docs:** https://learn.microsoft.com/en-us/entra/fundamentals/licensing — Entra DS *"charges accrue per hour based on the SKU…"*, listed separately from the P1/P2 tables.
- **Applied fix:** removed Entra DS from the P1 ladder in all 4 places; facet 2 now says "separate, per-hour billed service (not a P1/P2 feature)."

### 01-2 🟡 "Entra Joined = No on-prem AD needed" — **FIXED**
- **Was (deck 01b, device facet):** Entra Joined labelled "cloud-only / No on-prem AD needed," implying incompatibility with hybrid.
- **Correct:** Entra Joined works in **both cloud-only and hybrid** orgs and keeps SSO to on-prem resources; the device just isn't joined to on-prem AD.
- **`.md` proof:** `02-manage-identities__7-configure-manage-device-registration.md:46` *"Suitable for both cloud-only and hybrid organizations"* and `:58` SSO-to-on-prem.
- **Applied fix:** label now reads "Not joined to on-prem AD / Works in hybrid orgs too."

---

## Folder 02 — RBAC

### 02-1 🟡 "Azure has 70+ built-in roles"
- **Deck (core-roles gist + facts):** *"Azure has 70+ built-in roles…"* / `['Built-in roles','70+ total']`.
- **Correct:** Current official count is **over 120 built-in roles**. "70+" is a true lower bound (so not a falsehood) but badly understated. The four taught roles (Owner/Contributor/Reader/User Access Administrator) are fully correct.
- **`.md` proof (source contradicts itself):** `05-rbac__4-list-access.md:45` *"more than 70 built-in roles"* vs `05-rbac__8-summary.md:3` *"more than 200 built-in roles."*
- **Live docs:** https://learn.microsoft.com/en-us/azure/role-based-access-control/role-definitions-list (2025-10-23): *"Azure RBAC has over 120 built-in roles…"*
- **Fix:** update "70+" → "120+" in both the gist and the facts row.

---

## Folder 03 — Governance  ✅ CLEAN
No factual errors found. Verified: policy effects (Audit/Deny/DINE/Modify), 24h evaluation cycle, resource locks (CanNotDelete/ReadOnly, Owner/UAA to manage, ReadOnly blocks key-listing), tags don't inherit, mgmt-group 6-level nesting, RG no-rename, Policy-governs-resource vs RBAC-governs-person.

---

## Folder 04a — Storage Accounts

### 04a-1 🟠 "Premium page blobs — LRS only"
- **Deck (account-types tab — facts/gist/scn/exam/diagram):** `['Premium page blobs','VM disks · LRS only']`; singles out page blobs as the only LRS-restricted Premium kind.
- **Correct:** Premium page blob accounts support **LRS and ZRS** — same as Premium block blobs and Premium file shares. None of the three Premium single-service kinds is LRS-only.
- **`.md` proof (stale source):** `01-storage-accounts__4-determine-storage-account-kinds.md` table row *"Premium page blobs … LRS only."*
- **Live docs:** https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview (2026-06-16): *"Premium page blobs … LRS, ZRS"* + footnote that ZRS/GZRS/RA-GZRS are supported for "premium page blob accounts."
- **Fix:** change "LRS only" → "LRS / ZRS" in the gist, the red facts row, the scenario, the exam answer, and the diagram. (This was the deck's *distinguishing* answer, so it actively misleads.)

---

## Folder 04b — Blob  ✅ CLEAN
No factual errors found. Verified: access tiers (hot/cool/cold/archive), rehydration, lifecycle rules, block/page/append blobs, object replication, soft delete/versioning framing.

---

## Folder 04c — Azure Files

### 04c-1 🟠 File Sync limits: "100 sync groups / 99 servers"
- **Deck (File Sync gist + facts):** *"…up to 100 sync groups / 99 servers."*
- **Correct:** Current hard limits per Storage Sync Service are **200 sync groups** and **100 registered servers**. Both deck numbers are stale.
- **`.md` proof (stale source):** `04-azure-files__7-deploy-azure-file-sync.md:7` *"up to 100 sync groups … up to 99 registered Windows Servers."*
- **Live docs:** https://learn.microsoft.com/en-us/azure/storage/file-sync/file-sync-scale-targets (2025-11-03): *"Sync groups per Storage Sync Service | 200"*, *"Registered servers per Storage Sync Service | 100."*
- **Fix:** 100 → **200** sync groups; 99 → **100** servers.

### 04c-2 🟠 File Sync: "up to 50 server endpoints per sync group"
- **Deck (File Sync gist/facts/trap/diagram):** *"up to 50 server endpoints"* / `['Server endpoint','NTFS path · ≤50 · not system vol']`.
- **Correct:** A sync group supports **up to 100 server endpoints** (hard limit). The NTFS-path / not-system-volume detail is correct.
- **`.md` proof (stale source):** `04-azure-files__7-deploy-azure-file-sync.md:9` *"up to 50 server endpoints"* (also `:28`).
- **Live docs:** same scale-targets page: *"Server endpoints per sync group | 100."*
- **Fix:** ≤50 → **≤100** server endpoints (gist, facts, trap, diagram).

---

## Folder 04d — Storage Security

### 04d-1 🟡 "Managed HSM — FIPS 140-2 Level 3"
- **Deck (Encryption tab — gist + facts):** *"Managed HSM (FIPS 140-2 Level 3)"* / `['Managed HSM','FIPS 140-2 Level 3']`.
- **Correct:** Azure Key Vault Managed HSM is now validated to **FIPS 140-3 Level 3** (fleet firmware updated for both Managed HSM and Key Vault Premium). The teaching point (Managed HSM = highest-compliance, Level 3 option for CMK) is right; only the **140-2 → 140-3** generation is stale.
- **`.md` proof (stale source):** `03-storage-security__6-create-customer-managed-keys.md:15` *"Managed HSM provides FIPS 140-2 Level 3 validation…"*
- **Live docs:** https://learn.microsoft.com/en-us/azure/key-vault/managed-hsm/overview (2026-06-15): *"…using FIPS 140-3 Level 3 validated HSMs."*
- **Fix:** 140-2 → **140-3** in the gist and facts row.

---

## Folder 05 — VMs

### 05-1 🟡 Internal contradiction: VM compute "per-minute" vs "per-second"
- **Deck:** gist (line 334) says compute is *"billed **per-minute**"*; the facts row (line 335) says Pay-as-you-go is *"**per-second**, no commit."* Same deck, two different answers.
- **Correct:** **Per-minute** is current. VMs are *priced* per hour but *billed* per minute. The **gist is right; the facts row ("per-second") is the error.**
- **`.md` proof:** the source modules don't state granularity, so the facts-row value was added by the author.
- **Live docs:** https://learn.microsoft.com/en-us/azure/virtual-machines/overview (2026-04-10): *"For partial hours, Azure charges only for the minutes used."* (Note: a widely-circulated "5-minute minimum billing" claim is **unverified community speculation**, not in official docs — do not adopt it.)
- **Fix:** change the facts row "per-second" → **"per-minute"** so both agree.

### 05-note 🟡 "Data disks — max ~2 per vCPU" (worth tightening, not a hard error)
- **Deck (Disks facet facts):** `['Data disks','app data; max ~2 per vCPU']`.
- **Reality:** max data disks is **per VM size**, not a per-vCPU rule. "~2 per vCPU" is a rough heuristic that holds for many D-series sizes but isn't an Azure rule (the `~` softens it, so it's not flatly wrong).
- **Fix (optional):** reword to "max depends on VM size" to avoid teaching a non-rule.

> **Cleared, not an error:** an earlier lightweight pass suspected the per-second/per-minute issue *and* this disk note — the billing one is confirmed above; the disk note is downgraded to advisory. No other 05 errors found (availability sets/zones, fault/update domains, VMSS Uniform/Flexible, RI 72%, Hybrid Benefit all verified correct).

---

## Folder 06 — App Service

### 06-1 🟠 Custom domains wrongly excluded from the Shared tier
- **Deck (Domains & security — facts + exam):** facts *"Custom domain/TLS — higher tiers only"*; exam answer says both custom domain + TLS binding need a tier *"not Free/Shared."*
- **Correct:** A **custom domain** is supported on **Shared and all paid tiers** — only **Free (F1)** is excluded. The **TLS/SSL binding** is what needs **Basic or higher**. The deck conflates the two and wrongly excludes Shared from custom domains.
- **`.md` proof:** `03-app-service-plans__4-scale-up-scale-out.md:25` *"add a custom DNS name… scale your plan up to the **Shared** tier. Next… create an SSL binding, so you scale… up to the **Basic** tier."*
- **Live docs:** https://learn.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-custom-domain — *"must be a paid tier, not the Free (F1) tier"* and *"If you want to remain in the Shared tier… Add certificate later"* (Shared supports the custom domain; only the cert needs Basic+). TLS binding: https://learn.microsoft.com/en-us/azure/app-service/configure-ssl-bindings — *"Basic, Standard, or Premium."*
- **Fix:** split them — "Custom domain → paid tier (Shared+, not Free); TLS/SSL binding → Basic+." Correct the exam answer's "not Free/Shared."

> **Cleared, not an error (important):** an earlier pass claimed the deck's **"Backup needs Basic+"** was wrong and should be "Standard+." That was **re-verified and the deck is CORRECT.** Current docs https://learn.microsoft.com/en-us/azure/app-service/manage-backup (2026-06-02): *"Back up and restore is supported in the **Basic**, Standard, Premium, and Isolated tiers. For the Basic tier, you can only back up and restore the production slot."* — exactly what the deck says. **Do not change the backup tier.**

---

## Folder 07 — Containers  ✅ CLEAN (factually)
ACI vs Container Apps vs AKS vs ACR all verified (container groups, no ACI autoscale, Container Apps KEDA + scale-to-zero, ACR tiers/geo-replication).

### 07-1 🟠 GPU — "ACI supports GPU" / "both ACI and Container Apps support GPU" — **FIXED**
- **Was (deck, ACI facet + comparison facet/trap):** ACI GPU "supported (Linux, preview)"; comparison gist "the deciding axis is … not GPU (all support it)"; trap "No — both support GPU."
- **Correct (verified vs live docs):** **ACI GPU was RETIRED 14 Jul 2025** — ACI has no GPU now. **Azure Container Apps DOES** support GPU (serverless **NVIDIA T4 / A100**, GA). So "both support GPU" was **backwards.** AKS supports GPU node pools.
- **Live docs:** https://learn.microsoft.com/en-us/azure/container-instances/container-instances-gpu — *"This product is retired as of July 14, 2025."* · https://learn.microsoft.com/en-us/azure/container-apps/gpu-serverless-overview — serverless GPUs (T4/A100), pay-per-use, scale-to-zero.
- **Applied fix:** ACI gist + diagram now say "no GPU"; comparison gist + trap reframed to "ACI none · Container Apps serverless GPUs · AKS GPU node pools." Core teaching (the real divide is scaling/orchestration) preserved.

*Coverage note (not an error, out of scope): ACI **restart policies** (`Always`/`OnFailure`/`Never`) are not covered — track in a future coverage pass.*

---

## Folder 08 — IaC  ✅ CLEAN (factually)
No factual errors found. ARM template structure, parameters/variables/outputs, linked vs nested, `az deployment group create` noun-order all correct. *Note: missing **deployment modes (Incremental vs Complete)** and **what-if** are **coverage gaps**, not errors — out of scope here.*

---

## Folder 09a — VNets

### 09a-1 🟠 "Public IPs are dynamic by default" (true only for private IPs)
- **Deck (Static vs dynamic facet gist):** *"Both private and public IPs… **Dynamic is the default**… a dynamic IP can change after deallocation."*
- **Correct:** "Dynamic by default" is right for **private** IPs only. For **public** IPs it's now wrong: Basic SKU (the only one supporting dynamic) was **retired 30 Sep 2025**; the surviving **Standard** SKU is **Static, always**. Any public IP created today is Static and won't change after deallocation.
- **`.md` proof:** `01-virtual-networks__7-associate-public-ip-addresses.md` Standard table *"Allocation method | Static"*; `…__8-associate-private-ip-addresses.md` *"Dynamic assignment is the default"* — stated only for **private** IPs.
- **Live docs:** https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses — *"On September 30, 2025, Basic SKU public IPs were retired"*; Standard public IPv4 = Static (Dynamic ✗).
- **Fix:** scope "dynamic is default" to **private IPs**; note public IPs (Standard) are Static.

### 09a-2 🟡 Public IP SKU framing omits Basic retirement / Standard v1-v2
- **Deck (Private vs public IP facet):** *"Public IPs come in a SKU (Basic/Standard)…"* / `['…SKU','Basic or Standard']`.
- **Status:** **Not false** — current docs still present "Standard (v1 or v2) or Basic" and the AZ-104-relevant rule ("public IP SKU must match the LB SKU") still holds. It just **omits** the 30-Sep-2025 Basic retirement and the Standard v1/v2 split.
- **Live docs:** https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses (2026-02-25).
- **Fix (optional):** add a one-line note that Basic is retired for new use and Standard is the default going forward.

> **Checked, NOT an error (rejected):** the facet-2 trap *"attach a Basic public IP to a Standard LB — does it work? No, SKUs must match."* The **rule is verbatim-correct** in current docs (*"matching SKUs are required… you can't mix basic and standard"*). Using a now-retired Basic IP as the *example* is context-stale at most. **Leave it.**

---

## Folder 09b — Connectivity

### 09b-1 🟠 "Default deny is priority 65000" (it's 65500 — and contradicts the deck's own diagram)
- **Deck (NSG facet 0 exam):** *"add an inbound allow rule… with a priority lower than **65000** (the default deny)."*
- **Correct:** **65000 is not the deny.** Default inbound rules: `AllowVNetInBound` **65000** (Allow), `AllowAzureLoadBalancerInBound` **65001** (Allow), `DenyAllInbound` **65500** (Deny). The deck's **own diagram** (line 154) shows this correctly — the exam text contradicts it.
- **`.md` proof:** `02-nsg__3-determine-network-security-groups-rules.md` describes the three default rules but not the numbers.
- **Live docs:** https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview — `DenyAllInbound | 65500 | Deny`.
- **Fix:** change "65000" → **"65500"** in the exam answer. (Custom rules use 100–4096, automatically higher-precedence than all defaults.)

### 09b-2 🟠 "No NSG attached = default rules apply (all traffic allowed except inbound defaults)"
- **Deck (NSG evaluation facet 1 gist):** the quoted line — self-contradictory and wrong.
- **Correct:** Default rules exist only **inside an NSG you create.** With **no NSG at a level**, traffic passes to the next level; if **neither** subnet **nor** NIC has an NSG, a VM with a **Standard public IP** (the only SKU since Basic retired) is **secure by default** → inbound internet **blocked**, while outbound and intra-VNet flow freely. So it's neither "default rules applying" nor "all traffic allowed." *(Note: the original auto-suggested fix "all traffic incl. inbound is allowed" is also wrong.)*
- **`.md` proof:** `02-nsg__4-determine-network-security-groups-effective-rules.md` — no-NSG "allows all traffic… according to the default Azure security rules."
- **Live docs:** https://learn.microsoft.com/en-us/azure/virtual-network/network-security-group-how-it-works (VM4 example): *"All network traffic is blocked through a subnet and network interface if they don't have a network security group associated… The VM with a standard public IP address is secure by default."*
- **Fix:** reword to: default rules live **inside** an NSG; with no NSG attached anywhere, a Standard-IP VM is inbound-blocked (secure by default), outbound/intra-VNet allowed.

---

## Folder 09c — DNS

### 09c-1 🔴 "Azure DNS doesn't support DNSSEC"
- **Deck (What-it-is gist + facts):** *"Azure DNS doesn't support DNSSEC"* / `['DNSSEC','not supported']`.
- **Correct:** Outdated. **Azure public DNS supports DNSSEC zone signing** (preview Nov 2024, **GA in 2025**) — signs zones, adds RRSIG/DNSKEY, needs a DS record in the parent. Nuance: **public** zones support DNSSEC; **private** DNS zones still don't.
- **`.md` proof (stale source):** `03-azure-dns__2-what-is-azure-dns.md:96` *"At this time, Azure DNS doesn't support Domain Name System Security Extensions…"*
- **Live docs:** https://learn.microsoft.com/en-us/azure/dns/dnssec (2025-01-27, upd 2025-10-09): zone signing for Azure public DNS zones.
- **Fix:** change to "**Public** zones support DNSSEC (zone signing); **private** zones don't." Update the red facts row.

### 09c-2 🟠 Alias record lists "Load Balancer" as a target type
- **Deck (Zone apex & alias — gist/facts/trap/exam/diagram):** *"Targets: public IP, **LB**, Traffic Mgr, Front Door, CDN."*
- **Correct:** Alias target **types** are: a **Standard public IP**, a Traffic Manager profile, an Azure CDN endpoint, an Azure Front Door endpoint, or another record set in the same zone. A **Load Balancer is not a selectable alias target** — you point the alias at the LB's **public IP** resource. (The deck's "In practice" scenario does say "public-IP load balancer," but the Targets list/gist don't.)
- **`.md` proof:** `03-azure-dns__5-resolve-name-alias-record.md:20-25` lists targets as Traffic Manager / CDN / **public IP resource** / Front Door — no LB; `:44` reaches a load balancer *via* its public IP.
- **Live docs:** https://learn.microsoft.com/en-us/azure/dns/dns-alias — capabilities list (Standard public IP, Traffic Manager, CDN, same-zone record set, Front Door); LB absent.
- **Fix:** replace "Load Balancer / LB" in the Targets list with "Standard public IP (incl. a load balancer's public IP)"; keep the apex→load-balanced-app concept.

---

## Folder 09d — Load Balancing  ✅ CLEAN
No factual errors found. L4 LB vs L7 App Gateway (WAF, path/host routing, SSL termination, cookie affinity), Front Door (global L7 + WAF + caching), Traffic Manager (DNS-based) all correct. *Note: thin coverage of Basic-vs-Standard LB SKU is a coverage observation, not a factual error.*

---

## Folder 10 — Network Watcher  ✅ CLEAN
No factual errors found. The three families (Monitoring/Diagnostic/Traffic), the 7 diagnostic tools, Connection monitor vs troubleshoot, flow logs → storage account, IaaS-only scope all correct. (Reference "gold standard" deck.)

## Folder 10 — VM Monitor  ✅ CLEAN
No factual errors found. Azure Monitor data model, metrics vs logs, VM Insights, Log Analytics/agent, diagnostic settings all correct.

## Folder 11 — Backup & Recovery  ✅ CLEAN
No factual errors found. Recovery Services vs Backup vault, MARS/MABS-DPM/VM-extension agents, "Azure Monitor Agent is not backup," policies, soft delete (14-day floor, configurable), incremental vs differential, Backup vs ASR all correct.

---

## Cross-cutting takeaways
1. **The raw `library/` is a stale mirror.** ~90% of confirmed errors are the deck copying an outdated MS Learn `.md` line. When refreshing, prefer live `learn.microsoft.com` over the frozen markdown for: Cloud Shell ephemeral sessions, RBAC role count, Premium page-blob redundancy, File Sync scale targets, Managed HSM FIPS level, DNSSEC, Basic public-IP retirement.
2. **Two earlier-pass claims were wrong and are now cleared:** App Service backup ("Basic+" is correct, not Standard+) and the 05 disk note (advisory only). The 65500/65000 and the 05 billing contradiction from that pass were confirmed real.
3. **Out of scope but noted for a future coverage pass:** ARM deployment modes + what-if (08), ACI restart policies (07), Basic-vs-Standard LB SKU (09d), and the thin **Monitor** domain (action groups, KQL/Log Analytics, alert rules, Site Recovery — also the user's self-identified weak area). These are *missing topics*, not *wrong facts*. (The ACI **GPU** item that was on this list turned out to be a real error — fixed, see 07-1.)

## Priority order for fixing
1. 🔴 **09c-1 DNSSEC** · **00-1 Cloud Shell storage** · **09c-2 alias LB target**
2. 🟠 **09b-1 65500** · **09b-2 no-NSG behavior** · **04a Premium page blobs ZRS** · **04c File Sync numbers (×2)** · **06 custom-domain Shared tier** · **09a-1 public-IP dynamic** · **00-2 Cloud Shell installs**
3. 🟡 **02 role count** · **04d FIPS 140-3** · **05 billing contradiction** · **00-3** · **09a-2** · **05 disk note**
