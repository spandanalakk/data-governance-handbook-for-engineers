# 06 — PII: Encryption, Masking, Auditing

Secure PII without killing usability.

## Encryption
- **At rest**: KMS/CMEK on S3/GCS/ADLS and warehouse storage.
- **In transit**: TLS everywhere; private endpoints if possible.

## Masking & Tokenization
- **Unity Catalog**: `CREATE MASKING POLICY` + apply per column.
- **Redshift**: UDF + secure views; **Lake Formation LF-Tags**.
- **BigQuery**: **Policy Tags** + DLP for detection.
- Tokenize high-risk fields (e.g., PCI/PHI) using deterministic keys when joins are needed.

## Auditing
- Enable **query logs** and **grant logs** to a central, immutable store.
- Alert on anomalous patterns (off-hours PII scans, table dumps).

### Example: “Approved Analyst” View
```sql
-- Principle: analysts see only what they must, with masked PII by default.
CREATE VIEW curated.customers_for_analytics AS
SELECT
  customer_id,
  CASE WHEN has_pii_access() THEN email ELSE mask(email) END AS email,
  city
FROM curated.customers;
```
