# Fiserv DNA Core Banking Migration — Workflow Breakdown

**For use in:** Workflow Extraction Workshop — Facilitator Reference Document

---

## Overview

This workflow describes the end-to-end process of migrating a financial institution (FI) from a legacy donor core banking system to Fiserv's DNA core platform. The process spans roughly 18–24 months across four phases: discovery and configuration, parallel build and data engineering, iterative validation, and final go-live cutover with rollback protocols. The critical challenge throughout is reconciling the FI's undocumented legacy business rules with DNA's highly flexible configuration model — a gap that generates most of the delay and rework in the process.

---

## Phase 1: Pre-Implementation & Discovery

**Summary:** The FI and Fiserv formally engage, and the legacy system's data is handed over for analysis. The FI receives DNA's configuration guide (DGF) and must translate 18+ months of legacy business rules into DNA setup decisions — a process that is iterative, slow, and heavily dependent on internal subject matter experts who rarely agree. Fiserv then manually provisions initial product configurations in the DNA environment.

1. Contract Signed and Project Kicked Off `[HITL CANDIDATE]`
2. Deconversion Files Delivered — Legacy core sends flat files (EBCDIC, packed decimals) to Data Team `[TRIBAL KNOWLEDGE]`
3. DGF Delivered to FI — Fiserv presents DNA's configuration options `[HITL CANDIDATE]`
4. DGF Circulated to Domain Experts `[LOOP START]`
5. Legacy Rules Struggled Over Internally — back-office staff map legacy setup to DNA options `[TRIBAL KNOWLEDGE]` `[DELAY]`
6. Partial Requirements Returned to FIPM `[HITL CANDIDATE]`
7. Queries Forwarded to Fiserv Consulting `[DELAY]`
8. Guidance Returned to FIPM `[LOOP END → returns to step 4 for ~18 months]` `[TRIBAL KNOWLEDGE]`
9. DNA Products Manually Provisioned — "click, click, copy" through the DNA UI `[TRIBAL KNOWLEDGE]`
10. Configurations Stored in "Sacred Database"

---

## Phase 2: Execution — Reactive Build & Parallel Workflows

**Summary:** Two major workstreams run in parallel: the Data Team engineers the ETL pipeline to move legacy data into DNA's schema, while the Consulting Team builds out the customer-facing UI and third-party API integrations. These tracks converge when generating the first database "cut."

`[PARALLEL — Track A and Track B run simultaneously]`

**Track A — DNA Core Data Engineering**

11. Historical Layouts Queried `[TRIBAL KNOWLEDGE]`
12. Starter ETL Code Delivered
13. Legacy Data Profiled and Cleansed `[HITL CANDIDATE]` `[TRIBAL KNOWLEDGE]`
14. ETL Code Written and Adjusted

**Track B — CFC/XD Product & API Integration**

15. Partial XD/CFC System Built `[LOOP START]`
16. FI Notified of Partial Build
17. FIPM Reviews Configurations
18. Change Requests Delivered `[HITL CANDIDATE]` `[LOOP END → returns to step 15]`
19. Third-Party Connectivity Requested `[DELAY]`
20. External Setup Completed
21. Connectivity Configurations Delivered

**Convergence**

22. Sacred Database Configs Extracted and Ported
23. Conversion Execution Triggered
24. First Conversion Cut Executed

---

## Phase 3: Validation, Testing & Rework

**Summary:** Each "Cut" is subjected to automated validation, cryptographic reconciliation, and manual human review. Frontline staff perform hands-on comparison testing. Defects are logged, ETL code is corrected, and the Cut is regenerated. Repeats ~3 times until data accuracy reaches 99.8%.

`[LOOP START — Mock Conversion Cycle, ~3 iterations]`

25. Validation Manager Ported to Target Environment
26. Validation Manager Triggered against new Cut
27. Thousands of Validation Scripts Executed
28. Cryptographic Hashing and GL Reconciliation Run
29. Automated Balancing Reports Delivered
30. Cut Readiness Notification Sent to FI `[DELAY — 1-week window]`

`[PARALLEL — Manual Reconciliation]`

31. Fiserv Manual Balancing Performed (8+ hours) `[HITL CANDIDATE]`
32. FI Manual Balancing Performed (4–6 hours) `[HITL CANDIDATE]`

`[END PARALLEL]`

33. Frontline Staff Log Into Target Environment
34. "Stare & Compare" Validation Performed `[HITL CANDIDATE]` `[TRIBAL KNOWLEDGE]`
35. Day-in-the-Life and Fast-Forward Transactions Executed
36. Defect Logs Delivered to FIPM
37. Defect Logs Forwarded to Data Team
38. ETL Mapping Code Adjusted
39. Sacred Database Configs Re-copied and Conversion Re-executed `[LOOP END → returns to step 25]`

---

## Phase 4: Go-Live Cutover & Rollback Protocols

**Summary:** Following UAT sign-off, the legacy core continues running while DNA receives live change data. A transaction freeze is triggered, the final delta migration executes, and a post-migration GL reconciliation determines the Go/No-Go. If clean, traffic routes to DNA and the legacy core is decommissioned. If thresholds are breached, a hard rollback occurs.

40. UAT Sign-off Delivered `[HITL CANDIDATE]`
41. CDC Dual-Write Initiated
42. T-0 Transaction Freeze Initiated `[HITL CANDIDATE]`
43. Final Delta Migration Extracted
44. Post-Migration GL Reconciliation Executed

`[BRANCH — Rollback Trigger Evaluation (Go/No-Go)]`

**Option A — Mathematical Match & API Stability (Go-Live)**

45. API Gateway Reconfigured (Strangler Fig pattern) `[HITL CANDIDATE]`
46. Active Integrations Confirmed
47. Live System Access Delivered and Legacy Core Decommissioned `[HITL CANDIDATE]` `[TERMINATION]`
48. End Users Begin Live Access `[TERMINATION]`

**Option B — Threshold Breached** *(data error >0.2%, API error >1%, or CPU >90%)*

49. Hard Rollback Triggered `[HITL CANDIDATE]`
50. Blue-Green Switch Executed
51. Failure Report Delivered and Recovery Protocol Initiated `[TERMINATION]`

---

## Critical Path

Minimum sequence that must complete for go-live:

1. Contract Signed and Project Kicked Off
2. Deconversion Files Delivered from Legacy Core
3. DGF Delivered to FI
4. DGF Requirements Finalized *(end of ~18-month loop)*
5. DNA Products Manually Provisioned and Sacred Database Created
6. Legacy Data Profiled and Cleansed
7. ETL Code Written
8. XD/CFC Build Accepted by FI *(end of reactive build loop)*
9. Third-Party Connectivity Established
10. Sacred Database Configs Ported to Target
11. First Conversion Cut Executed
12. Validation Manager Run and Balancing Reports Generated
13. Manual Reconciliation Completed (Fiserv + FI)
14. Frontline Staff Validation Completed and Defects Resolved
15. 99.8% Accuracy Threshold Achieved *(end of mock conversion loop, ~3 cycles)*
16. UAT Sign-off Delivered
17. CDC Dual-Write Initiated
18. T-0 Transaction Freeze and Final Delta Migration Executed
19. Post-Migration GL Reconciliation Passes at 100%
20. API Gateway Reconfigured *(Go-Live branch)*
21. Legacy Core Decommissioned and Live Access Delivered

---

## Key Systems

| System | Role |
|---|---|
| **Legacy Donor Core** | Source system; provides deconversion flat files; runs in parallel via CDC dual-write until decommissioned |
| **DNA Target Environment** | Fiserv's modern core banking platform; live system of record after cutover |
| **Conversion Engine & Repo** | Stores historical ETL code, executes conversion scripts, runs Validation Manager, handles rollback |
| **XD / CFC Portals** | Customer-facing digital banking interfaces; configured by Fiserv Consulting, reviewed by FI |
| **"Sacred Database"** | Client-specific DNA configuration set; built manually via DNA UI; no version control; single point of failure |
| **3rd Party APIs** | External systems (DMA, ATM switch, ISO 20022); introduce external scheduling dependencies |
| **Validation Manager** | Automated testing suite; runs thousands of scripts, performs cryptographic hashing and GL reconciliation |

---

## HITL Candidates Summary

| Step | Why Human Involvement Is Irreplaceable |
|---|---|
| Contract Signed | Legal and relationship decisions require authorized human signatories |
| DGF Requirements Finalization | DNA configuration options are business decisions — only the FI's SMEs can determine correct setup |
| Legacy Data Profiling & Cleansing | Decisions about merging duplicate records or discarding corrupt data require contextual judgment that propagates through every subsequent Cut |
| Reactive Build Review by FIPM | The FI must evaluate whether UI and product behavior matches real operational need — the vendor cannot make this call |
| Fiserv Manual Balancing (8+ hrs) | Automated reports flag discrepancies; deciding whether a discrepancy is acceptable or a blocker requires experienced analyst judgment |
| FI Manual Balancing (4–6 hrs) | The FI must independently verify their customer data was faithfully converted — a fiduciary responsibility |
| "Stare & Compare" Validation | Tellers and customer care staff are the only people who know what a correct legacy screen looks like in practice |
| UAT Sign-off | Contractual and regulatory milestone; cannot be automated |
| T-0 Transaction Freeze | Timing has direct customer impact; requires judgment on business hours, batch cycles, and risk tolerance |
| API Gateway Reconfiguration | Routing 100% of live traffic is point-of-no-return; requires a human Go/No-Go call |
| Legacy Core Decommission | Irreversible action requiring FI executive authorization |
| Hard Rollback Trigger | Even when thresholds are formally breached, a human must confirm given the business and reputational stakes |

---

## Tribal Knowledge Flags

| Location | What Knowledge Is at Risk | Risk If Lost |
|---|---|---|
| Legacy Deconversion File Formats | EBCDIC field layouts and field-level meaning known only by legacy system admins — some may have already left | ETL mapping errors that don't surface until late validation or never |
| Legacy Business Rules (DGF Loop) | Why the legacy system was configured as it was — fee logic, edge cases, special account types — exists only in long-tenured staff memory | DNA gets configured incorrectly; defects discovered late where fixes are far more expensive |
| DNA Configuration Guidance (Fiserv Consulting) | Which DNA options interact poorly and which choices cause downstream problems — delivered verbally, not captured in the DGF | Configuration errors requiring expensive rework; knowledge leaves with the consultant |
| Manual Product Provisioning | The "click, click, copy" sequence has no script, no version control, no audit trail | Silent misconfigurations in the Sacred Database difficult to detect or audit after the fact |
| Conversion Engine Starter Set Selection | Applicability of historical ETL code depends on undocumented similarity assessments by the selecting engineer | Wrong baseline code means more rework cycles; poor selection is invisible until validation fails |
| "Stare & Compare" Validation | Frontline staff validate by memory — they know what a correct legacy screen looks like because they used it daily | Defects missed in validation that only surface when real customers are affected post-go-live |
| Sacred Database Configuration History | What was changed, when, and why is tracked only through manual provisioning history — no version-controlled audit log | Corrupted or incorrectly ported Sacred Database invalidates an entire Cut; root cause analysis is extremely difficult |
