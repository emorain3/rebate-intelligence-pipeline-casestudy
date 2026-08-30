# Auto Shop Revenue Recovery Case Study

A data engineering case study built in five days as part of a technical skills assessment. The problem: a regional network of independent collision repair shops was processing rebate data manually — a slow, error-prone process that made it nearly impossible to catch missed payments or flag suspicious activity. This project is my proposed solution, extended onto a modern analytics engineering stack.

📄 **[View the full case study →](https://emorain3.github.io/rebate-intelligence-pipeline-casestudy/)**

> **Privacy Note:** All data in this repository is fully synthesized. The original dataset was anonymized and resampled to protect the privacy of the source organization and any identifying business information. Findings such as silent shop counts reflect the resampled data and may differ from figures referenced in the original case study presentation.

---

## What's in Here

**Phase 1 — Original Pipeline**
- Power BI dashboard — three pages surfacing silent shops, anomaly flags, and an executive summary
- Python/Pandas Medallion pipeline (Bronze → Silver → Gold)
- Case study site — a walkthrough of the business problem, engineering decisions, and what I learned

**Phase 2 — Modern Stack Rebuild**
- dbt Core transformation layer on Snowflake — staging, intermediate, and mart models
- 25 passing dbt tests, including referential integrity and a custom singular regression baseline
- Live lineage graph hosted on GitHub Pages
- Streamlit dashboard querying Gold marts at runtime

---

## Stack

| Phase | Tools |
|---|---|
| Phase 1 — Original Pipeline | Python · Pandas · Microsoft Fabric · Power BI |
| Phase 2 — Modern Stack Rebuild | Snowflake · dbt Core · Streamlit |

---

## Live Links

| Artifact | URL |
|---|---|
| Case Study Site | https://emorain3.github.io/rebate-intelligence-pipeline-casestudy/ |
| dbt Lineage Graph | https://emorain3.github.io/rebate-intelligence-pipeline/ |
| Streamlit Dashboard | https://rebate-intelligence-pipeline-dashboard.streamlit.app/ |

---

## Key Engineering Decisions

**Composite Key over Single-Column Primary Key**
Transaction IDs are not unique — a single transaction can generate multiple rebate entries through rebate decomposition. The composite key `(transaction_id + partner_id + memo + net_amount)` treats each rebate event as its own grain, preventing valid revenue from being silently removed.

**Exception Typing over Binary Flagging**
Rather than a simple is_error boolean, each anomalous row is classified by exception type (`zero_amount`, `null_amount`, `negative_amount`, `null_memo`, `null_partner`). This separates data quality issues from inactivity signals — a missing memo is not the same business problem as a zero-dollar transaction.

**Silent Shop Classification at the Aggregate Layer**
Silent shop status is assigned at the Gold mart level, not the row level. An affiliate is only classified as a silent shop when their entire transaction history contains zero clean, valid financial activity. This prevents cosmetic data quality flags from being misread as inactivity.

**Fabric vs. Snowflake + dbt**
The original pipeline used Microsoft Fabric — appropriate for a BI-first, small-team environment with Power BI as the single output. The Phase 2 rebuild uses Snowflake + dbt to demonstrate the composable, data-platform-first pattern appropriate for multi-team environments where transformation logic requires version control, testing, and lineage as first-class engineering concerns.

---

## About Me

I'm a Cloud Data Engineer based in Metro Atlanta. I work across Azure and GCP with a focus on building data systems that are reliable, auditable, and actually useful to the people depending on them. Huge Microsoft Fabric Fan btw. I know. I know.

[LinkedIn](https://linkedin.com/in/ecclesiamorain) · [hire.ecclesia@outlook.com](mailto:hire.ecclesia@outlook.com)
