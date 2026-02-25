# Example Spec Readiness Assessment: Bulk Import of Billing Codes

> This is a filled-in example of a Spec Readiness Assessment (GPT Mode 3 output). Shows the 20-item scored checklist, summary, and recommendation.

---

## Spec Readiness Assessment: Bulk Import of Billing Codes via CSV

*Assessed after Discovery session and resolution of open items (Raj confirmed performance feasibility, David confirmed audit event requirements).*

### Scores

| # | Item | Score | Notes |
|---|------|-------|-------|
| 1 | Problem statement specific and evidence-based | 🟢 | Clear structured statement with support ticket evidence and retro flag |
| 2 | Target user segment identified | 🟢 | "FX PMs managing 40-60 billing codes per quarterly update" — specific |
| 3 | Current workaround documented | 🟢 | Step-by-step current process described with time and error data |
| 4 | Impact quantified | 🟢 | 4+ hours → 15 min; 5% error rate; 15-20 hours/quarter team-wide |
| 5 | V1 scope explicitly bounded | 🟢 | Clear in-scope and out-of-scope lists from Discovery |
| 6 | API surface sketched | 🟢 | POST /api/v1/billing-codes/bulk with request/response shape described |
| 7 | System interactions mapped | 🟡 | Billing Engine and Reporting Platform identified; explicit confirmation from Billing Engine team still pending (Sarah said she'd check, no written confirmation yet) |
| 8 | Backward compatibility assessed | 🟢 | New endpoint, no changes to existing. Additive only. |
| 9 | Key assumptions documented | 🟢 | 5 assumptions rated with confidence/impact in 2x2 grid |
| 10 | High-risk assumptions have validation plans | 🟢 | A2 (performance) validated by Raj — confirmed 500 codes in <30s with current infrastructure |
| 11 | Compliance risks identified | 🟢 | SOX audit events required per code; import-level event confirmed with David; correlation ID requirement documented |
| 12 | Rollback/failure scenarios considered | 🟡 | All-or-nothing design handles failed imports. Rollback for successful-but-wrong imports identified as a gap — bulk deactivation not in V1 scope. Risk accepted with caveat. |
| 13 | Success metrics measurable | 🟢 | Time reduction (4h → 15min), validation error rate (0%), audit consistency |
| 14 | Acceptance criteria testable | 🟢 | Given/When/Then scenarios for happy path, validation failure, idempotency |
| 15 | Performance constraints stated | 🟢 | 500 codes in <60s synchronous; validated by tech lead |
| 16 | Non-functional requirements addressed | 🟡 | Security (RBAC role needed) and auditability covered. Availability and scalability not explicitly stated — acceptable for a batch endpoint. |
| 17 | Tech lead assessed feasibility | 🟢 | Raj confirmed feasibility, estimated 2-3 sprints, no architectural concerns |
| 18 | Domain model impact understood | 🟢 | No schema changes; uses existing BillingCode entity; new BulkImportReport may be needed |
| 19 | DACI roles assigned | 🟢 | Driver: Sarah, Approver: James, Contributors: Raj/Maria/David, Informed: Billing Engine team |
| 20 | Stakeholder alignment confirmed | 🟡 | James (GPM) and Raj briefed. David (Compliance) reviewed. Billing Engine team mentioned as "Informed" but not yet formally notified. |

### Summary
- **Green**: 16/20
- **Yellow**: 4/20
- **Red**: 0/20

### Recommendation

**Ready** — 0 Red items and 4 Yellow items (threshold: 0 Red, ≤3 Yellow is "Ready"; this is borderline but all Yellows are low-risk).

The 4 Yellow items are manageable:
- **Item 7** (System interactions): Billing Engine team confirmation is in progress. Low risk — bulk-created codes use the same entity model.
- **Item 12** (Rollback): Known gap (no bulk deactivation). Accepted as future work, not a V1 blocker.
- **Item 16** (Non-functional): Batch endpoint with admin-only access. Availability/scalability requirements less critical than real-time endpoints.
- **Item 20** (Stakeholder alignment): Billing Engine team notification is a to-do, not a blocker.

These are captured as `[NEEDS CLARIFICATION]` markers in the generated prompt below.

### Draft /speckit.specify Prompt

See `example-specify-prompt.md` for the generated draft.
