# GitHub Spec Kit — Quick Reference

> Reference for spec-kit commands, conventions, and template structure. Used by the SDD Companion GPT when generating `/speckit.specify` prompts.

---

## Overview

GitHub Spec Kit is an open-source CLI toolkit for Spec-Driven Development. It provides three sequential commands that move from intent to implementation:

```
/speckit.specify  →  /speckit.plan  →  /speckit.tasks
     (WHAT)            (HOW)           (DO)
```

Spec Kit works across AI coding assistants: GitHub Copilot, Claude Code, Gemini, Cursor.

---

## Commands

### `/speckit.specify` — Define What

**Purpose**: Capture requirements, acceptance criteria, and constraints for a feature or change.

**Inputs** (provided in the prompt):
- Feature name and description
- User stories or problem statement
- Acceptance criteria (Given/When/Then where applicable)
- Constraints and non-functional requirements
- Constitution article references
- Domain context

**Output**: A specification document in `.specify/specs/` containing:
- Title and description
- Detailed requirements
- Acceptance criteria
- Constitution compliance notes
- Open questions (if any)

**Example prompt structure**:
```
/speckit.specify

Feature: Bulk Import of Billing Codes via CSV

## Problem
Product managers currently add billing codes one at a time through the admin UI.
For large taxonomy updates (e.g., new product line with 50+ codes), this takes
hours and is error-prone.

## Requirements
- Upload a CSV file containing billing code definitions
- Validate all rows before processing any (all-or-nothing)
- Support up to 500 billing codes per import
- Each code must reference a valid taxonomy node
- Codes requiring approval enter PENDING_APPROVAL status
- Produce a detailed import report (success/failure per row)

## Acceptance Criteria
- GIVEN a valid CSV with 100 billing codes
  WHEN the PM uploads via POST /api/v1/billing-codes/bulk
  THEN all 100 codes are created in DRAFT status
  AND an import report is returned with 100 successes

- GIVEN a CSV where row 15 references an invalid taxonomy node
  WHEN the PM uploads the file
  THEN no codes are created
  AND the import report shows the validation error for row 15

## Constraints
- Must comply with Constitution Articles I (API-First), III (Compliance), VI (Schema Governance)
- SOX audit: each imported code must have an individual audit event
- Idempotency: re-uploading the same CSV must not create duplicates

## Domain Context
See: Product Catalog domain model (BillingCode, TaxonomyNode, ApprovalWorkflow entities)
```

---

### `/speckit.plan` — Define How

**Purpose**: Create a technical implementation plan that satisfies the specification.

**Inputs**:
- Reference to the specification created by `/speckit.specify`
- Technology constraints (languages, frameworks, infrastructure)
- Architecture preferences

**Output**: A plan document in `.specify/plans/` containing:
- Architecture overview
- Component breakdown
- API contract (OpenAPI snippet if applicable)
- Data model changes
- Test strategy
- Migration plan (if applicable)
- Constitution compliance mapping

---

### `/speckit.tasks` — Define Work Items

**Purpose**: Break the plan into discrete, implementable tasks.

**Inputs**:
- Reference to the plan created by `/speckit.plan`

**Output**: Task documents in `.specify/tasks/` containing:
- Task title and description
- Acceptance criteria
- Dependencies on other tasks
- Estimated complexity
- Files likely to be modified

---

## Directory Structure

After running spec-kit commands, the repository contains:

```
.specify/
├── constitution.md          # Non-negotiable project principles
├── specs/
│   └── bulk-import-billing-codes.md
├── plans/
│   └── bulk-import-billing-codes.md
└── tasks/
    ├── 001-csv-parser.md
    ├── 002-validation-engine.md
    └── 003-bulk-api-endpoint.md
```

---

## Constitution Document

The constitution is the foundation of spec-kit. It defines principles that every specification must reference and every implementation must satisfy.

**Structure**:
```markdown
# Constitution

## Article I: [Principle Name]
[Description of the non-negotiable principle]

### Rationale
[Why this principle exists]

### Enforcement
[How compliance is verified]
```

**Usage in specs**: Specs explicitly cite which constitution articles apply:
```markdown
## Constitution Compliance
- **Article I (API-First)**: All functionality exposed via REST endpoints
- **Article III (Compliance)**: Audit events for each billing code created
```

**Usage in plans**: Plans map each architecture decision to a constitution article:
```markdown
## Compliance Mapping
| Decision | Constitution Article | How Satisfied |
|----------|---------------------|---------------|
| REST endpoint for bulk import | Article I | POST /api/v1/billing-codes/bulk |
| Individual audit events | Article III | Event emitted per code in batch |
```

---

## Conventions

### Naming
- Spec files: kebab-case matching the feature name (e.g., `bulk-import-billing-codes.md`)
- Plan files: same name as the spec they implement
- Task files: numbered prefix with descriptive name (e.g., `001-csv-parser.md`)

### Cross-References
- Plans reference specs: `Implements: specs/bulk-import-billing-codes.md`
- Tasks reference plans: `Part of: plans/bulk-import-billing-codes.md`
- All artifacts reference constitution articles by number

### Status Tracking
Spec documents include a status field:
- `DRAFT` — Initial creation, under discussion
- `APPROVED` — Reviewed and approved for implementation
- `IMPLEMENTED` — Code complete and validated
- `SUPERSEDED` — Replaced by a newer spec (link to successor)

---

## Tips for Generating Good `/speckit.specify` Prompts

1. **Start with the problem, not the solution** — describe the pain point before the feature
2. **Include concrete acceptance criteria** — Given/When/Then removes ambiguity
3. **Reference constitution articles** — forces the spec to address compliance
4. **Provide domain context** — mention relevant entities from the domain model
5. **State constraints explicitly** — performance limits, backward compatibility, regulatory requirements
6. **Flag open questions** — it's better to acknowledge unknowns than to assume
7. **Keep scope tight** — one feature per spec; break large features into multiple specs
