# 03 — Redshift Policies (with Lake Formation)

Implement guardrails using **AWS IAM + Lake Formation + Redshift**.

## Setup Outline
1. Catalog data in **AWS Glue** and manage access via **Lake Formation**.
2. Use **LF-Tags** for PII and domains; grant LF-tag-based access.
3. In Redshift, implement column masking and RLS.

## LF-Tag Example
```bash
# via AWS CLI (pseudo)
aws lakeformation add-lf-tags-to-resource --lf-tags "Key=PII,Value=Yes" --resource {...}
```

## Redshift Column Masking
```sql
-- Create a masking UDF (example)
CREATE OR REPLACE FUNCTION mask_email(email VARCHAR)
RETURNS VARCHAR IMMUTABLE AS $$
  SELECT CASE
    WHEN current_user IN (SELECT username FROM pii_readers) THEN email
    ELSE REGEXP_REPLACE(email, '^(.).+(@.+)$', '\\1***\\2')
  END;
$$ LANGUAGE SQL;

-- Apply via views
CREATE OR REPLACE VIEW v_customers AS
SELECT
  customer_id,
  mask_email(email) AS email,
  dob,
  city
FROM public.customers;
GRANT SELECT ON v_customers TO GROUP data_readers;
```

## Row-Level Security (Simplified)
```sql
-- Use session context or user mapping to filter rows
CREATE OR REPLACE VIEW v_customers_na AS
SELECT * FROM public.customers WHERE region = 'NA';
```

## Audit
- Use **CloudTrail** + **Redshift system tables** (`STL_QUERY`, `SVL_STATEMENTTEXT`) for usage trails.

**Outcome:** Least-privilege access that scales across accounts.
