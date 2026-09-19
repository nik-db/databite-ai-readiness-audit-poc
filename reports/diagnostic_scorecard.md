# AI-Readiness Diagnostic Scorecard — insurance_claims.csv

| Category | Rating | Key Finding |
|---|---|---|
| Data Quality | 🔴 | property_damage: 36% missing, uniformly distributed across incident types — genuine, unexplained gap. police_report_available: 34.3% missing (same pattern likely — not yet tested). collision_type's 17.8% "missing" is fully explained by incident_type logic — false alarm, correctly ruled out. 1 isolated outlier (umbrella_limit). |
| Governance | 🔴 | No data dictionary; 1000/1000 rows uniquely re-identifiable via age+zip+occupation (proven) |
| Lineage | 🔴 | No upstream lineage beyond ingestion timestamp; source system/process unknown |
| Semantic Clarity | 🟡 | Domain-specific fields (policy_csl) undocumented; category sets undefined |
| AI/RAG-Readiness | 🟡 | Structured data requires deliberate flattening strategy for embedding (addressed in Step 5) |