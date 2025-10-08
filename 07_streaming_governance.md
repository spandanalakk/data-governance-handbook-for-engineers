# 07 — Streaming Governance (Kafka → Lake → Warehouse)

Bring governance to **data in motion**.

## Schema Registry + Contracts
- Enforce Avro/Protobuf schemas with **compatibility rules**.
- Fail fast on incompatible changes; publish a **data contract** per topic.

```json
{
  "topic": "orders.v1",
  "owner": "supply_chain_team",
  "sla": "p95 < 5m end-to-end",
  "pii": false,
  "schema": "avro://schemas/orders_v1.avsc"
}
```

## Unity Catalog / Delta Live Tables
- Use **DLT expectations** to validate streams (nulls, ranges).
- Emit lineage automatically to Unity Catalog.

```sql
-- DLT expectation example (SQL)
CREATE LIVE TABLE orders_cleaned
TBLPROPERTIES ("quality" = "gold")
AS SELECT * FROM STREAM(LIVE.orders_raw)
WHERE order_id IS NOT NULL;
```

## Real-Time PII Strategy
- Avoid PII on hot streams when possible; tokenize at edge.
- If unavoidable, apply **field-level encryption** before publish.

## Auditable Pipelines
- Emit **OpenLineage** events from stream processors (Flink/Spark/Kafka Connect).
- Persist offsets/versions for reproducibility.

**Outcome:** Governed, debuggable streaming without killing latency.
