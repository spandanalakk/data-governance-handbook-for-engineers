# Feature Comparison — Databricks, AWS, GCP

| Capability | Databricks (Unity Catalog) | AWS (Redshift + Lake Formation) | GCP (BigQuery + Data Catalog) |
|---|---|---|---|
| **Catalog / Metadata** | Unity Catalog (accounts-level), schemas, tables, functions | Glue Data Catalog + LF tags | Data Catalog, searchable entries |
| **Column Security** | Masking Policies | Column-level masking via UDF/views; LF-tag grants | Policy Tags (column-level IAM) |
| **Row Security** | Row Filter Policies | Views + session context / RLS patterns | Row Access Policies |
| **Lineage** | Built-in lineage (tables, columns) + DLT | AWS Glue/AF lineage limited; 3P/OpenLineage | BigQuery lineage + Audit logs |
| **Audit / Logs** | Account-level audit logs to cloud storage | CloudTrail + Redshift STL/SVL tables | Cloud Logging + Data Access logs |
| **Data Quality** | DLT expectations; integrates GE/Soda | Deequ/Soda on EMR; Lambda/Data Quality on Glue | GE/Soda; Dataform tests; DQ solutions |
| **Encryption** | KMS/CMEK on storage/UC | KMS across S3/Redshift | CMEK across GCS/BigQuery |
| **Cross-Account Sharing** | Unity Catalog sharing | Lake Formation data sharing | BigQuery Analytics Hub / BQ Omni |
| **CI/CD Hooks** | Databricks CLI/Repos + UC APIs | IaC via CDK/Terraform; Glue jobs | Terraform/Cloud Build; bq CLI |


