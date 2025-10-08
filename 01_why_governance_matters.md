# 01 — Why Governance Matters (Principles + Lineage)

Most data engineers meet governance only when something breaks: missing columns, PII leakage, or dashboard mismatches.
This chapter reframes governance as an **engineering practice** built on four pillars and implemented through **lineage**.

## The 4 Pillars (Engineer’s Cut)
1. **Data Quality** — Contracts + tests prevent schema/logic drift.
2. **Security & Privacy** — Roles, masking, encryption (KMS/CMEK), and least privilege.
3. **Discoverability** — Catalogs, ownership, tags, search.
4. **Observability & Audit** — Who touched what, when; reproducible lineage.

## Lineage from Kafka → Lake → Warehouse → Dashboard
**Goal:** Trace column-level provenance to speed debugging and improve trust.

**Minimal lineage checklist**
- Every dataset has: *owner*, *domain*, *purpose tag*, *PII tag (yes/no + type)*, *SLAs*.
- Jobs emit **OpenLineage** events (or vendor-native lineage in Unity Catalog/DLT, BigQuery, Redshift).

### Try This: Emit OpenLineage from Airflow
```python
# airflow_settings.py
from openlineage.airflow import DAG
# Configure OpenLineage backend via env:
# OPENLINEAGE_URL, OPENLINEAGE_API_KEY
```

### Try This: Column-level tags (pseudo)
```sql
-- Example taxonomy (apply via your catalog tool)
-- table: analytics.customers
-- columns:
--   email:  tags = ['PII', 'CONTACT']
--   dob:    tags = ['PII', 'SENSITIVE']
--   city:   tags = ['PUBLIC']
```

**Outcome:** Faster incident resolution, safer reuse, clearer ownership.
