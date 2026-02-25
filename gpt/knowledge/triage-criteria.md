# Triage Criteria — Tier Definitions

> Used by the SDD Companion GPT (Mode 1: Triage) to classify incoming ideas/requests into the appropriate tier. Classification determines the required process overhead.

---

## Three Classification Axes

Every incoming idea is assessed on three axes:

### 1. Scope
| Level | Description | Examples |
|-------|-------------|----------|
| **Narrow** | Single entity, single field, isolated change | Add a description field to billing code |
| **Moderate** | Multiple entities, new business rules, new workflows | Bulk import with validation rules |
| **Broad** | New domain concepts, cross-cutting concerns, architectural changes | Product lifecycle state machine with approval workflows |

### 2. API Surface Impact
| Level | Description | Examples |
|-------|-------------|----------|
| **None** | No API changes | Configuration update, UI label change |
| **Additive** | New endpoints or optional fields | New GET endpoint for reporting, optional filter parameter |
| **Breaking** | Existing endpoint changes, required field additions, behavior changes | Changing response schema, adding required request field |

### 3. Regulatory Weight
| Level | Description | Examples |
|-------|-------------|----------|
| **None** | No compliance implications | Internal tooling change, documentation update |
| **Standard** | Standard audit requirements, existing approval patterns | New billing code type using existing activation workflow |
| **Elevated** | New SOX touchpoints, new approval workflows, new audit requirements | New entity type that affects financial reporting |

---

## Tier Definitions

### Tier 1: Config Change

**Profile**: Narrow scope + No API impact + No regulatory weight

**Description**: Small changes that don't affect the API surface, domain model, or compliance posture. These are configuration tweaks, data fixes, or minor UI updates.

**Process**: Create a Jira ticket with clear description. No Problem Brief, no Discovery, no spec-kit.

**Time Investment**: 2-minute triage conversation.

**Examples**:
- Add a new billing code to an existing taxonomy node (using existing activation workflow)
- Update a product description field
- Change sort order of categories in the UI
- Fix a typo in an API response message
- Update a configuration parameter in a non-production environment

**Signal phrases**: "just need to add...", "can we update...", "quick change to..."

---

### Tier 2: Enhancement

**Profile**: Moderate scope + Additive API impact + Standard or no regulatory weight

**Description**: Meaningful changes that extend existing capabilities. New endpoints, new business rules, schema extensions — but within the existing domain model and architectural patterns.

**Process**: Problem Brief (Confluence) → optional Discovery session (GPT Mode 2) → Spec Readiness Assessment (GPT Mode 3) → `/speckit.specify` → `/speckit.plan` → implementation.

**Time Investment**: 1-2 weeks of process before development begins.

**Examples**:
- Bulk import of billing codes via CSV upload
- New search/filter endpoint for products by category
- Adding a new billing rule type (e.g., `TIERED_WITH_MINIMUM`)
- Batch deactivation of billing codes by taxonomy node
- New reporting endpoint aggregating product metrics

**Signal phrases**: "we need a way to...", "it would save time if...", "can we build a feature for..."

---

### Tier 3: New Capability

**Profile**: Broad scope + Breaking API impact possible + Elevated regulatory weight likely

**Description**: Significant new capabilities that introduce new domain concepts, require architectural decisions, or have cross-cutting implications. These are product bets that need full SDD lifecycle.

**Process**: Problem Brief → full Discovery session (GPT Mode 2, all 4 stages) → Assumption Map (Confluence) → Spec Readiness Assessment (GPT Mode 3) → `/speckit.specify` → `/speckit.plan` → architecture review → implementation.

**Time Investment**: 3-4 weeks of process before development begins.

**Examples**:
- Product lifecycle state machine with approval workflows
- Self-service product configuration portal for business users
- Real-time webhook notification system for downstream consumers
- Multi-tenant product catalog supporting different business lines
- Automated compliance reporting and audit trail analytics

**Signal phrases**: "we've never had...", "this would fundamentally change...", "we need a whole new way to..."

---

## The Regulatory Escalation Rule

**If a request has compliance implications (SOX, audit trail, approval workflow changes), elevate it by one tier.**

| Base Tier | + Regulatory Flag | Result |
|-----------|-------------------|--------|
| Tier 1 (Config Change) | Has compliance implications | → **Tier 2 (Enhancement)** |
| Tier 2 (Enhancement) | Has compliance implications | → **Tier 3 (New Capability)** |
| Tier 3 (New Capability) | Has compliance implications | Stays Tier 3 (already full process) |

**Examples of regulatory escalation**:
- "Add a billing code" (Tier 1) + "it's a new SOX-regulated code type" → Tier 2
- "Bulk import billing codes" (Tier 2) + "some codes bypass approval workflows" → Tier 3

---

## Triage Output Format

After classifying, the GPT produces a structured output:

```
## Triage Result

**Request**: [one-line summary]
**Tier**: [1/2/3] — [Config Change / Enhancement / New Capability]
**Regulatory Flag**: [Yes/No — if Yes, explain why]

### Classification Rationale
- **Scope**: [Narrow/Moderate/Broad] — [explanation]
- **API Surface Impact**: [None/Additive/Breaking] — [explanation]
- **Regulatory Weight**: [None/Standard/Elevated] — [explanation]

### Recommended Next Steps
[Tier-specific next actions]
```

---

## Quick Decision Matrix

| Scope | API Impact | Regulatory | → Tier |
|-------|-----------|------------|--------|
| Narrow | None | None | **1** |
| Narrow | None | Standard | **2** (escalated) |
| Narrow | Additive | None | **2** |
| Moderate | Additive | None | **2** |
| Moderate | Additive | Standard | **2** |
| Moderate | Additive | Elevated | **3** (escalated) |
| Moderate | Breaking | Any | **3** |
| Broad | Any | Any | **3** |
