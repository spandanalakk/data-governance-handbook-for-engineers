# The Practical Data Governance Playbook — for Data Engineers Who Hate Bureaucracy

Turning governance from a blocker into a backbone. This repo is a hands-on **handbook** (not theory) with ready-to-run snippets for Databricks Unity Catalog, AWS Redshift/Lake Formation, and GCP BigQuery/Data Catalog — plus data quality, PII, and streaming governance.

> Tip: Each chapter is a standalone `.md` you can read or copy into your own wiki.

## Contents
- [01 — Why Governance Matters (Principles + Lineage)](01_why_governance_matters.md)
- [02 — Unity Catalog Setup](02_unity_catalog_setup.md)
- [03 — Redshift Policies (with Lake Formation)](03_redshift_policies.md)
- [04 — BigQuery + Data Catalog](04_bigquery_data_catalog.md)
- [05 — Data Quality Checks + Dashboard](05_data_quality_checks.md)
- [06 — PII: Encryption, Masking, Auditing](06_pii_encryption_audit.md)
- [07 — Streaming Governance (Kafka → Lake → Warehouse)](07_streaming_governance.md)

## Who is this for?
- Data engineers who ship pipelines and need **clear, enforceable** governance.
- Analytics engineers, BIEs, and platform teams building **secure, discoverable** data platforms.

## How to use
- Browse chapters in order or cherry-pick what you need.
- Copy/paste the **“Try This”** sections into your infra repos or notebooks.
- Use the **assets/** folder for diagrams or to drop architecture images.

---

### Badges (optional)
![Status](https://img.shields.io/badge/status-active-brightgreen)
![Focus](https://img.shields.io/badge/focus-hands--on-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

### Contributing
PRs welcome! Please keep examples **cloud-native** and vendor-accurate (Databricks, AWS, GCP).

