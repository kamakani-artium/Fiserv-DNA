# Miro Board Build Guide: Fiserv DNA Migration — Workflow Extraction Workshop

---

## Board Overview

**Total canvas size:** Set Miro canvas to approximately 12,000 × 6,000 px. Use Miro's infinite canvas with frames to organize sections. All coordinates below assume the top-left origin (0, 0) and use frame-relative positioning.

**Four top-level frames:**

| Frame | Label | Canvas Position (approx.) |
|---|---|---|
| A | Color Legend | Top-left: (100, 100), size 800 × 600 |
| B | Ride Service Example | Top-right: (9,000, 100), size 2,800 × 1,800 |
| C | Working Area (blank) | Center: (2,000, 900), size 6,500 × 4,800 |
| D | Parking Lot | Bottom-right: (9,000, 2,200), size 2,800 × 1,400 |

---

## 1. Color Legend

Place Frame A at the top-left of the canvas. Use a Miro sticky-note or shape key.

| Element Type | Hex Color | Miro Shape | Notes |
|---|---|---|---|
| Standard Step | `#4A90D9` | Rectangle card (rounded corners) | Default for all process steps |
| Termination / End State | `#D0021B` | Rectangle card (sharp corners) | Steps 47, 48, 51 |
| HITL Candidate | `#F5A623` | Rectangle card (rounded corners) | Human decision or sign-off required |
| Tribal Knowledge Flag | `#9B59B6` | Small diamond sticky | Attach alongside step card as a flag tag |
| Delay Marker | `#E67E22` | Horizontal parallelogram | Attach alongside step card as a flag tag |
| System / Integration Touchpoint | `#1ABC9C` | Hexagon | Standalone system nodes in margins |
| Loop / Cycle Marker | `#2ECC71` | Curved arrow annotation | Dashed green arrow with label on arc midpoint |
| Parallel Track Swimlane | `#BDC3C7` | Rectangle frame (no fill, dashed border) | Wraps parallel track groups |
| Branch / Decision Point | `#F39C12` | Diamond shape | Go/No-Go and Driver Declined points |
| Actor / SME Label | `#ECF0F1` (white bg) | Text tag below card | Use abbreviations per table below |

**Actor Abbreviation Tags (attach as small text labels below cards):**

| Actor | Tag |
|---|---|
| FI Project Manager | `FIPM` |
| FI Back-Office & SMEs | `FI-SME` |
| FI Frontline Staff | `FI-Front` |
| Fiserv Consulting Team | `FSV-Consult` |
| Fiserv Data Team | `FSV-Data` |
| External End-Users | `Ext-Users` |
| 3rd Party APIs & BUs | `3P-API` |

**System Hexagons (teal `#1ABC9C`) — place in margins near first reference:**

| System | First Referenced At |
|---|---|
| `Legacy Donor Core` | Step 2 |
| `Sacred Database` | Step 10 |
| `Conversion Engine & Repo` | Step 23 |
| `XD/CFC Portals` | Step 15 |
| `DNA Target Environment` | Step 25 |
| `Validation Manager` | Step 25 |

---

## 2. Board Layout

### Spatial Map

**Frame A — Color Legend (top-left):** Reference only. Does not connect to other frames.

**Frame B — Ride Service Example (top-right):** Self-contained worked example. Surround with dashed yellow border labeled `"WORKED EXAMPLE — READ BEFORE MAPPING."` Flows left to right within frame.

**Frame C — Working Area (center, dominant):** Holds all four phases. Internal layout:

- **Flow direction:** Left to right across the frame, four vertical phase columns.

| Phase | Column x-range (relative to frame) | Label |
|---|---|---|
| Phase 1 | 0–1,500 px | Pre-Implementation & Discovery |
| Phase 2 | 1,600–3,400 px | Execution: Reactive Build & Parallel Workflows |
| Phase 3 | 3,500–5,200 px | Validation, Testing & Rework |
| Phase 4 | 5,300–6,500 px | Go-Live Cutover & Rollback |

- **Phase boundary boxes:** Large rounded-rectangle frame per column, light gray fill (`#F4F6F7`), bold phase label at top-center. No fill on interior — border only.

- **Phase 2 parallel swimlanes:** Inside the Phase 2 column, split vertical space into two horizontal lanes:
  - Upper lane (y: 800–1,800 px): **Track A — Data Team**
  - Lower lane (y: 1,900–2,900 px): **Track B — Consulting Team**
  - Each lane: dashed `#BDC3C7` rectangle border, italic label on left edge.
  - Tracks converge at step 22, centered below both lanes at y: ~3,100 px.

- **Phase 3 parallel sub-lane (steps 31–32):** Small dashed box within Phase 3 wrapping steps 31 and 32 side by side, labeled `"Parallel Balancing"`. Both merge into step 33.

- **Loop arrows:** Curved dashed green arrows (`#2ECC71`, 3 px stroke) that exit the loop-end step, arc above or below the loop group, and point back to the loop-start step. Label at arc midpoint. See Loop Summary below.

- **Go/No-Go Branch (Phase 4):** Diamond decision shape after step 44.
  - Arrow labeled `"GO →"` points right to Option A (steps 45–48).
  - Arrow labeled `"ROLLBACK ↓"` points downward to Option B (steps 49–51).

**Frame D — Parking Lot (bottom-right):** Header sticky `"PARKING LOT"` in bold 24 pt yellow. Pre-place 12–16 blank yellow stickies in a 4×4 grid. Instruction text: `"Drop anything that can't be resolved in the session here — assign an owner before you leave."`

---

## 3. Card List — Phase by Phase

**Card dimensions:** 200 × 80 px rounded rectangles. Horizontal spacing: 40 px. Arrows: 2 px solid `#2C3E50`.

**Flag tags:** Small stickies (60 × 30 px) at top-right corner of parent card.

---

### Phase 1 — Pre-Implementation & Discovery

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 1 | Contract Signed & Kicked Off | `#F5A623` | `FIPM` | HITL | → 2 |
| 2 | Deconversion Files Delivered | `#4A90D9` | `FSV-Data` | TRIBAL KNOWLEDGE | → 3 |
| 3 | DGF Delivered to FI | `#F5A623` | `FIPM` | HITL | → 4 |
| 4 | DGF Circulated to Domain Experts | `#4A90D9` | `FI-SME` | LOOP START | → 5 |
| 5 | Legacy Rules Struggled Internally | `#4A90D9` | `FI-SME` | TRIBAL KNOWLEDGE, DELAY | → 6 |
| 6 | Partial Requirements to FIPM | `#F5A623` | `FIPM` | HITL | → 7 |
| 7 | Queries to Fiserv Consulting | `#4A90D9` | `FSV-Consult` | DELAY | → 8 |
| 8 | Guidance Returned to FIPM | `#4A90D9` | `FIPM` | TRIBAL KNOWLEDGE, LOOP END | → 4 (loop), → 9 (exit) |
| 9 | DNA Products Provisioned | `#4A90D9` | `FSV-Consult` | TRIBAL KNOWLEDGE | → 10 |
| 10 | Configs Stored in Sacred Database | `#4A90D9` | `FSV-Consult` | — | → 11 |

**Loop arrow (steps 4–8):** Arc above the row from step 8 back to step 4.
Label: `"~18 months — repeat until DGF requirements complete"`

---

### Phase 2 — Execution: Reactive Build & Parallel Workflows

**Track A — Data Team (upper swimlane)**

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 11 | Historical Layouts Queried | `#4A90D9` | `FSV-Data` | TRIBAL KNOWLEDGE | → 12 |
| 12 | Starter ETL Code Delivered | `#4A90D9` | `FSV-Data` | — | → 13 |
| 13 | Legacy Data Profiled & Cleansed | `#F5A623` | `FSV-Data` | HITL, TRIBAL KNOWLEDGE | → 14 |
| 14 | ETL Code Written & Adjusted | `#4A90D9` | `FSV-Data` | — | → 22 |

**Track B — Consulting Team (lower swimlane)**

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 15 | Partial XD/CFC System Built | `#4A90D9` | `FSV-Consult` | LOOP START | → 16 |
| 16 | FI Notified of Partial Build | `#4A90D9` | `FIPM` | — | → 17 |
| 17 | FIPM Reviews Configurations | `#4A90D9` | `FIPM` | — | → 18 |
| 18 | Change Requests Delivered | `#F5A623` | `FIPM` | HITL, LOOP END | → 15 (loop), → 19 (exit) |
| 19 | Third-Party Connectivity Requested | `#4A90D9` | `3P-API` | DELAY | → 20 |
| 20 | External Setup Completed | `#4A90D9` | `3P-API` | — | → 21 |
| 21 | Connectivity Configs Delivered | `#4A90D9` | `3P-API` | — | → 22 |

**Loop arrow (steps 15–18):** Arc below the swimlane from step 18 back to step 15.
Label: `"Repeat until FI approves configuration"`

**Convergence**

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 22 | Sacred DB Configs Extracted & Ported | `#4A90D9` | `FSV-Consult` | — | → 23 |
| 23 | Conversion Execution Triggered | `#4A90D9` | `FSV-Data` | — | → 24 |
| 24 | First Conversion Cut Executed | `#4A90D9` | `FSV-Data` | — | → 25 |

Place a `"CONVERGENCE"` text annotation between the two swimlanes and steps 22–24. Draw arrows from step 14 and step 21 both merging into step 22.

---

### Phase 3 — Validation, Testing & Rework

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 25 | Validation Manager Ported | `#4A90D9` | `FSV-Data` | LOOP START | → 26 |
| 26 | Validation Manager Triggered | `#4A90D9` | `FSV-Data` | — | → 27 |
| 27 | Thousands of Validation Scripts Run | `#4A90D9` | `FSV-Data` | — | → 28 |
| 28 | Hashing & GL Reconciliation Run | `#4A90D9` | `FSV-Data` | — | → 29 |
| 29 | Automated Balancing Reports Delivered | `#4A90D9` | `FSV-Data` | — | → 30 |
| 30 | Cut Readiness Notification Sent | `#4A90D9` | `FIPM` | DELAY | → 31 & 32 |

**Parallel Balancing sub-box (steps 31–32) — dashed box, two side-by-side lanes**

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 31 | Fiserv Manual Balancing (8+ hrs) | `#F5A623` | `FSV-Consult` | HITL | → 33 |
| 32 | FI Manual Balancing (4–6 hrs) | `#F5A623` | `FI-SME` | HITL | → 33 |

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 33 | Frontline Staff Log Into Target | `#4A90D9` | `FI-Front` | — | → 34 |
| 34 | Stare & Compare Validation | `#F5A623` | `FI-Front` | HITL, TRIBAL KNOWLEDGE | → 35 |
| 35 | Day-in-Life & Fast-Forward Txns | `#4A90D9` | `FI-Front` | — | → 36 |
| 36 | Defect Logs to FIPM | `#4A90D9` | `FIPM` | — | → 37 |
| 37 | Defect Logs to Data Team | `#4A90D9` | `FSV-Data` | — | → 38 |
| 38 | ETL Mapping Code Adjusted | `#4A90D9` | `FSV-Data` | — | → 39 |
| 39 | Configs Re-copied & Conversion Re-run | `#4A90D9` | `FSV-Data` | LOOP END | → 25 (loop), → 40 (exit) |

**Loop arrow (steps 25–39):** Arc above the row from step 39 back to step 25.
Label: `"~3 iterations — repeat until 99.8% accuracy"`

---

### Phase 4 — Go-Live Cutover & Rollback

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 40 | UAT Sign-off Delivered | `#F5A623` | `FIPM` | HITL | → 41 |
| 41 | CDC Dual-Write Initiated | `#4A90D9` | `FSV-Data` | — | → 42 |
| 42 | T-0 Transaction Freeze Initiated | `#F5A623` | `FIPM` | HITL | → 43 |
| 43 | Final Delta Migration Extracted | `#4A90D9` | `FSV-Data` | — | → 44 |
| 44 | Post-Migration GL Reconciliation | `#4A90D9` | `FSV-Data` | — | → Decision Diamond |

**Decision Diamond:** Yellow fill `#F39C12`, 200 × 120 px, label `"Go / No-Go Decision"`.
- Arrow `"GO →"` → right to step 45
- Arrow `"ROLLBACK ↓"` → down to step 49

**Option A — Go-Live**

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 45 | API Gateway Reconfigured | `#F5A623` | `FSV-Consult` | HITL | → 46 |
| 46 | Active Integrations Confirmed | `#4A90D9` | `3P-API` | — | → 47 |
| 47 | Live Access Delivered; Legacy Decommissioned | `#D0021B` | `FSV-Consult`, `FIPM` | HITL, TERMINATION | → 48 |
| 48 | End Users Begin Live Access | `#D0021B` | `Ext-Users` | TERMINATION | — (end) |

**Option B — Rollback** (flows downward from decision diamond)

| Step | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| 49 | Hard Rollback Triggered | `#F5A623` | `FIPM` | HITL | → 50 |
| 50 | Blue-Green Switch Executed | `#4A90D9` | `FSV-Data` | — | → 51 |
| 51 | Failure Report & Recovery Initiated | `#D0021B` | `FIPM` | TERMINATION | — (end) |

Place steps 47, 48, and 51 with double-line borders in addition to the red fill to visually reinforce termination.

---

## 4. Structural Elements

### Phase Boundary Boxes
Four large rounded-rectangle containers spanning the full vertical height of the working area. No fill, `#BDC3C7` dashed 2 px border, bold phase label at top-center in 20 pt.

### Swimlane Separators (Phase 2)
Two horizontal dashed rectangles inside the Phase 2 container:
- Upper: italic label `"Track A — Data Team"` on left edge
- Lower: italic label `"Track B — Consulting Team"` on left edge
- Text annotation `"CONVERGENCE"` below both, above steps 22–24

### Parallel Balancing Sub-lane (Phase 3)
Small dashed box wrapping steps 31–32 side by side, labeled `"Parallel Balancing"`. Both arrows merge into step 33.

### Loop Arrows — Summary

| Loop | From → To | Label |
|---|---|---|
| Phase 1 DGF loop | Step 8 → Step 4 | `"~18 months — repeat until DGF complete"` |
| Phase 2 XD/CFC loop | Step 18 → Step 15 | `"Repeat until FI approves config"` |
| Phase 3 validation loop | Step 39 → Step 25 | `"~3 iterations — repeat until 99.8% accuracy"` |

All loop arrows: dashed, `#2ECC71` green, 3 px stroke, curved Miro connector, label at arc midpoint.

### System Hexagons (teal `#1ABC9C`)
Place in margins near first reference step. Connect with thin dotted lines to relevant cards.

---

## 5. Example Corner — Ride Service Pre-Seeded Diagram

**Location:** Frame B (top-right). Flow left to right. Bold header: `"WORKED EXAMPLE — Ride Service Workflow"`. Subtitle: `"Reference card types, loops, branches, and parallel paths before you begin mapping."` Dashed yellow frame border.

### Card Specifications

| Card | Card Label | Color | Actor Tag | Flags | Connects To |
|---|---|---|---|---|---|
| R1 | Car Ordered by Rider | `#4A90D9` | `Rider` | — | → R2 |
| R2 | Driver Matched | `#4A90D9` | `Platform` | BRANCH | → R3, → R-B1 |
| R-B1 | Driver Declined | `#4A90D9` | `Driver` | — | → R-B2 |
| R-B2 | Return to Driver Available | `#4A90D9` | `Platform` | LOOP | → R2 (loop) |
| R3 | Driver Arrived | `#4A90D9` | `Driver` | — | → R4A, → R4B |
| R4A | Driver Navigating to Destination | `#4A90D9` | `Driver` | PARALLEL | → R5 |
| R4B | Rider Tracking Driver Live | `#4A90D9` | `Rider` | PARALLEL | → R5 |
| R5 | Ride Completed | `#4A90D9` | `Platform` | — | → R6, → R-H1 |
| R6 | Payment Processed | `#4A90D9` | `Platform` | TERMINATION | — (end) |
| R-H1 | Dispute Raised by Rider | `#F5A623` | `Rider` | HITL | → R-H2 |
| R-H2 | Human Agent Reviews Dispute | `#F5A623` | `Support Agent` | HITL | → R-H3 |
| R-H3 | Resolution Delivered | `#4A90D9` | `Support Agent` | TERMINATION | — (end) |

### Structural Elements Within the Example Frame

- **Decision diamond at R2:** Yellow `#F39C12`, labeled `"Driver accepts?"`. Arrow `"Yes →"` to R3; arrow `"No ↓"` to R-B1.
- **Loop arrow (R-B2 → R2):** Green dashed curved arrow arcing back left to R2. Label: `"Retry — find next available driver"`.
- **Parallel swimlane box (R4A / R4B):** Small dashed box, R4A upper lane, R4B lower lane, labeled `"Parallel Tracking"`. Fork from R3 into both; merge from both into R5.
- **HITL extension:** Dotted elbow arrow from R5 downward to R-H1, labeled `"If dispute →"`. Sits below the main flow line as an exception path.
- **Termination caps:** Red square endcaps on final arrows of R6 and R-H3.

**Card layout:**
```
[R1] → [R2] → [R3] → [R4A] ──→ [R5] → [R6] ■
         ↓      ↑     [R4B] ──→   ↓
        [R-B1]→[R-B2]─┘          [R-H1] → [R-H2] → [R-H3] ■
```

---

## Builder Checklist

Before handing the board to participants, confirm:

- [ ] All four frames are labeled and visible at 50% zoom
- [ ] Color legend is readable at full zoom and matches all card colors on the board
- [ ] All 51 step cards are placed with correct colors, actor tags, and flag tags
- [ ] All three loop arrows are green, dashed, labeled, and arc back to their loop-start card
- [ ] Phase 2 has two labeled swimlanes with a convergence row below
- [ ] Phase 3 parallel balancing sub-box wraps steps 31–32 only and merges at step 33
- [ ] Go/No-Go diamond has two labeled outgoing arrows; Option B track flows downward
- [ ] Steps 47, 48, and 51 use red termination color with double-line or stop markers
- [ ] All system hexagons are placed in margins with dotted connector lines
- [ ] Parking lot frame has 12+ blank yellow stickies and instruction text
- [ ] Ride service example is in top-right frame with dashed yellow border and subtitle
- [ ] Ride service example includes: branch (declined), parallel lanes, loop (retry), and HITL extension (dispute)
- [ ] All frames are locked before participants begin so they cannot be accidentally moved
