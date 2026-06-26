# IaC (ARM / Bicep) — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

- ✗→✓ **RG → Settings → Deployments is the home for ALL deployment info** — the exact JSON template (View template), plus each deployment's **timestamp**, status, duration, inputs/outputs, and events. Deployment *history* lives on the **RG**, not on the individual resource. *(Missed it as #11 on "view template"; scored #12 right after on "deployment date/time" — same blade.)*

## Custom deployment from an exported template
- ✓ **Deploying from an exported/captured ARM template → you can only set Subscription, Resource Group, Location.** Everything describing the resource (OS, VM size, availability options, disks) is **baked into the template JSON** and fixed; the custom-deployment form only picks *where* it lands. To change resource config you'd edit the template/parameters, not the form. Anchor: **template = what to build (locked) · deployment form = where to put it (sub/RG/region).**

## Secrets in templates
- ✓ **ARM template + "password must NOT be in plain text" → Azure Key Vault.** Don't put the secret in the template/parameter file — **reference the Key Vault secret by its resource ID** in the parameter file; the value is pulled at deploy time, never exposed. Need a vault + an **access policy** granting the deployment read access. Distractor traps (all "protection/password" names): **Entra Password Protection** = bans weak passwords (quality, not storage); **storage data protection** = blob soft-delete/versioning (can't store secrets); **label protection** = AIP document/email labels. Anchor: **store/retrieve a secret (password/key/cert) = Key Vault.**
