# Grounding notes — Container Apps + ACI (fetched from MS docs, not in the AZ-104 training modules)

The AZ-104 training module in our library covers **ACI only**. The exam objective also lists **Azure Container Apps** (and ACR, AKS). Container Apps is newer (GA 2022) — that's why the module predates it. Fetched from learn.microsoft.com on the project date to fill the gap. Use these as source facts.

## Azure Container Instances (ACI) — docs summary
- The **fastest, simplest** way to run a container in Azure — no VMs, no orchestrator. Starts in seconds.
- Linux or Windows containers; images from Docker Hub / ACR / other registries.
- **Container group** = the deploy unit: one or more containers sharing host, local network, storage, lifecycle (multi-container groups are **Linux only**). Like a Kubernetes pod.
- **Custom sizes**: specify exact CPU cores + memory; **billed per second**.
- **GPU**: yes — Linux containers can use NVIDIA GPU resources (**preview**). (So GPU is NOT a Container-Apps-only feature.)
- **No built-in autoscale.** ACI is a fixed container group you size manually. For more/orchestrated: **NGroups** (manage many groups, rolling upgrades, multi-AZ) or **virtual nodes on AKS** (run pods as ACI). Standby pools for faster start.
- Networking: public IP + FQDN (`label.region.azurecontainer.io`), or deploy into a **VNet** (needs a **NAT gateway** for outbound). IP can change on restart → use App Gateway for a static IP.
- **Availability zones**: zonal (pin a container group to one self-selected zone).
- Managed identity supported (incl. ACR image pull). Confidential containers + Spot (up to ~70% off) SKUs. Hypervisor-level isolation (VM-grade).
- Image size limit 15 GB. No privileged operations.
- **Use ACI when**: simple, short-lived, or burst tasks; single container or a small container group; batch/build jobs; you don't need autoscaling or orchestration.

## Azure Container Apps — docs summary
- A **serverless container platform** built on **Kubernetes + KEDA (autoscaling) + Dapr (microservices)** — but Microsoft manages the cluster; you never see Kubernetes.
- **Autoscales on any KEDA trigger**: HTTP traffic, event-driven (queues etc.), CPU or memory load. **Scale to zero** is supported — EXCEPT apps scaling on CPU/memory can't scale to zero.
- **Revisions**: run multiple versions, **traffic splitting** for blue/green & A/B.
- Built-in **HTTPS/TCP ingress**, **internal ingress + DNS service discovery**, secrets management, run on-demand/scheduled/event **jobs**, can host Azure Functions.
- Runs containers from any registry (Docker Hub, ACR). Optional custom **VNet**. Logs to **Log Analytics**. Specialized hardware (incl. GPU) available via workload profiles.
- **Use Container Apps when**: microservices, event-driven or HTTP apps that need **autoscaling (incl. to zero)**, blue/green deploys — without managing Kubernetes (AKS).

## The real distinction (the part the user was confused about — it's NOT GPU)
- **ACI** = single serverless container group, **no autoscale**, manual sizing. Simplest, for one-off / burst / batch.
- **Container Apps** = serverless **microservices** platform, **KEDA autoscale incl. scale-to-zero**, revisions/traffic-split. For apps that grow and shrink with load.
- **AKS** = full managed Kubernetes — you manage the cluster/orchestration; for complex container orchestration at scale (gap topic, lightly tested at AZ-104).
- **ACR** (Azure Container Registry) = the private registry that stores the images all of the above pull from (a storage/CI concern, not a runtime).

## Where this also feeds: the VM vs Container vs App Service comparison
- These three are the AZ-104 compute hosting models. A parent-folder comparison HTML should contrast: control vs management overhead, scaling model, and "pick when." (See CONTEXT build-notes.)
