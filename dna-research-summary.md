# Fiserv DNA — Research Summary

**Last updated:** 2026-06-26
**Primary source:** [`thisisartium/fiserv-services-presales-knowledge-base`](https://github.com/thisisartium/fiserv-services-presales-knowledge-base) (presales scoping artifacts — transcripts, summaries, proposal)
**Scope:** Synthesis of everything the presales knowledge base says about the **DNA** core-banking conversion use case.

---

## What DNA is

**DNA is Fiserv's core banking / processing platform** — a **SQL/.NET application** used by **~220–460 financial institutions** (banks and credit unions). Fiserv's **DNA implementation team** is a *consulting* group (not product developers) that runs **conversions and onboardings**: when a new client comes onto DNA, the team builds out that client's entire product, fee, rate, and configuration database from scratch.

**Conversion vs. implementation** (a distinction Fiserv draws explicitly):
- **Conversion** (DNA) — data-centric: move and reconcile data from a source/donor core into DNA. Described as "a complicated beast."
- **Implementation** (CFC, XD) — configuration-centric: set up features and rules.

DNA is the harder, conversion-heavy product. It is one of several Fiserv cores; others named: **Premier**, COBOL-based cores (Jeff's domain), and **XP2 / ClearTouch / DataSafe** as source cores that data converts *from*.

---

## Core domain concepts

| Concept | What it is |
|---|---|
| **Sacred database** | Each client gets a *dedicated* DNA instance built off Fiserv's current release ("base"). For a conversion, a copy of the sacred DB is taken, then client data is converted into it. Products/fees/config must be built here **before the first data cut**. No version control — a single point of failure. |
| **MM list** (a.k.a. M&M sheet / MM report) | A **product-description report DNA already outputs** — lists *every* configurable value for a product (e.g., sweeps allowed? cycles? maintenance charges? interest posting?). Fiserv already has code to *extract* this from DNA. |
| **FI Configuration document** | A standard spreadsheet clients fill out, mapping their values to DNA's. **Column B = "what currently exists in DNA"**; clients add new values to be built. |
| **System & business tables** | The underlying DNA SQL tables where products/fees/config live. "Not a huge number of tables" — which is why reverse-loading is feasible. |
| **Cut** | A conversion run producing a populated target environment; typically 2–4 cuts + 1–3 mocks before the live event. |

---

## The problem

A consultant onboarding a new FI to DNA spends **~200 hours over 6–8 weeks** (+ 40–80 programming hours) manually:
1. **Mining** everything the FI offers — deposit/loan products, fees, rates, business rules — from websites, PDF fee schedules, disclosures, brochures.
2. **Categorizing** it into DNA's data model.
3. **Building** every structure in the sacred database before the first cut.
4. **Hand-coding** client-specific one-offs that don't map to standard fields.

The hard part is **the fine print** — exception details that are easy to miss but hit revenue/customers. Documented examples (mostly from Corey):
- Stop-payment fee **$25, but $10 for under-18 / over-65** (buried in an unrelated disclosure).
- First **8 non-network ATM transactions free**, then charged.
- **$3/month** on accounts inactive ≥ 1 year.
- **$5/month** e-statement fee for a bad email on file (client-specific "special," not native to DNA).

The reliable catchers of these have **pattern recognition built across hundreds of institutions** — the least-documented, most valuable asset, and the core tribal knowledge to capture.

---

## Proposed solution (presales)

A **two-phase agentic system** with mandatory human-in-the-loop:

- **Phase 1 — Requirements gathering.** Agent mines public sources, extracts/categorizes products/rates/rules into the DNA model, **with source citation + confidence per field**.
- **Phase 2 — Automated population.** Script the data into DNA's system/business tables. Key mechanism (Corey): **reverse-engineer the MM list** — since Fiserv already extracts the MM list *out of* DNA and it touches few tables, run that code in reverse to load data back *in*.

Supporting design points:
- **Two-bucket mapping:** split scraped data into (a) **native-to-DNA** items needing a verified direct map, and (b) **client-specific "specials"** needing custom implementation.
- **HITL is mandatory, not optional:** Fiserv **will not allow autonomous writes** to the sacred database — regulatory/audit chain-of-custody requires human sign-off on every change. Consultants review/approve/correct via an interface, with the **existing spreadsheet as a familiar fallback**.
- **Bonus source:** core **parameter printouts** (every core, including Premier, produces one) are another mineable input; the approach could extend to other cores.

---

## Key people

| Person | Role |
|---|---|
| **Corey (Simmons)** | Owns the **DNA implementations portfolio**; primary domain SME and source of exception/tribal knowledge. Eastern time. |
| **Sarabjeet** | Manages the **implementation engine**; owns the FI workbook (Excel) used as input. |
| **Jeff** | Oversees a separate **COBOL-based core** (Premier-adjacent); same process, different system. Central time. |
| **Andre** | Raised the key early **skepticism**: *why AI vs. established SQL scripting patterns?* — worth tracking as the value-justification question. |

---

## ⚠️ Critical context: DNA vs. the active workstream

There is a fork in the overall engagement that matters for anyone picking up DNA work:

- **DNA is the *original* presales target** — nearly all deep scoping content is about the DNA conversion use case.
- **Active delivery (as of the knowledge base) pivoted to XD** — the only weekly delivery artifacts ("Artbeats") are for the **XD** product (Client360/ServiceNow tickets, partial config + SQL generation). XD is "already staffed."
- **DNA is being spun up as a separate, newer workstream.** In the **2026-06-02 CFC call**, Fiserv asks Artium for dedicated resources for **CFC *and* DNA** as two new parallel products alongside XD. DNA scope = put the previously-discussed Corey/Andre conversion estimates "into an execution engine," expanded to the **full end-to-end conversion + configuration lifecycle**.

**Implication:** DNA *delivery* artifacts (code, architecture, live progress) did **not** exist in the presales knowledge base as of mid-June 2026 — DNA was at the staffing/SOW stage. This `Fiserv-DNA` repo is where that DNA-specific work is now being documented.

A pragmatic early-win suggestion from the proposal review (Corey): pivot the *first* DNA deliverable from product-configuration scraping to the **FI Configuration document → automated coding into the sacred database** — "a much straighter line" toward a quick investor-day statement.

---

## Source map (where to read more, by depth)

| Topic | File (in presales KB repo) |
|---|---|
| Domain model & problem | `06-20260408-fiserv-goals.md` |
| Verbatim DNA conversion discussion (sacred DB, MM list, fee exceptions) | `05-20260408-recording.txt` *(richest single source)* |
| Two-bucket mapping + reverse-engineering the MM list | `10-proposal-review.txt` |
| Andre's "why AI?" skepticism | `06-20260407-andre-summary.md` |
| DNA as a new staffed workstream | `05-20260602-CFC.txt` |
| Synthesized plan / suggested path | `03-suggested-path.md`, `03-addendum.md` |

## Related docs in this repo

- [`workflow-analysis.md`](./workflow-analysis.md) — end-to-end DNA migration workflow breakdown (4 phases, critical path, HITL candidates, tribal-knowledge flags) for the Workflow Extraction Workshop.
- [`miro-board-guide.md`](./miro-board-guide.md) — build guide for rendering that workflow on a Miro board.
