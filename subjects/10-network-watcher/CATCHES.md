# Network Watcher — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Packet capture & the agent fork
- ✓ **Packet capture / IP flow verify on a VM → install the Network Watcher Agent VM extension first.** It's the on-VM piece that lets Network Watcher actually capture packets. Match the agent to the job (classic distractor trap): **Azure Monitor Agent** = OS/workload logs & metrics (not packets) · **Performance Diagnostics Agent** = boot/app/perf troubleshooting · **Defender for Servers** = security/threat detection. Also: **Traffic Analytics ≠ packet capture** — it analyzes **NSG flow logs** for traffic patterns (a red herring when the ask is packet capture).
