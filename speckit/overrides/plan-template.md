# Plan Template Override — JPMC Product Catalog

> This template extends the default spec-kit plan template with JPMC-specific sections for compliance review, OpenAPI specification, and migration planning. Use this for all `/speckit.plan` output.

---

## [Feature Name] — Implementation Plan

**Status**: [DRAFT / APPROVED / IN PROGRESS / COMPLETE]
**Author**: [Name]
**Date**: [YYYY-MM-DD]
**Implements**: [Link to spec — e.g., `specs/bulk-import-billing-codes.md`]
**Problem Brief**: [Confluence link]

---

## Architecture Overview

[High-level description of the implementation approach. How does this fit into the existing system architecture?]

```
[Simple ASCII architecture diagram showing the key components and data flow]
```

---

## Component Breakdown

### Component 1: [Name]

**Responsibility**: [What this component does]

**Key decisions**:
- [Decision 1 — e.g., "CSV parsing uses streaming to handle large files"]
- [Decision 2]

**Interfaces**:
- Input: [What this component receives]
- Output: [What this component produces]

### Component 2: [Name]

**Responsibility**: [...]

---

## Data Model Changes

### New Entities / Tables

| Entity | Fields | Notes |
|--------|--------|-------|
| [Entity name] | [Key fields] | [Why this is needed] |

### Modified Entities / Tables

| Entity | Change | Migration Notes |
|--------|--------|----------------|
| [Entity name] | [What changes] | [How existing data is affected] |

### No Changes

[If no data model changes, state explicitly: "No data model changes required."]

---

## OpenAPI Specification

> **Required for all plans involving API changes.** Full endpoint specification expanding on the spec's API Contract Preview.

```yaml
openapi: 3.1.0
info:
  title: "[Feature Name] API"
  version: "[API version]"

paths:
  /api/v[N]/[resource]:
    [method]:
      summary: "[Description]"
      operationId: "[operationId]"
      tags:
        - [tag]
      parameters:
        - name: X-Correlation-ID
          in: header
          required: true
          schema:
            type: string
            format: uuid
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/[RequestSchema]'
      responses:
        '200':
          description: "[Success description]"
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/[ResponseSchema]'
        '400':
          description: "Validation error"
        '409':
          description: "Conflict (idempotency)"

components:
  schemas:
    [RequestSchema]:
      type: object
      properties:
        [field]:
          type: [type]
          description: "[description]"
      required:
        - [field]
    [ResponseSchema]:
      type: object
      properties:
        [field]:
          type: [type]
          description: "[description]"
```

---

## Test Strategy

### Unit Tests
| Component | What's Tested | Approach |
|-----------|--------------|----------|
| [Component] | [Business logic] | [e.g., "JUnit 5 with parameterized tests for validation rules"] |

### Integration Tests
| Scope | What's Tested | Approach |
|-------|--------------|----------|
| [API endpoint] | [End-to-end behavior] | [e.g., "Testcontainers with PostgreSQL, full request/response cycle"] |

### Contract Tests
| Contract | Consumer | Approach |
|----------|----------|----------|
| [OpenAPI spec] | [Consumer name] | [e.g., "Specmatic contract verification against OpenAPI spec"] |

### Acceptance Tests
| Scenario | From Spec | Approach |
|----------|-----------|----------|
| [Scenario name] | [Spec acceptance criterion reference] | [e.g., "Cucumber scenario matching Given/When/Then from spec"] |

---

## Compliance Review Checkpoint

> **Required. This section must be reviewed by the compliance team before implementation begins for any feature touching financial data.**

### SOX Control Mapping

| Operation | SOX Control | Implementation |
|-----------|------------|----------------|
| [e.g., "Billing code creation"] | [e.g., "Dual approval for financial data changes"] | [e.g., "ApprovalWorkflow with DUAL_APPROVAL type"] |

### Audit Event Design

| Event | Trigger | Payload |
|-------|---------|---------|
| [e.g., `BILLING_CODE_BULK_IMPORTED`] | [When the event fires] | [Key fields: actor, entityIds, correlationId, before/after] |

### Approval Workflow Changes

[Describe any new or modified approval workflows, or state "No changes to approval workflows."]

### Compliance Sign-off

| Reviewer | Status | Date | Notes |
|----------|--------|------|-------|
| [Compliance reviewer name] | [Pending / Approved / Conditional] | [Date] | [Any conditions] |

---

## Migration Plan

> **Required if there are breaking API changes, schema changes, or behavior changes affecting existing consumers.**

### Is a Migration Required?

[Yes / No — if No, state why: "All changes are additive and backward-compatible."]

### Migration Steps (if applicable)

1. **[Step 1]**: [What happens — e.g., "Deploy new endpoint alongside existing endpoint"]
2. **[Step 2]**: [e.g., "Notify consumers of new endpoint availability"]
3. **[Step 3]**: [e.g., "Begin deprecation period for old endpoint"]
4. **[Step 4]**: [e.g., "Remove old endpoint after deprecation period"]

### Rollback Plan

[How to reverse this change if something goes wrong in production]

**Rollback trigger**: [What condition triggers a rollback]
**Rollback steps**:
1. [Step 1]
2. [Step 2]

### Data Migration

[If schema changes require data migration, describe the approach]

**Migration script**: [Location or description]
**Estimated duration**: [How long the migration takes]
**Can run online?**: [Yes / No — does it require downtime?]

---

## Constitution Compliance Mapping

| Decision | Constitution Article | How Satisfied |
|----------|---------------------|---------------|
| [Architecture decision] | [Article #] | [How this decision satisfies the article] |
| [Technology choice] | [Article #] | [Explanation] |
| [Test approach] | [Article #] | [Explanation] |

---

## Implementation Sequence

Suggested order for implementing components:

| Order | Component | Dependencies | Estimated Complexity |
|-------|-----------|-------------|---------------------|
| 1 | [Component] | None | [S / M / L] |
| 2 | [Component] | [Depends on #1] | [S / M / L] |
| 3 | [Component] | [Depends on #1, #2] | [S / M / L] |

---

## Open Questions

- [Question that must be resolved during implementation]
- [Question]

---

## Links

- **Spec**: [Link to specification]
- **Problem Brief**: [Confluence link]
- **Decision Records**: [Confluence links for decisions made during planning]
- **Related specs**: [Links to related specifications]
