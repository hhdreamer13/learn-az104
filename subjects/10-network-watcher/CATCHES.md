# Network Watcher — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Connection Monitor vs Connection Troubleshoot (time is the tell)
- ✗ **"Track average round-trip time (RTT) over time" → Connection Monitor** (continuous, ongoing). **Connection Troubleshoot = one-time** check right now, no RTT tracking. Discriminator = TIME: *track / monitor / over time / RTT / alert* → **Monitor**; *check once now / why did this fail* → **Troubleshoot**. (IP flow verify = NSG allow/deny; NSG flow logs = traffic logging — neither measures RTT.) *(Deck already has this exact pair — confused them under pressure, not a knowledge gap.)*

## Packet capture & the agent fork
- ✓ **Packet capture / IP flow verify on a VM → install the Network Watcher Agent VM extension first.** It's the on-VM piece that lets Network Watcher actually capture packets. Match the agent to the job (classic distractor trap): **Azure Monitor Agent** = OS/workload logs & metrics (not packets) · **Performance Diagnostics Agent** = boot/app/perf troubleshooting · **Defender for Servers** = security/threat detection. Also: **Traffic Analytics ≠ packet capture** — it analyzes **NSG flow logs** for traffic patterns (a red herring when the ask is packet capture).
