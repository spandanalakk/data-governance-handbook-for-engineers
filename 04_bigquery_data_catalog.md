# 04 — BigQuery + Data Catalog

Governance with **BigQuery IAM**, **Policy Tags**, and **Data Catalog** search.

## Policy Tags for Column-Level Security
1. Create a taxonomy (e.g., `sensitive/pii/contact`, `sensitive/pii/financial`).
2. Attach policy tags to columns; grant `roles/bigquery.datapolicyUser` + IAM to groups.

```sql
-- Example table
CREATE OR REPLACE TABLE lake.curated.customers AS
SELECT * FROM lake.raw.customers;

-- Attach policy tags via UI or API (pseudo)
-- bq update --policy_tags '{"email":"projects/..../policyTags/pii_contact"}' lake.curated.customers
```

## Row-Level Security
```sql
CREATE OR REPLACE ROW ACCESS POLICY region_policy
ON lake.curated.customers
GRANT TO ("group:data_readers@company.com")
FILTER USING (region IN ('US','CA'));
```

## Audit & Search
- **Cloud Logging** for query access.
- **Data Catalog** for discovery (owners, tags, lineage links).

**Outcome:** Fine-grained controls with strong metadata/search UX.
