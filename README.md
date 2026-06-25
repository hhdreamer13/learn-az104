# AZ-104 Learning Platform

A personal, build-it-as-you-go study platform for the **AZ-104 (Microsoft Azure Administrator)** exam — built around how I actually learn: **visual, big-picture first, exam-focused**. The goal is to fix *placement* ("where does this live — Monitor? Policy? Entra?"), not to re-explain why each service exists.

## Start here

**Open [`index.html`](index.html)** in a browser. It's a visual map of every subject, grouped by exam domain (with scored weights), and it launches each deck. No build step, no server — just open the file.

## What's inside

| Path | What it is |
|---|---|
| **[`index.html`](index.html)** | The hub — visual subject map + launcher. Your home base. |
| **[`subjects/`](subjects/)** | **21 self-contained HTML decks** across 13 subject folders — one per subject. Each is a stepped deck of independent facets: hand-drawn SVG diagram + side-panel explanation + hidden self-test. |
| **[`library/`](library/)** | Raw Microsoft Learn markdown (225 units) — the untouched source the decks were built from, kept as provenance. *Note: it's a frozen ~2024 mirror; prefer live docs when refreshing.* |
| **[`subjects/DESIGN-SYSTEM.md`](subjects/DESIGN-SYSTEM.md)** | The frozen spec every deck follows (layout, voice, the SVG engine). Read before building any new page. |

## How a deck works

Each deck is a **stepped board of facets** you can land on out of order:
- **Tabs / ← →** move between facets.
- Left = a diagram (the hero); right = the explanation (what it is → real-world examples → "where it lives" → facts that change answers).
- Each facet ends with a hidden **Trap / Exam** reveal for active recall — the *only* place exam-tactical framing appears.

Color meanings are consistent everywhere: **green** = correct/safe · **red** = trap/blocked · **amber** = a fork/caution · **blue** = a neutral tool · **purple** = placement ("where it lives").

## Project docs

- **[`CONTEXT.md`](CONTEXT.md)** — the why, the approach, current status, and what's next.
- **[`STRUCTURE.md`](STRUCTURE.md)** — AZ-104 exam domains ↔ learning paths ↔ where the raw files live.

## Status

All 21 decks are built and fact-checked. Next up: hard Q&A cards mined from real practice-exam questions, then per-domain flashcard/review pages. See [`CONTEXT.md`](CONTEXT.md) for the live plan.

---

*Personal study project. Content is compressed/rewritten from Microsoft Learn and verified against current `learn.microsoft.com` docs, but always confirm against official docs for exam-critical facts.*
