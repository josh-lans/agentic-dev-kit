# Case Study — MonLite

*An enterprise observability platform, built solo with the Conductor method.*
*(Capability-level overview — architecture and outcomes, no proprietary code.)*

## The problem
A global managed-services SAP estate relied on a commercial monitoring product with a **seven-figure** price tag and real capability gaps. The goal: a platform that covers more, fits the environment better, and removes the licensing dependency.

## What was built
A full-stack monitoring and observability platform spanning **SAP applications, six database engines, hosts, and web services**, with real-time alerting, role-based access, and an embedded AI diagnostic assistant.

**Architecture (3 tiers):**
- **Control plane** — a Python/FastAPI service (modular routes + background workers) on PostgreSQL, with advisory-lock leader election delivering ~5-second automatic HA failover.
- **Distributed collectors** — lightweight Python agents that gather telemetry and push it back to the control plane.
- **Frontend** — a React 19 single-page app for dashboards, alarms, thresholds, and visualizations.

**Notable capabilities:**
- **Deep SAP monitoring** via both SAPControl (SOAP) and ABAP RFC, across 20 surveillance categories, plus SAP Cloud Connector and Cloud ALM.
- **Six database connectors** (HANA, Oracle, SQL Server, DB2, Sybase ASE, MaxDB) with per-engine thresholds and backup tracking — no extra licensing required.
- **"Luna," an AI diagnostic assistant** that runs fully air-gapped on a local LLM (so telemetry never leaves the firewall), with optional cloud fallback, a RAG pipeline, 100+ diagnostic tools, live database querying, and anti-hallucination validation.
- **Enterprise hardening** — SSO (Okta / Microsoft Entra ID), team-scoped RBAC, encrypted credential vault, HMAC-signed audit logs; thousands of automated tests and a multi-workflow CI/security pipeline.

## Outcome
**Scale-tested to handle 3,000+ systems and 200+ concurrent users** (sub-300ms p95 and a 0.07% error rate under mixed load in a multi-phase test campaign). Presented as a business case and live platform to global leadership.

## How the method showed up
This was the proving ground for the Conductor method: hundreds of structured sessions under a living `.claude-docs/` system, multi-agent worktree orchestration for parallel feature work, a strict "no partial fixes" gate, and a persistent ops agent running against live SAP systems and databases to drive diagnostics and R&D.
