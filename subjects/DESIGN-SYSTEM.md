# AZ-104 Subject HTML — Design System (the repeatable spec)

The frozen way we build every subject page. Reference implementations: `10-network-watcher.html` (the gold standard) and `00-tooling-cloud-shell.html`. When in doubt, copy the structure of the Network Watcher file.

---

## 1. What each page IS
A self-contained, single-file `.html` for one subject. One screen, no page scroll. A **stepped deck of independent facets** with a hand-drawn SVG diagram as the hero, a side panel that explains, and a hidden self-test. The user has visual memory and revisits to self-test — the page is a **calm reference that teaches**, not a lecture and not an exam pep-talk.

## 2. Layout (fixed — don't redesign)
- **Header**: kicker (`AZ-104 · <area>`) · big plain `h1` = just the subject name · one-line lede = a plain **definition of what the thing is**.
- **Tabs**: one per facet, click or ← →.
- **Body = two columns**: left = SVG diagram (the hero); right = side panel.
- **Side panel zones, in order**: facet title → **gist** (what it is) → **In practice — real examples** (the scenario rows) → **Where it lives** (only when there's placement confusion) → **Facts that change answers** → **Recall, then reveal** (on a faint tinted panel).
- **Footer**: counter + Prev/Next.

## 3. The engine (carry over verbatim)
- Responsive SVG: `fit()` measures the container and sets the viewBox to real pixels; everything draws inside `area()` (padded rect). **Never** a fixed viewBox — it clips. Re-`go()` on resize.
- `area()` MUST return `{x,y,w,h,cx,cy,r,b}` — include BOTH `cx:W/2` and `cy:H/2`. (A missing `cy` makes `a.cy` undefined → `NaN` coords → boxes render at the top-left over the title. This bit us once.)
- Helpers: `T()` text, `Twrap()` wrapping text (use it — labels overflow otherwise), `box()`, `arrow()`, `node()` (titled box).
- Color meanings are FIXED and consistent across all files:
  - green = the correct/safe/persistent answer · red = a trap / "does not / lost / blocked" · amber = a caution or a fork · blue = a neutral actor/tool · purple = placement ("where it lives").
- Light theme only. One clean sans everywhere; mono ONLY for literal commands/identifiers (`az`, `sudo`, `0.0.0.0/0`).

## 4. Structure of the CONTENT (the part that matters most)
- **Facets = independent, sensibly ordered, landable out of order.** No story arc, no payoff-reveal dependency. Each facet stands alone.
- **Step count matches real content.** Don't pad a thin subject (Cloud Shell = 4). Don't cram a rich one — if two things are tested differently, they may deserve separate FILES (Monitor split into VM-monitoring and Network-watcher).
- **Follow the source's own taxonomy.** If the MS docs group the subject (e.g. Network Watcher → Monitoring / Diagnostic / Traffic families), USE that grouping as the facet structure. Don't invent your own organizing frame and scatter things across it — that destroys the clean boundaries the user needs. The first facet is usually "the map" showing all the groups with every item in its box.
- **Order split files by DEPENDENCY, not by MS module number.** A cross-cutting/"on top of" topic must come AFTER the things it references — e.g. Storage order is accounts → blob → files → **security** (SAS/keys reference blobs and file shares, so security is last). MS Learn's own continuity is flawed (it cites App Service plans before App Service exists); don't inherit that. Ask "what must the reader already know for this to land?" and sequence accordingly. The `a-/b-/c-/d-` prefixes encode that order.
- Read the library `.md` files thoroughly before building. Build from them + knowledge.

## 5. VOICE — how we talk on the page
DO:
- **Define what the thing IS**, plainly, in the title/lede and each gist.
- **Explain each tool/concept**: what it is, what it does, how it behaves, with a **concrete real-world example** woven into the explanation (the user explicitly wants real-world examples).
- Calm, confident reference tone — like explaining to a capable colleague.

DO NOT (these are hard rules):
- **NEVER put our meta-conversation / strategy talk on the page.** Banned phrasings: "the exam never asks X, it asks Y", "master this fork and questions fall", "you stop guessing", "the exam loves to offer…", any fear-framing about the test. That's me talking ABOUT the subject; the page must TEACH the subject.
- No "this module covers…", no marketing ("enables organizations to…"), no portal click-throughs (unless a step IS the point), no copied MS sentences (compress/rewrite), no feature lists that don't change an answer, no screenshots.
- **Exam-specific framing lives ONLY in the hidden Trap/Exam reveal at the bottom** — never in the header, lede, titles, or gists.

## 6. The self-test element ("Recall, then reveal")
- Each facet ends with two hidden items: **Trap** (red chip) and **Exam** (amber chip). Question visible, answer hidden behind "Reveal answer".
- This is the only place exam-tactical content appears. It's for active recall: user reads the picture, recalls, then reveals to check.

## 7. The "In practice" rows (the heart of "which tool?" subjects)
- Format per row: a real symptom/situation (tagged **Example**) → the tool to use (tagged **Use**, tool name in green) → a short why.
- For any "which tool/option do I pick?" subject, these scenario→answer rows ARE the content. Lead with them; don't reduce them to terse labels.

## 8. The "Where it lives" panel (the placement cure)
- Purple panel, shown only when a subject has cross-domain/placement confusion (the user's core problem: "is action group Monitor or Policy?").
- States plainly: it lives HERE, and the "you also saw it in X because…" note. Solves placement via an explicit element, not a buried footnote.

---

## Build checklist (per subject)
1. Read all the subject's library `.md` files.
2. Find the source's own taxonomy → that's the facet skeleton; facet 0 = the map.
3. Write each facet: define it, explain it, add real-world example rows.
4. Add Where-it-lives only if there's placement confusion.
5. Facts that change answers (terse, color-coded).
6. Trap + Exam reveals (the ONLY exam framing).
7. Verify: JS parses, diagrams fit `area()` (no clipping), voice has zero meta/strategy talk, boundaries between facets are clean.
