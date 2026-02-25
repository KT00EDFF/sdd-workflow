# Decision Record Template

> Copy this template when a significant decision is made during Discovery, Specification, or Implementation. Decisions are immutable records — do not edit after approval; create a new record to supersede.

---

## Header

| Field | Value |
|-------|-------|
| **Decision ID** | [DR-YYYY-NNN — e.g., DR-2026-001] |
| **Title** | [Short decision title — e.g., "Use CSV format for bulk billing code import"] |
| **Status** | [Proposed / Approved / Superseded / Withdrawn] |
| **Date** | [YYYY-MM-DD] |
| **Related Problem Brief** | [Link] |
| **Supersedes** | [Link to prior Decision Record, if applicable] |

---

## DACI

| Role | Person |
|------|--------|
| **Driver** | [Who is proposing this decision] |
| **Approver** | [Who has final say] |
| **Contributors** | [Who was consulted] |
| **Informed** | [Who needs to know] |

---

## Context

[Describe the situation that requires a decision. What question are we answering? What prompted this?]

---

## Options Considered

### Option A: [Name]

**Description**: [What this option entails]

**Pros**:
- [Pro 1]
- [Pro 2]

**Cons**:
- [Con 1]
- [Con 2]

**Estimated Effort**: [Relative size]

---

### Option B: [Name]

**Description**: [What this option entails]

**Pros**:
- [Pro 1]
- [Pro 2]

**Cons**:
- [Con 1]
- [Con 2]

**Estimated Effort**: [Relative size]

---

### Option C: [Name] *(optional)*

**Description**: [What this option entails]

**Pros / Cons / Effort**: [...]

---

## Decision

**Chosen option**: [Option letter and name]

---

## Rationale

[Explain WHY this option was chosen. What factors were decisive? What trade-offs were accepted?]

---

## Consequences

**Positive consequences**:
- [What this decision enables]

**Negative consequences / trade-offs accepted**:
- [What we're giving up or accepting as a cost]

**Risks introduced**:
- [Any new risks created by this decision]

---

## Compliance Review

| Question | Answer |
|----------|--------|
| Does this decision affect audit trails? | [Yes / No — if Yes, explain] |
| Does this decision affect approval workflows? | [Yes / No — if Yes, explain] |
| Does this decision affect SOX-regulated operations? | [Yes / No — if Yes, explain] |
| Does this decision introduce new security considerations? | [Yes / No — if Yes, explain] |
| Constitution articles affected | [List article numbers — e.g., "III, VI"] |

---

## Review Date

**Review by**: [YYYY-MM-DD — when this decision should be re-evaluated]

**Review trigger**: [What would cause us to revisit this decision — e.g., "If bulk import exceeds 500 rows regularly"]

---

## Links

- **Problem Brief**: [link]
- **Assumption Map entries**: [relevant assumption IDs]
- **Spec (if created)**: [link]
- **Meeting notes**: [link]
