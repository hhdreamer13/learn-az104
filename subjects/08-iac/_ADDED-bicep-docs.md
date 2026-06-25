# Grounding notes — Bicep (fetched from MS docs; the training module covers ARM JSON only)

The AZ-104 module in our library teaches **ARM JSON templates**. The exam objective also tests **Bicep** ("interpret/modify a Bicep file", "convert ARM↔Bicep"). Fetched from learn.microsoft.com to fill the gap.

## Bicep — docs summary
- A **domain-specific language** with **declarative** syntax for deploying Azure resources. A more concise, readable alternative to ARM JSON.
- **Transparent abstraction over ARM JSON**: at deploy time the Bicep CLI **transpiles** the `.bicep` file into an ARM JSON template. Anything valid in ARM JSON is valid in Bicep (same resource types/API versions/properties). No capability lost.
- **Concise syntax**: no `[...]` bracket expressions — call functions directly, read params/vars directly. Each resource gets a **symbolic name**.
- **Automatic dependencies**: reference a resource's symbolic name and Bicep infers `dependsOn` — you rarely write it by hand (unlike ARM JSON).
- **Modules**: break code into reusable parts (a module deploys a set of related resources). Cleaner reuse than ARM linked templates.
- **Flexible structure**: declare params/vars/outputs anywhere (ARM JSON forces them into fixed sections).
- **Idempotent + ARM-orchestrated**: same as ARM — deploy repeatedly → same state; Resource Manager orders/parallelizes interdependent resources.
- **No state files to manage**: Azure stores all state (key contrast with **Terraform**, which keeps a state file). Free + open source, Microsoft-supported.
- **what-if** operation: preview create/update/delete changes before deploying.
- **Convert both ways**: `bicep decompile` (ARM JSON → Bicep), `bicep build` (Bicep → ARM JSON). Bicep Playground shows both side by side.
- Deploy the same way as ARM: `az deployment group create --template-file main.bicep` / `New-AzResourceGroupDeployment -TemplateFile main.bicep`.

## ARM vs Bicep (the exam comparison)
- **ARM JSON** — verbose, manual `dependsOn`, bracket expressions, fixed sections, linked/nested templates for reuse. Still fully supported.
- **Bicep** — Microsoft's **recommended** IaC for Azure: concise, auto-dependencies, modules, same power (transpiles to ARM). Choose Bicep for new Azure-native IaC.
- Both are **declarative + idempotent**, both deploy via ARM, both manage no state file (vs Terraform).
- Deployment-stacks (a newer governance-of-deployments feature) is also an exam objective — manages a group of resources as a single deployed unit.
