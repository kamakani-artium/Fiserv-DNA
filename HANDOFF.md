# Session Handoff — Fiserv DNA Research

**Written:** 2026-06-26
**For:** A fresh Claude Code session resuming DNA work
**Owner:** kamakani-artium (kamakani@artium.ai)

---

## TL;DR

This repo (`kamakani-artium/Fiserv-DNA`) is where we document **individual work on the Fiserv DNA core-banking conversion** use case. This session researched what the Artium presales knowledge base says about DNA and captured the synthesis in [`dna-research-summary.md`](./dna-research-summary.md). Read that first, then this file for state and next steps.

---

## Repo contents

| File | Purpose |
|---|---|
| [`dna-research-summary.md`](./dna-research-summary.md) | **Start here.** Synthesis of the DNA use case from presales material — domain, problem, proposed solution, people, critical context. |
| [`workflow-analysis.md`](./workflow-analysis.md) | End-to-end DNA migration workflow (4 phases, critical path, HITL candidates, tribal-knowledge flags). Predates this session. |
| [`miro-board-guide.md`](./miro-board-guide.md) | Build guide to render the workflow on a Miro board. Predates this session. |
| `HANDOFF.md` | This file. |

---

## What was done this session

1. **Set up GitHub access** on this machine (SSH key `~/.ssh/id_ed25519`, ed25519, in macOS Keychain; git identity = `kamakani-artium` / `kamakani@artium.ai`). Auth to GitHub is verified and working.
2. **Analyzed the presales knowledge base** [`thisisartium/fiserv-services-presales-knowledge-base`](https://github.com/thisisartium/fiserv-services-presales-knowledge-base) — a content/knowledge repo (transcripts, summaries, proposals), not code.
3. **Deep-dived DNA specifically** — traced every DNA mention across transcripts/summaries/proposal and synthesized it into `dna-research-summary.md`.

## Key findings (one-paragraph version)

DNA is Fiserv's SQL/.NET core platform (~220–460 FIs). Onboarding a new FI is a ~200-hour manual job: mine the FI's products/fees/rates from public docs, map them into DNA's model, and build them in a per-client "sacred database" before data cuts. The proposed agentic solution is two-phase (Phase 1: extract/map from public sources with citations + confidence; Phase 2: auto-populate DNA by reverse-engineering the "MM list" report), with **mandatory human sign-off** (no autonomous DB writes, for audit reasons). **Critical context:** DNA is the *original* presales target but active delivery pivoted to the XD product; DNA is being spun up as a *separate, newly-staffed* workstream as of the 2026-06-02 CFC call — so this repo is the home for that new DNA-specific work. See `dna-research-summary.md` for detail and a source map.

---

## State of the work

- **Research phase: complete** for the presales material. Findings captured in `dna-research-summary.md`.
- **No DNA delivery artifacts yet** (code, architecture, evals) — DNA was at staffing/SOW stage in the source material.
- **Source repo** was cloned to scratchpad for analysis; it is **not** committed here (it's a separate Artium repo).

## Open questions (carried from presales material)

- Which 3–5 completed onboarding spreadsheets will be the **baseline** for the agent?
- Which **consultants** does Corey want involved, and when can she introduce them? (This dependency shapes every build decision.)
- What does the work look like closer to the **program and check cycle**, and who owns that conversation?
- Andre's open challenge: **why AI vs. established SQL scripting patterns?** — needs a crisp value answer.

## Suggested next steps

- [ ] Decide first DNA deliverable. Proposal-review suggestion: start with **FI Configuration document → automated coding into the sacred database** (straighter line to a quick win) rather than full public-source scraping.
- [ ] If the source spreadsheets / MM-list samples become available, design the **Phase-1 extraction eval** (compare agent output to finished consultant spreadsheets; the fee-exception examples in the summary make good test cases).
- [ ] Reconcile `workflow-analysis.md` (a detailed migration workflow) against the presales findings — confirm where it came from and whether it reflects current DNA scope.
- [ ] Consider sketching the Phase-1/Phase-2 architecture for DNA as a standalone doc.

---

## How to resume in a fresh session

```bash
# Clone (SSH access already configured on kamakani's machine)
git clone git@github.com:kamakani-artium/Fiserv-DNA.git
cd Fiserv-DNA

# Read in this order:
#   1. dna-research-summary.md   (the findings)
#   2. HANDOFF.md                (this file — state + next steps)
#   3. workflow-analysis.md      (workflow detail)
```

The richest primary source for going deeper lives in the **presales** repo
`thisisartium/fiserv-services-presales-knowledge-base` — see the "Source map" table
at the bottom of `dna-research-summary.md` for which file covers what.
