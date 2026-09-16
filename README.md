# insurance-finance-transformation-notes
Insurance Finance Transformation — Reference Notes
SAP Profitability and Performance Management (PaPM) & Financial Products Subledger (FPSL)
Conceptual reference model, based on 13+ years delivering IFRS17 / US GAAP / Local GAAP finance transformation projects for insurance and financial services clients.
> This repository contains generalized, non-client-specific notes on how insurance finance transformation projects are typically modeled in SAP PaPM and FPSL. It's meant to illustrate how these solutions are architected conceptually — not to reproduce any client implementation.
---
Why this exists
Most of my deepest technical work has lived inside client SAP systems and internal delivery documentation — none of it public by nature. This repo is a way to show, rather than just describe, how I think about structuring profitability and accounting logic for complex financial transformation projects.
---
1. The Core Problem
Insurance companies transitioning to IFRS17 (and reconciling with US GAAP / Local GAAP) face a common challenge:
Actuarial and operational data lives in disconnected source systems (policy admin, claims, reinsurance, general ledger).
Regulatory accounting standards require granular, auditable calculations at a contract or cohort level.
Finance teams need both the accounting output (subledger postings) and the ability to analyze profitability drivers behind it.
SAP addresses this with two complementary tools:
Tool	Purpose
SAP FPSL (Financial Products Subledger)	Accounting engine — generates IFRS17-compliant journal entries and subledger postings from raw contract/actuarial data
SAP PaPM (Profitability and Performance Management)	Modeling/calculation engine — flexible, rules-based calculation logic used to allocate costs, calculate profitability, and feed FPSL or reporting layers
---
2. A Simplified Reference Data Flow
```
Source Systems                Calculation Layer              Accounting / Reporting
───────────────               ──────────────────              ──────────────────────
Policy Admin  ─┐
Claims        ─┼──►  Data Integration  ──►  SAP PaPM  ──►  SAP FPSL  ──►  General Ledger
Reinsurance   ─┤        (staging,           (allocation,      (subledger
Actuarial     ─┘        validation)          modeling,         postings,
                                              scenario           IFRS17
                                              simulation)        reporting)
```
Typical stages in a PaPM model:
Data Sourcing — pull raw contract, premium, claims, and reinsurance data into staging tables
Cleansing & Validation — data quality checks, currency conversion, reference data mapping
Allocation Logic — distribute shared costs (acquisition costs, overheads) across contract groups using configurable drivers
Calculation Steps — apply IFRS17 measurement models (General Measurement Model, Premium Allocation Approach, Variable Fee Approach) depending on contract type
Output Structuring — format results for consumption by FPSL (accounting) or reporting tools (SAC, BW)
---
3. Why This Matters for Solution Consulting
Having built these models hands-on (not just configured them, but designed the underlying logic with actuaries, accountants, and IT architects) is what shapes how I approach presales and solution consulting work today:
Discovery — I know which questions actually uncover data-architecture blockers before they become implementation risk.
Demos & POCs — I can build believable, technically sound mock scenarios instead of generic canned demos.
Translating complexity — I've spent years explaining IFRS17 measurement models to non-technical stakeholders; that same skill now applies to explaining any complex SaaS product to a prospective customer.
---
4. Illustrative Example — Simplified Cost Allocation Rule (pseudocode)
A simplified illustration of the kind of allocation logic typically configured in a PaPM function (not an actual client rule):
```
FUNCTION allocate_acquisition_costs(contract_group):
    total_costs = get_period_costs(cost_center = "Acquisition")
    driver_basis = get_driver(contract_group, driver_type = "New Business Premium")

    FOR each contract IN contract_group:
        contract_share = contract.premium / driver_basis.total_premium
        allocated_cost = total_costs * contract_share
        POST allocated_cost TO contract.cost_account

    RETURN allocation_summary
```
This kind of driver-based allocation pattern repeats throughout PaPM models — cost allocation, risk adjustment calculation, contractual service margin (CSM) roll-forward — just with different drivers and measurement rules depending on the standard being applied.
---
About Me
Aleksandar Stojanović — SAP Finance & Insurance Transformation Consultant turned Solution Consultant (Presales), with 13+ years designing and delivering PaPM/FPSL solutions for IFRS17, US GAAP, and Local GAAP across international insurance and financial services clients.
LinkedIn
