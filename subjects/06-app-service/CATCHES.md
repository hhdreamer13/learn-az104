# App Service — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
(Covers App Service plans + App Service app: deploy, slots, scaling, logging, security.)
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Plans & OS
- ✓ **"How many App Service plans?" = number of distinct OSes required, NOT number of apps.** A plan has **one OS, fixed at creation**; many apps share a plan, but only if same OS. **These are App Service's supported-runtime rules, not general language limits** (Python/PHP run on Windows elsewhere — but App Service only offers them on Linux). On App Service: **ASP.NET (Framework, e.g. v4.8) = Windows only**; **PHP / Python / Ruby = Linux only**; **Node / Java / .NET Core = either**. A mix that forces both → 1 Windows + 1 Linux = **two**. Traps: "five" (one-per-app — wrong, apps share) and "one" (forgets ASP.NET v4.8 can't run on App Service Linux).

## Deployment slots
- ✓ **Undo a bad swap = swap the SAME two slots again.** After a swap the old production app moved INTO the source (staging) slot — so re-swapping staging↔production restores last-known-good instantly, no backup needed. Trap: swapping a *third* slot (e.g. dev) instead pushes that slot's untested config live — rollback must use the exact two slots originally swapped.

## Logging (the confusable fork)
- ✗ **Raw HTTP request data (method, URI, client IP/port, user agent, response code) → Web Server Logging.** Match the *data kind* to the *log type*: **Web Server Logging** = the access log of every request (who hit what, from where, what status). **Application Logging** = logs *your app code* writes (traces/exceptions) — for debugging app logic. "HTTP 500 = server error" baits you toward Application Logging, but they asked for *request metadata*, which lives in the **web server** log. Decoys: security playbook = Sentinel; resource health alerts = availability notices (neither is logging). Anchor: **request metadata → Web Server Logging · app's own traces → Application Logging.** *(Missed — picked Application Logging.)*
  - Full App Service log set: **Web Server Logging** (HTTP access) · **Application Logging** (app traces) · **Detailed Error Messages** (full HTML error page for 4xx/5xx) · **Failed Request Tracing** (per-request IIS-pipeline trace).
