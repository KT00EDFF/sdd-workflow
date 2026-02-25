# Spec Template Override — JPMC Product Catalog

> This template extends the default spec-kit specification template with JPMC-specific sections for compliance, API contracts, and downstream consumer impact. Use this for all `/speckit.specify` output.

---

## [Feature Name]

**Status**: [DRAFT / APPROVED / IMPLEMENTED / SUPERSEDED]
**Author**: [Name]
**Date**: [YYYY-MM-DD]
**Tier**: [2 / 3]
**Problem Brief**: [Confluence link]
**Discovery Session**: [Confluence link, if applicable]

---

## Problem

[Structured problem statement: **[User]** experiences **[problem]** when **[context]**, resulting in **[impact]**.]

---

## Requirements

### Functional Requirements

1. [Requirement — specific, testable, traceable to problem statement]
2. [Requirement]
3. [Requirement]

### Non-Functional Requirements

| Category | Requirement |
|----------|------------|
| Performance | [e.g., "Bulk import of 500 codes completes within 60 seconds"] |
| Availability | [e.g., "Endpoint available 99.9% during business hours"] |
| Scalability | [e.g., "Support up to 500 codes per import, 10 concurrent imports"] |
| Security | [e.g., "Requires RBAC role: BILLING_CODE_ADMIN"] |
| Auditability | [e.g., "Individual audit event per billing code created"] |

---

## Acceptance Criteria

```gherkin
Feature: [Feature Name]

  Scenario: [Happy path]
    Given [precondition]
    When [action]
    Then [expected result]
    And [additional expected result]

  Scenario: [Validation failure]
    Given [precondition with invalid data]
    When [action]
    Then [expected error behavior]
    And [no side effects]

  Scenario: [Edge case]
    Given [edge case precondition]
    When [action]
    Then [expected behavior]
```

---

## Compliance Impact Assessment

> **Required for all specs.** This section is reviewed by the compliance team for features touching financial data.

| Question | Answer | Details |
|----------|--------|---------|
| Does this create or modify financial data? | [Yes / No] | [Which entities] |
| Are audit events required? | [Yes / No] | [Which operations, what event structure] |
| Is maker-checker (dual approval) required? | [Yes / No] | [Which operations] |
| Does this affect SOX controls? | [Yes / No] | [Which controls, how] |
| Are new approval workflows needed? | [Yes / No] | [Workflow type, approvers] |
| Does this change data retention requirements? | [Yes / No] | [What changes] |

**Compliance reviewer**: [Name — to be assigned]
**Compliance review status**: [Pending / Approved / Conditional]

---

## API Contract Preview

> **Required for all specs involving API changes.** This is a preview — the full OpenAPI spec is defined in the Plan phase.

### Endpoints

#### `[METHOD] /api/v[N]/[resource]`

**Description**: [What this endpoint does]

**Request**:
```json
{
  "field1": "type — description",
  "field2": "type — description"
}
```

**Response (success)**:
```json
{
  "field1": "type — description",
  "field2": "type — description"
}
```

**Response (error)**:
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable message",
    "details": []
  }
}
```

**Headers**:
| Header | Required | Description |
|--------|----------|-------------|
| `X-Correlation-ID` | Yes | Request correlation ID |
| `X-Idempotency-Key` | Yes (writes) | Idempotency key for write operations |
| `Authorization` | Yes | Bearer token |

---

## Downstream Consumer Impact

> **Required for all specs.** Identifies which downstream systems are affected and what they need to do.

| Consumer | Impact | Action Required | Communication |
|----------|--------|----------------|--------------|
| Billing Engine | [None / Minor / Major] | [What they need to change, if anything] | [Who to notify, when] |
| Client Portal | [None / Minor / Major] | [Action] | [Communication plan] |
| Reporting Platform | [None / Minor / Major] | [Action] | [Communication plan] |
| Risk Systems | [None / Minor / Major] | [Action] | [Communication plan] |
| Internal Admin UI | [None / Minor / Major] | [Action] | [Communication plan] |

**Breaking changes**: [Yes / No — if Yes, see migration plan in Plan phase]

---

## Constitution Compliance

| Article | Applicable | How Satisfied |
|---------|-----------|---------------|
| I — API-First | [Yes / No] | [Brief explanation] |
| II — Domain Integrity | [Yes / No] | [Brief explanation] |
| III — Compliance by Design | [Yes / No] | [Brief explanation] |
| IV — Test-First | [Yes / No] | [Brief explanation] |
| V — Backwards Compatibility | [Yes / No] | [Brief explanation] |
| VI — Schema Governance | [Yes / No] | [Brief explanation] |
| VII — Simplicity / YAGNI | [Yes / No] | [Brief explanation] |
| VIII — Observability | [Yes / No] | [Brief explanation] |
| IX — Security Boundaries | [Yes / No] | [Brief explanation] |

---

## Open Questions

- [NEEDS CLARIFICATION]: [Question that must be resolved during planning]
- [NEEDS CLARIFICATION]: [Question]

---

## Domain Context

**Relevant entities**: [List entities from the domain model that this feature touches]

**Entity relationships affected**: [Describe how relationships change or are extended]

**Glossary terms**: [Any domain terms used in this spec — reference the domain glossary]
