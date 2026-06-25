# AZ-104 Learning Platform — Project Context

## What this is
A personal, build-it-as-we-go learning platform for the AZ-104 (Azure Administrator) exam.
The point is to learn *my way* — visual, big-picture-first, exam-focused — and have fun doing it.
If it turns out good, it might become a real platform later. No pressure on that yet.

## Who I am
- ~2 years working with Azure. Not a newbie — I don't need the "why does X exist" padding.
- Already finished all the MS Learn modules (read passively to get through them).
- Currently ~70% on exam simulations.
- I have strong **visual memory**: I remember things when I see them in one good big picture, then zoom in.
- My core problem right now is **placement / mixing things up** — e.g. "I know I saw action groups somewhere, but was it Monitor, Entra, Policy?" It's not a details problem, it's a "where does this live" problem.

## What I want from notes
- Real things that change and have effect — not explanations of why a service exists.
- Equivalents and decision-making: e.g. for Load Balancer, I want "what are the alternatives and how to choose" — not "why load balancing matters."
- Exam-oriented: gotchas, traps, the things MS uses to make questions hard.

## Approach (discovered step by step — not locked)
1. **Raw data first.** Pull the raw MS Learn markdown so we own the source material from Microsoft itself. This is the foundation — we build on top of MS Learn, not from memory.
2. Then layer on top: compressed/visual notes, decision tables, exam gotchas, my weak points, practice questions.
3. Discover the structure as we go. No need to pre-decide scope, timing, or which areas first.

## Raw data source
MS Learn content lives in the `MicrosoftDocs/learn` GitHub repo. Mapping convention:
```
Learn URL:  .../modules/<module-slug>/<unit-slug>
Raw file:   https://raw.githubusercontent.com/MicrosoftDocs/learn/main/learn-pr/wwl-azure/<module-slug>/includes/<unit-slug>.md
```
- Unit 1 is usually `1-introduction.md`; last unit is usually a knowledge-check/summary.
- The raw files are long/padded — that's the bloat we're escaping. We keep them as **source material to compress from**, not as something to read directly.

## Library (raw MS Learn markdown — built)
Lives in `library/`, organized by **exam domain** → module → unit `.md`.
- 28 modules, 225 unit files, ~1.3M. Each module folder also has `_module.yml` (MS metadata).
- See `STRUCTURE.md` for the domain↔path mapping and the exam-coverage gaps.

How it was pulled (repeat this to refresh):
- The MS Learn content repo is `github.com/MicrosoftDocs/learn`. Modules are NOT all under one folder — they're scattered across `learn-pr/{azure, wwl-azure, wwl-sci}/`. Guessing the `wwl-azure` URL convention misses ~half. Always locate the real path via the repo tree first.
- Method that avoids GitHub API rate limits entirely: `git clone --filter=blob:none --no-checkout --depth 1`, then `git sparse-checkout set <real module paths>`, then `git checkout`. This uses the git CDN, zero `api.github.com` calls. (Rate limits only hit the API, not raw git/CDN.)
- Substitution note: the identity path's "Azure Policy initiatives" module (`sovereignty-policy-initiatives`) is NOT in this repo — used `enforce-governance-azure-policy-resource-locks` instead (covers Policy + resource locks, better for the exam anyway).

## Subjects (our mental model — flat, no mixing)
`subjects/` = 11 clean single-subject folders + 1 tooling folder. We dropped the 5-domain grouping as STRUCTURE (it lumped Entra+RBAC+Policy and Monitor+Backup — the cause of the mixing). Domains kept only as a weight tag. See `subjects/README.md` for the full map and the audit-driven placements. Raw `library/` stays as untouched provenance.

Key fixes baked into placement: backup pulled out of compute → backup-recovery; azure-architecture split (mgmt-infra → governance, AZ-900 padding parked in `_skip-az900/`); Network Watcher → monitor; App Service plans+app merged; SSPR folded into entra-id.

## Status
- [x] Context file created
- [x] Researched + verified AZ-104 structure (STRUCTURE.md)
- [x] Pulled raw MS Learn markdown library (library/, MS-mirror provenance)
- [x] Audited library vs exam objectives (drove the subjects/ placement; one-time, log removed)
- [x] Re-placed into flat 11-subject mental model (subjects/)
- [x] Designed + froze the artifact design system (see subjects/DESIGN-SYSTEM.md)
- [x] **Built ALL subject decks — 21 HTML files, every AZ-104 domain covered.** Map below.
- [x] **Built `index.html` hub** (project root) — visual subject map grouped by exam domain (weights + color dots), one card per deck, "where did I see this?" placement strip. Launcher style (opens decks in same tab). Domain dot color == card color (verified).
- [x] **Fact-accuracy audit of all 21 decks** (one-time; log removed after fixing). Per-deck fact-check vs the deck's own MS Learn `.md` sources + live `learn.microsoft.com`, with adversarial verification. **All confirmed errors FIXED in the HTML** (17 total). Key lesson to remember: ~90% of errors were the deck copying an OUTDATED `.md` line — the raw `library/` is a frozen ~2024 mirror, so **prefer live docs when refreshing any fact.**
- [ ] **NEXT (in progress): Idea 2 → Idea 1.** (Idea 2 first, it feeds Idea 1.)
  - **Idea 2 — hard Q&A from real sims.** User is taking Tutorials Dojo sim exams (already done 2 MS sims earlier). Will bring back the questions that *caught* them (wrong/guessed/trap) with the right answer, the tempting wrong answer, and the catch. We turn each into a per-subject "hard Q&A with the catch" card. **Blocked on user supplying the questions** (paste or screenshot — they live in the sim tool, not the repo).
  - **Idea 1 — per-DOMAIN flashcard/review HTML** (5 pages: Identity+Gov, Storage, Compute, Networking, Monitor). Per-domain ON PURPOSE — sits ABOVE the subject decks to fix cross-subject *placement* confusion (user's core problem). Contents: rapid flip-card defs, comparison tables (the forks the exam loves), tricky/confusable pairs, "catch" cards. Build AFTER Idea 2 so the real sim catches populate the "tricky" sections. Reuses subjects/DESIGN-SYSTEM.md.
- [ ] Parked (decide later): cross-cutting compute-comparison page (VM vs Container vs App Service) + CLI-vs-PowerShell command reference — both detailed in build-notes below.

## Built library — 21 decks (all complete)
Each deck = one self-contained HTML following subjects/DESIGN-SYSTEM.md. Decks ordered by dependency where split.
- **00** tooling-cloud-shell
- **01 entra-id** (×3): a-entra-fundamentals · b-manage-identities · c-sspr
- **02** rbac
- **03** governance
- **04 storage** (×4, dependency order): a-storage-accounts · b-blob · c-files · d-security
- **05** vms (create→equip→fragility→protect→scale→automate→cost arc, 8 facets)
- **06** app-service (plan-first→app)
- **07** containers (ACI + Container Apps + AKS + ACR; fetched Container Apps docs for the gap) — *07\* = clean except the ACI-GPU fix: GPU was RETIRED from ACI (14 Jul 2025); Container Apps HAS serverless GPUs (T4/A100, GA). Deck's old "both support GPU" was backwards, now corrected.*
- **08** iac (ARM + Bicep; fetched Bicep docs for the gap)
- **09 networking** (×4, dependency order): a-vnets · b-connectivity (NSG/ASG/peering/routes) · c-dns · d-load-balancing (LB L4 vs App Gw L7 vs Front Door vs Traffic Manager)
- **10** network-watcher · vm-monitor (the two halves of Monitor, split)
- **11** backup-recovery

Gap-filling docs fetched & saved: 07-containers/_ADDED-container-apps-and-aci-docs.md, 08-iac/_ADDED-bicep-docs.md.

## Artifact design — FROZEN. Full spec: `subjects/DESIGN-SYSTEM.md` (read it before building any page)
Each subject = one self-contained .html: stepped deck of INDEPENDENT facets, hand-drawn SVG diagram as hero, side panel (gist → real examples → where-it-lives → facts → hidden trap/exam recall), light theme, responsive `fit()/area()` engine. Reference files: `subjects/10-network-watcher.html` (best) and `00-tooling-cloud-shell.html`.

Hard-won lessons baked into the spec (do not relearn these):
1. **Design, don't transcribe.** First template was rejected for turning the user's throwaway words ("gotchas", "decision tables") into literal headers. Use design judgment to make the subject LAND.
2. **Deck of facets, not a story.** Independent, landable-out-of-order facets; no payoff-reveal arc. Step count matches real content (don't pad). If two things are tested differently → separate files (Monitor → vm-monitoring + network-watcher).
3. **Content = scenario→tool, not definitions.** The exam tests "symptom → which tool", so lead with real-world examples; never strip them to terse labels.
4. **Follow the source's OWN taxonomy.** Use the MS docs' grouping (e.g. Network Watcher → Monitoring/Diagnostic/Traffic families) as the facet skeleton; don't invent a frame and scatter tools across it (that muddies boundaries). Facet 0 = the map of all groups.
5. **VOICE: teach, never talk about the exam on the page.** BANNED on-page: "the exam never asks X", "master this fork", "you stop guessing", any fear-framing. Title/lede = plain DEFINITION of what it is. Each facet = what it is / does / real-world example, calm reference tone. Exam-specific framing lives ONLY in the hidden Trap/Exam reveal at the bottom.
6. **Clean theme:** light only, one sans (mono only for literal commands), no clipping (responsive viewBox), strong zone separation, [[light-theme-clean-design]].
7. **Order split files by dependency, not MS module number** (Storage: accounts→blob→files→security; security references blobs/shares so it's last). MS Learn's own continuity is flawed — don't inherit it.

## Build notes — specific subjects (don't lose these)
- **Compute — scale up vs out, per service** (user-observed exam trap; it's a cross-service "which can do what" question):
  - Single **VM** → up/down only (resize+reboot), manual.
  - **VMSS** → out/in, automated (autoscale) — IaaS horizontal-HA answer.
  - **App Service** → BOTH: scale UP = bigger *plan pricing tier*; scale OUT = more *instances* (manual, or autoscale on Standard+).
  - **ACI containers** → no elastic autoscale (single container group; CPU/mem = a kind of up). Elastic container scaling = **AKS / Container Apps** (gap topics).
  - → Put the "scale up vs out by service" comparison in the **App Service** file (contrast w/ VMs); make the **ACI-can't-autoscale** trap explicit in the **Containers** file. VM file already covers its half — leave as-is.
- **Compute comparison HTML (do at end of compute):** a parent-folder `subjects/` comparison page contrasting VM vs Container (ACI/Container Apps) vs App Service — control vs management overhead, scaling model, "pick when." User asked for it after containers. Containers file fetched real Container Apps + ACI docs (saved in 07-containers/_ADDED-container-apps-and-aci-docs.md) since Container Apps is newer (GA 2022) and not in the training module. NOTE corrected fact: GPU is NOT Container-Apps-only — ACI also supports GPU (Linux, preview). The real ACI-vs-Container-Apps distinction is SCALING: ACI has no autoscale; Container Apps autoscales (KEDA) incl. scale-to-zero.
- **CLI / PowerShell / commands coverage (user wants this — decide approach later):** the exam asks command-level questions, and the verbs collide confusingly. Capture the real distinctions:
  - **Create vs deploy a VM**: `az vm create` / `New-AzVM` = imperative, create ONE VM directly. `az deployment group create` / `New-AzResourceGroupDeployment` = deploy a TEMPLATE (ARM/Bicep) that may contain a VM + its resources, declaratively. "Create" = single imperative resource; "deployment" = template-based.
  - Watch lookalike PowerShell verbs: `New-AzVM` (the VM) vs `New-AzResourceGroupDeployment` (a template deploy) vs `New-AzResourceGroup` (just the RG). There is no `New-AzureCreate` — distractor-style fake cmdlets are a classic trap.
  - CLI noun-order trap already in IaC file: `az deployment group create` (correct) vs deprecated `az group deployment create`.
  - Likely useful command pairs across subjects: VM (`az vm create`/`New-AzVM`), RG (`az group create`/`New-AzResourceGroup`), deploy (`az deployment group create`/`New-AzResourceGroupDeployment`), storage, RBAC (`az role assignment create`), etc.
  - **Approach TBD**: either weave the key command into each subject's relevant facet, OR a small cross-cutting "commands / CLI vs PowerShell" reference page. Decide later. The Cloud Shell file (00) is the natural home for general CLI-vs-PowerShell framing.
- **05-vms is DONE** and intentionally excludes networking detail (→ 09-networking), ARM/CLI/PowerShell create options (→ 08-iac / cross-cutting). Those are deferred by placement, not dropped. RI / Azure Hybrid Benefit + VM extensions were judged minor and kept light.
- **11-backup-recovery**: cover MARS (files/folders/system-state) vs MABS/DPM (on-prem servers→vault) vs VM-backup-extension (whole Azure VM) vs Azure Monitor Agent (monitoring, NOT backup) as a 4-way agent trap. Also Recovery Services vault vs Backup vault; full vs incremental.
