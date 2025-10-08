# 02 — Unity Catalog Setup (Databricks)

End-to-end steps to set up **Unity Catalog** with permissions, masking, and audit hooks.

## Prereqs
- Metastore configured; workspaces attached.
- Service principals for CI/CD.
- External locations + storage credentials for delta tables.

## Create Catalogs, Schemas, and Tables
```sql
-- As metastore admin
CREATE CATALOG gov_demo COMMENT 'Governed demo catalog';
USE CATALOG gov_demo;
CREATE SCHEMA raw;
CREATE SCHEMA curated;

CREATE TABLE curated.customers (
  customer_id STRING,
  email STRING,
  dob DATE,
  city STRING
) USING DELTA;
```

## Grant Access (RBAC)
```sql
GRANT USAGE ON CATALOG gov_demo TO role data_readers;
GRANT USAGE ON SCHEMA curated TO role data_readers;
GRANT SELECT ON TABLE curated.customers TO role data_readers;
```

## Masking Policies
```sql
-- Databricks SQL (Unity Catalog)
CREATE MASKING POLICY mask_email AS (val STRING) RETURNS STRING ->
  CASE
    WHEN is_account_group_member('pii_readers') THEN val
    ELSE regexp_replace(val, '(^.).*(@.*$)', '\\1***\\2')
  END;

ALTER TABLE curated.customers
  ALTER COLUMN email SET MASKING POLICY mask_email;
```

## Row Filters (Row-Level Security)
```sql
CREATE FUNCTION is_region_member(region STRING) RETURNS BOOLEAN
RETURN is_account_group_member(CONCAT('region_', region));

CREATE ROW FILTER POLICY region_filter
AS (city STRING) -> is_region_member(city);

ALTER TABLE curated.customers
  SET ROW FILTER region_filter ON (city);
```

## Audit (System Tables & Events)
- Enable **audit log delivery** to cloud storage.
- Track grants, query history, object changes.

**Outcome:** Centralized governance without blocking developer velocity.
