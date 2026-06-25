# IaC (ARM / Bicep) — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

- ✗→✓ **RG → Settings → Deployments is the home for ALL deployment info** — the exact JSON template (View template), plus each deployment's **timestamp**, status, duration, inputs/outputs, and events. Deployment *history* lives on the **RG**, not on the individual resource. *(Missed it as #11 on "view template"; scored #12 right after on "deployment date/time" — same blade.)*
