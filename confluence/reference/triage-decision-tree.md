# Triage Decision Tree

> A structured flowchart for classifying incoming requests. Use this alongside the Triage Criteria definitions. Start at the top and follow the path.

---

## Decision Flowchart

```
START: New request/idea received
│
├─ Q1: Does this change the API surface?
│  │
│  ├─ NO → Q2: Does this change the database schema?
│  │  │
│  │  ├─ NO → Q3: Does this have compliance implications?
│  │  │  │
│  │  │  ├─ NO → ✅ TIER 1: Config Change
│  │  │  │       → Create Jira ticket
│  │  │  │
│  │  │  └─ YES → ⬆️ TIER 2: Enhancement (escalated)
│  │  │           → Problem Brief required
│  │  │
│  │  └─ YES → Q4: Is the schema change additive (new tables/columns only)?
│  │     │
│  │     ├─ YES → TIER 2: Enhancement
│  │     │        → Problem Brief + spec-kit
│  │     │
│  │     └─ NO → TIER 3: New Capability
│  │              → Full SDD lifecycle
│  │
│  └─ YES → Q5: Are the API changes additive only (new endpoints/optional fields)?
│     │
│     ├─ YES → Q6: Does this introduce new domain concepts?
│     │  │
│     │  ├─ NO → Q7: Does this have elevated compliance implications?
│     │  │  │
│     │  │  ├─ NO → TIER 2: Enhancement
│     │  │  │       → Problem Brief + spec-kit
│     │  │  │
│     │  │  └─ YES → ⬆️ TIER 3: New Capability (escalated)
│     │  │           → Full SDD lifecycle
│     │  │
│     │  └─ YES → TIER 3: New Capability
│     │           → Full SDD lifecycle
│     │
│     └─ NO (breaking changes) → TIER 3: New Capability
│              → Full SDD lifecycle
```

---

## Quick-Reference Summary

| Path | Result | Next Step |
|------|--------|-----------|
| No API change → No schema change → No compliance | **Tier 1** | Jira ticket |
| No API change → No schema change → Compliance flag | **Tier 2** (escalated) | Problem Brief |
| No API change → Additive schema change | **Tier 2** | Problem Brief + spec-kit |
| No API change → Non-additive schema change | **Tier 3** | Full SDD lifecycle |
| Additive API change → No new domain concepts → No elevated compliance | **Tier 2** | Problem Brief + spec-kit |
| Additive API change → No new domain concepts → Elevated compliance | **Tier 3** (escalated) | Full SDD lifecycle |
| Additive API change → New domain concepts | **Tier 3** | Full SDD lifecycle |
| Breaking API change | **Tier 3** | Full SDD lifecycle |

---

## Compliance Trigger Checklist

Before finalizing the tier, check these compliance triggers. If **any** are true, set the regulatory flag and consider escalation:

- [ ] Creates, modifies, or deactivates billing codes
- [ ] Changes billing rules or calculation logic
- [ ] Modifies product lifecycle state transitions
- [ ] Introduces or changes approval workflows
- [ ] Affects data that appears in financial reports
- [ ] Changes access control or authorization rules for sensitive data
- [ ] Modifies audit trail behavior or retention policies
- [ ] Introduces a new integration with an external system that handles financial data

**Rule**: If any box is checked and the current tier is below Tier 3, escalate by one tier.

---

## Triage Conversation Guide

### Opening Questions (ask in order, stop when tier becomes clear)

1. **"What changes, from the user's perspective?"**
   - Helps assess scope (narrow vs. broad)
   - Listen for: single field change (Tier 1) vs. new workflow (Tier 2/3)

2. **"Does this need a new API endpoint or change an existing one?"**
   - Directly classifies API surface impact
   - "No" → likely Tier 1; "New endpoint" → likely Tier 2; "Change existing" → likely Tier 3

3. **"Does this touch billing codes, product lifecycle, or approval workflows?"**
   - Compliance trigger check
   - "Yes" → regulatory flag; consider escalation

4. **"Are we introducing a concept the system doesn't have today?"**
   - Domain integrity check
   - "Yes" → Tier 3 (new domain concept)

5. **"Who else needs to know about this change?"**
   - Downstream consumer impact check
   - "Nobody" → contained scope; "Billing team, client portal team" → broader scope

---

## Examples

### Example 1: "Add a new billing code for FX spot trading"
- API change? **No** — using existing activation workflow
- Schema change? **No** — new row, not new structure
- Compliance? **Standard** — existing billing code workflow handles it
- **→ Tier 1: Config Change** (standard billing activation is already in place)

### Example 2: "Build a CSV upload for bulk billing code creation"
- API change? **Yes** — new endpoint (`POST /api/v1/billing-codes/bulk`)
- Additive? **Yes** — new endpoint, no changes to existing
- New domain concept? **No** — billing codes already exist
- Compliance? **Standard** — each code still goes through activation workflow
- **→ Tier 2: Enhancement**

### Example 3: "Add product lifecycle states with approval workflows"
- API change? **Yes** — new endpoints for transitions, approvals
- New domain concept? **Yes** — LifecycleState and ApprovalWorkflow
- Compliance? **Elevated** — new SOX touchpoints for lifecycle transitions
- **→ Tier 3: New Capability**

### Example 4: "Let PMs deactivate billing codes in bulk"
- API change? **Yes** — new bulk deactivation endpoint
- Additive? **Yes** — new endpoint
- New domain concept? **No** — deactivation already exists
- Compliance? **Elevated** — bulk deactivation of financial codes is a SOX concern
- **→ Tier 3: New Capability** (escalated from Tier 2 due to compliance)
