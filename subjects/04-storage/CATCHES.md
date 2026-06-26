# Storage — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
(Covers all four storage sub-decks: a-storage-accounts, b-blob, c-files, d-security.)
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Tools & data movement
- ⚠ **AzCopy copies Blob and File only** — NOT Table or Queue. AzCopy is the file/object copy tool; Table (NoSQL) and Queue (messaging) aren't supported. *(Reasoned it from "AzCopy = file copy" — anchor: the two "file-like" services.)*

## Import/Export (ship physical disks)
- ⚠ **Import/Export drive prep needs TWO CSVs before the journal file: Dataset CSV + Driveset CSV.** Dataset = *what* to copy (files/folders list); Driveset = *which drives* (disk→drive-letter map). Anchor: **Data**set = data, **Drive**set = drives. Trap distractor: "WAImportExport file" — that's the *tool you run*, not a file you create. *(Obscure topic, unfamiliar — reasoned to it; just recognize the keywords.)*

## File Sync
- ✗ **File Sync NEVER overwrites — conflicts are kept as both copies.** Same filename differing on two endpoints → newest write keeps the original name, the older one is renamed with the endpoint appended (e.g. `tutorials-FileServer1.docx`). So any "file X will be overwritten by Y" statement is **False**. Also: a sync group is **one shared namespace** — every file syncs to *all* endpoints (so dojo.mp4 reaches FileServer1 too). *(Missed both overwrite statements.)*

- ✓ **You can't map a drive (File Explorer) to a Blob container — drive-mapping = Files/SMB only.** So "map a drive with SAS / account key" answers are wrong whenever the target is **Blob**. Also: **Import/Export = ship physical disks**, so it fails any "over the Internet" requirement. Internet + Blob upload via GUI → **Azure Storage Explorer**.

- ✗ **`azcopy make` = create a container/file share; `copy` = move data; `sync` = mirror.** "Copy a VM image into a *new* container" tests the **create** step → `make`, not `copy`. And the endpoint subdomain names the service: a **container lives in Blob → `https://<acct>.blob.core.windows.net/<container>`** (`file.core.windows.net` = a file share). Anchor: **make=create · copy=move · sync=mirror**, and **container ⇒ blob endpoint**.
  - **Root cause = misread, not knowledge:** "container" is overloaded — read it as an **Azure Container Instance (Docker)** when it meant a **storage blob container**. The AzCopy URL `.../tdimage` is always the blob container; any ACI mention is scenario noise. When you see "container" near AzCopy/storage URLs → **blob folder**, not compute.
- ⚠ **`azcopy copy [source] [destination] [flags]` — order = direction.** Local path first, URL second = **upload**; URL first = **download** (wrong way — a classic reversed-args trap). For folders **with subdirectories you MUST add `--recursive`** or only the top folder copies (the look-alike option without the flag is the decoy). `az storage blob copy start-batch` is blob→blob (container-to-container), not an on-prem upload. *(Don't know AzCopy by heart — carried by source→destination order + --recursive logic.)*

## Firewall / network access
- ✗ **Storage firewall ON → Azure services (Backup, Monitor…) are blocked too, unless "Allow trusted Microsoft services to access this storage account" is ticked.** It's a separate gate from the subnet list: **subnet must be in the allowed list** (for VMs) AND the **trusted-services checkbox must be on** (for Azure platform services). Both default to deny. So if that box is unchecked, **Azure Backup can NEVER back up the VM disks** to that account — even though Backup is "Microsoft's own." *(Got the VM-subnet one; guessed the trusted-services one.)*

## Replication / redundancy
- ✓ **"Single datacenter failure" + "synchronous" → ZRS** — 3 availability zones, synchronous, primary region only. GZRS/RA-GZRS *over-deliver* (add async geo-replication you weren't asked for) and async also violates "synchronous". Anchor: **LRS**=sync/1 datacenter · **ZRS**=sync/3 zones · **GRS/GZRS**=+async to a 2nd region. *(Also: ZRS needs GPv2 or premium BlockBlob/FileStorage — GPv1 & legacy BlobStorage can't do ZRS.)*
- ⚠ **Converting TO ZRS via "live migration" has TWO gates — kind AND current replication.** (1) **Kind must support ZRS**: GPv2, FileStorage, BlockBlobStorage only — **not GPv1, not legacy BlobStorage**. (2) **Live migration only starts from LRS or GRS** — an **RA-GRS** account must first be switched (manually) to LRS/GRS, which fails a "live migration only" requirement. So GPv2+LRS = direct; GPv1/BlobStorage = never; RA-GRS = needs a manual step first. Master rule: changing redundancy **within the primary region** (LRS↔ZRS) = manual/live migration; changing the **geo** part (add/remove GRS, RA-read access) = just a portal/CLI toggle. *(Correct but several stacked facts — hard.)*
