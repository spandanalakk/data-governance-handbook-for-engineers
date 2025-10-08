# Diagrams (GitHub-safe Mermaid)

```mermaid
flowchart LR
  K[Kafka Topics] --> DLT[Delta Live Tables]
  DLT --> DL[Delta Lake - Bronze]
  DL --> CL[Curated Delta - Silver and Gold]
  CL --> WH[Warehouse]
  WH --> BI[Dashboards]

  %% Keep "Governance" simple; avoid line breaks and special characters
  subgraph Governance
    UC[Unity Catalog - RBAC, Policies, Lineage]
    DQ[Quality Checks]
    AUD[Audit Logs]
  end

  DL --> UC
  CL --> DQ
  WH --> AUD
