# Problem Brief: Bulk Import of Billing Codes via CSV

> This is a filled-in example of the Problem Brief template. Use it as a reference for what a well-written Problem Brief looks like.

---

## Header

| Field | Value |
|-------|-------|
| **Title** | Bulk Import of Billing Codes via CSV |
| **Domain Area** | Billing Codes |
| **Tier** | 2 — Enhancement |
| **Regulatory Flag** | Yes — billing code creation requires audit events (SOX) |
| **Date Created** | 2026-02-24 |
| **Author** | Sarah Chen (PM, FX Products) |
| **Status** | Ready for Spec |

---

## Problem Statement

**Product managers managing the FX taxonomy** experience **significant time waste and data entry errors** when **adding billing codes during quarterly taxonomy updates**, resulting in **4+ hours of manual work per batch and a ~5% error rate that requires manual correction plus audit trail cleanup**.

---

## Current State

**How do users handle this today?**

PMs add billing codes one at a time through the Admin UI:

1. Navigate to Billing Codes section
2. Click "Add New Billing Code"
3. Fill in: code, description, taxonomy node (dropdown), billing rule (dropdown)
4. Set `requiresApproval` flag
5. Click "Create"
6. Repeat for each billing code

For a typical quarterly FX taxonomy update with 40-60 new billing codes, this process takes 4-6 hours.

**Pain points with current approach:**

- **Time**: 4-6 hours of repetitive manual entry for 40-60 codes
- **Errors**: ~5% error rate (wrong taxonomy node, typo in code, wrong billing rule). Approximately 2-3 errors per batch.
- **Error correction**: Each error requires creating a new code (can't edit the wrong one — codes are immutable) and deactivating the incorrect code. Deactivation generates an audit event that requires a written justification.
- **Consistency**: No way to validate all codes against a standard before submission. Errors are found one at a time, post-creation.
- **Scheduling**: The 4-6 hour process must be completed by the quarterly deadline, creating time pressure that increases errors.

---

## Desired State

**What does the improved experience look like?**

PM prepares a CSV file with all billing codes (using a standard template), uploads it through the API, and all codes are validated and created in one operation.

**Key improvements:**

- **Time**: 40-60 billing codes created in under 15 minutes (prep + upload + review)
- **Validation**: All codes validated before any are created (all-or-nothing). Errors caught before creation, not after.
- **Consistency**: CSV template enforces consistent format. Validation rules applied uniformly.
- **Reporting**: Detailed import report showing success/failure per row with specific error messages.

---

## Impact Assessment

| Dimension | Assessment |
|-----------|-----------|
| **Affected Users** | 5 PMs who manage taxonomy updates; primarily the 3 FX PMs who handle the largest batches |
| **Time Impact** | 4-6 hours → ~15 minutes per batch (quarterly, per PM) |
| **Error Impact** | ~5% error rate → 0% (pre-validation). Eliminates ~10-15 error correction cycles per quarter across the team |
| **Revenue/Cost Impact** | Errors in billing codes can delay billing code activation, which delays revenue recognition. Each error adds ~30 minutes of correction + audit justification. |
| **Frequency** | Quarterly taxonomy updates (4x/year), plus ad-hoc new product launches (~6/year) |

---

## Constraints

**Regulatory / Compliance:**
- Each imported billing code must generate an individual audit event (SOX requirement — cannot batch audit events)
- Billing codes with `requiresApproval=true` must enter `PENDING_APPROVAL` status and go through the existing ApprovalWorkflow
- The import operation itself must have a correlation ID linking all individual audit events

**Technical:**
- Must use the existing BillingCode entity model (no schema changes to the billing code table)
- Must validate against existing TaxonomyNode and BillingRule references
- Maximum 500 codes per import (API request size constraint)
- Import must be idempotent — re-uploading the same CSV must not create duplicates

**Business:**
- Must be available before the Q3 2026 taxonomy update (July 1)
- CSV template must be downloadable from the Admin UI
- Must not change the single-code creation workflow (keep existing UI as-is)

**Out of Scope (V1):**
- No updating existing billing codes via CSV (create-only)
- No deletion/deactivation via CSV
- No scheduling of future imports
- No integration with external taxonomy systems (manual CSV only)
- No UI for the import (API-only in V1; Admin UI integration is a separate Tier 2 feature)

---

## DACI

| Role | Person | Responsibility |
|------|--------|---------------|
| **Driver** | Sarah Chen (PM, FX Products) | Owns the Problem Brief through to spec completion |
| **Approver** | James Liu (GPM) | Final decision authority |
| **Contributors** | Raj Patel (Tech Lead), Maria Santos (Designer — UI template download only), David Kim (Compliance Lead) |
| **Informed** | Billing Engine team (downstream consumer), Client Portal team (no impact) |

---

## Status History

| Date | Status | Notes |
|------|--------|-------|
| 2026-02-24 | Draft | Initial Problem Brief created after GPT Triage (Tier 2) |
| 2026-02-24 | In Discovery | Discovery session completed with GPT Mode 2 |
| 2026-02-24 | Ready for Spec | Spec Readiness Assessment passed (16 Green, 4 Yellow, 0 Red) |

---

## Links

- **GPT Triage Output**: See `example-discovery-session.md` (opens with triage)
- **Discovery Session Notes**: See `example-discovery-session.md`
- **Assumption Map**: [Confluence link — would be created from discovery output]
- **Spec Readiness Assessment**: See `example-spec-readiness.md`
- **Spec (when created)**: See `example-specify-prompt.md` for the draft prompt
