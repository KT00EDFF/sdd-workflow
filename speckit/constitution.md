# JPMC Product Catalog — Software Constitution

> This constitution defines non-negotiable principles for all software built within the Product Catalog domain. Every specification MUST reference applicable articles. Every implementation MUST satisfy referenced articles. Violations are blockers — not warnings.

**Version**: 1.0
**Effective Date**: 2026-02-24
**Amendment Process**: See `constitution-guide.md`

---

## Article I: API-First Principle

All product catalog functionality MUST be exposed through versioned REST APIs conforming to OpenAPI 3.1.

### Requirements
- No feature ships without a corresponding API endpoint
- No consumer accesses data through direct database queries
- API contracts are defined before implementation begins
- All endpoints are documented in OpenAPI 3.1 format
- API versioning follows URI path versioning (`/api/v1/`, `/api/v2/`)

### Rationale
The Product Catalog serves multiple downstream consumers (billing engine, client portal, reporting, risk systems). A stable, versioned API contract is the only sustainable integration pattern at this scale.

### Enforcement
- Specification must include an API Contract Preview section
- Plan must include OpenAPI schema definitions
- Contract tests verify implementation matches OpenAPI spec
- No direct database access from application code outside the data access layer

---

## Article II: Domain Integrity

The canonical domain model is the single source of truth for entity definitions, relationships, and business rules.

### Requirements
- All entities conform to the documented domain model (Product, ProductCategory, TaxonomyNode, BillingCode, BillingRule, ConfigurationProfile, LifecycleState, ApprovalWorkflow)
- Taxonomy nodes are immutable once published — deprecated, never deleted or renamed
- Product lifecycle transitions follow the defined state machine — no ad-hoc states
- Billing codes are deactivated, never deleted
- Domain language in specs matches the glossary exactly

### Rationale
Domain model integrity prevents semantic drift across teams and systems. When the billing engine and client portal agree on what a "Product" is, integration errors disappear.

### Enforcement
- Specs must use canonical entity names from the domain model
- Plans must map new entities/fields to the domain model or propose amendments
- Schema migrations are reviewed against domain model documentation
- No "shadow" data models in consumer applications

---

## Article III: Compliance by Design

Every state-changing operation MUST produce an immutable audit event. Regulated operations MUST use approval workflows.

### Requirements
- All mutations produce audit events containing: who, what, when, why (correlation ID), before/after state
- SOX-regulated operations require dual approval (maker-checker pattern)
- All API requests carry a correlation ID (`X-Correlation-ID` header)
- Approval workflows are tracked with full event history
- Audit events have 7-year minimum retention
- Deactivated entities are retained indefinitely

### Rationale
The Product Catalog manages billing codes and product configurations that directly impact financial reporting. SOX compliance is not optional — it's a legal requirement.

### Enforcement
- Specification must include a Compliance Impact Assessment
- Plans must identify which operations are SOX-regulated
- Integration tests verify audit event emission for every write endpoint
- Approval workflow tests verify maker-checker enforcement

---

## Article IV: Test-First Development

All features MUST have tests written before or alongside implementation. No feature ships without passing automated tests.

### Requirements
- Unit tests for all business logic (minimum 80% line coverage on new code)
- Integration tests for all API endpoints
- Contract tests verifying OpenAPI spec compliance
- Acceptance tests derived from specification acceptance criteria
- No mocking of the data access layer in integration tests — use test containers or in-memory databases
- Test data must be representative of production scenarios

### Rationale
In a regulated environment, untested code is unauditable code. Contract tests prevent the "works on my machine" integration failures that plague multi-consumer architectures.

### Enforcement
- Plans must include a Test Strategy section
- CI pipeline rejects PRs with failing tests
- Coverage gates enforce minimum thresholds on changed files
- Contract test failures block deployment

---

## Article V: Backwards Compatibility

Published APIs MUST NOT introduce breaking changes without a documented migration path and deprecation period.

### Requirements
- Semantic versioning (semver) for all API versions
- Breaking changes require a new API version (`v1` → `v2`)
- Deprecated endpoints must remain functional for a minimum of 2 release cycles
- Deprecation notices include: sunset date, migration guide, replacement endpoint
- Consumer impact assessment required before deprecation

### Rationale
Downstream consumers (billing engine, client portal, risk systems) cannot absorb breaking changes without coordination. Unplanned breakage causes production incidents in financial systems.

### Enforcement
- Specification must include a Downstream Consumer Impact section
- Plans must include a Migration Plan for any breaking changes
- CI includes backward compatibility checks (schema comparison)
- Deprecation headers added to sunset endpoints (`Sunset`, `Deprecation`)

---

## Article VI: Schema Governance

Database schemas evolve through versioned, reviewable migration scripts. Data is preserved, never destroyed.

### Requirements
- All schema changes are expressed as migration scripts (forward and rollback)
- Taxonomy is append-only — new nodes are added, existing nodes are deprecated (never deleted or renamed)
- Billing codes and products are deactivated, never hard-deleted
- Migration scripts are reviewed by a tech lead and a DBA
- Schema changes in production require a change management ticket

### Rationale
The Product Catalog is the enterprise master for product data. Schema changes ripple across every downstream consumer. Append-only taxonomy ensures historical references remain valid indefinitely.

### Enforcement
- Plans must include schema migration scripts or confirm no schema changes
- CI validates migration scripts execute cleanly against a replica
- No `DROP TABLE`, `DROP COLUMN`, or `DELETE` in production migration scripts without explicit approval
- Taxonomy node deletion triggers a build failure

---

## Article VII: Simplicity / YAGNI

Build only what the specification requires. Do not anticipate requirements that have not been validated.

### Requirements
- Every feature traces to a specification with validated requirements
- No speculative abstractions ("we might need this later")
- Prefer simple solutions over clever ones
- Configuration over code only when variability is a validated requirement
- No premature optimization — measure first, optimize with evidence

### Rationale
Over-engineering creates maintenance burden and obscures business logic. In a regulated environment, unnecessary code is unnecessary audit surface.

### Enforcement
- Code review verifies all changes trace to specification requirements
- Unused code and dead feature flags are removed in the same PR that supersedes them
- Architecture decision records document why complexity was necessary

---

## Article VIII: Observability

All services MUST emit structured logs, health checks, and distributed traces.

### Requirements
- Structured logging (JSON format) with correlation IDs on every log entry
- Health check endpoints: `/health` (liveness) and `/ready` (readiness)
- Distributed tracing with trace context propagation (W3C Trace Context)
- Metrics: request count, latency percentiles (p50, p95, p99), error rate
- Alert thresholds defined for all critical paths (billing code activation, product lifecycle transitions)

### Rationale
In a multi-consumer architecture, debugging production issues requires end-to-end visibility. Correlation IDs connecting audit events to traces connect business operations to technical execution.

### Enforcement
- Plans must identify key observability touchpoints
- Integration tests verify structured log output
- Health check endpoints are tested in CI
- Production readiness review includes observability checklist

---

## Article IX: Security Boundaries

All access MUST be authenticated and authorized. Secrets MUST NOT exist in code or configuration files.

### Requirements
- Authentication via SSO/OAuth 2.0 — no application-managed passwords
- Authorization via RBAC with principle of least privilege
- API keys and secrets stored in vault (e.g., HashiCorp Vault, CyberArk)
- No secrets in source code, environment variables, or configuration files committed to version control
- Service-to-service communication uses mutual TLS or OAuth client credentials
- Input validation on all API endpoints (reject unexpected fields, enforce type constraints)

### Rationale
The Product Catalog manages financially sensitive data. A security breach in billing codes or product configurations has direct financial and regulatory consequences.

### Enforcement
- CI scans for secrets in committed code (e.g., git-secrets, truffleHog)
- Security review required for any new authentication/authorization patterns
- Penetration testing includes Product Catalog API endpoints
- RBAC configuration is version-controlled and auditable
