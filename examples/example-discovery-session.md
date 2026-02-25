# Example Discovery Session: Bulk Import of Billing Codes

> Simulated transcript of a GPT Discovery session (Mode 2). This shows the GPT's questioning style, how it challenges the PM, and what the structured output looks like.

---

## Session Setup

**PM**: Sarah Chen (FX Products)
**Feature**: Bulk Import of Billing Codes via CSV
**Tier**: 2 (Enhancement) — previously triaged
**Mode**: Discovery Companion (Mode 2)

---

## Transcript

**PM**: I have a Problem Brief for bulk importing billing codes. Right now, our PMs add billing codes one at a time and it takes hours during taxonomy updates. We want to build a CSV upload feature.

**GPT**: Let's run through Discovery on this. I'll ask questions in four stages: problem validation, scope, assumptions, and success criteria. Let's start with the problem.

### Stage 1: Problem Validation

**GPT**: Who specifically experiences this problem? Not "PMs" generally — which PMs, doing what, how often?

**PM**: The FX PMs — Sarah, Mike, and Jessica. They manage the largest taxonomy with 40-60 new billing codes per quarterly update. The other 2 PMs do smaller updates, maybe 10-15 codes.

**GPT**: Good — specific. How do they handle it today? Walk me through the last time this happened.

**PM**: Last quarter, I personally added 52 billing codes. I had a spreadsheet with all the codes planned out, then I went into the Admin UI and entered them one by one. Took me about 4 and a half hours. I made 3 mistakes — wrong taxonomy node on two codes and a typo in one code string.

**GPT**: What happened with those 3 mistakes?

**PM**: I had to create new codes with the correct values and deactivate the wrong ones. Each deactivation requires a written justification for the audit trail. That added another 45 minutes.

**GPT**: So you already have the data in a spreadsheet. The pain is the manual transfer from spreadsheet to UI, plus error correction. What evidence do you have beyond your own experience?

**PM**: Mike and Jessica have the same complaint. I've seen 2 support tickets from Mike about it. And our team retro last quarter flagged it as a pain point.

**GPT**: Three PMs experiencing it, retro flag, support tickets — that's a reasonable evidence base for a Tier 2 enhancement. Not a single anecdote. What happens if we don't solve this?

**PM**: We keep wasting 15-20 hours per quarter across the team on data entry. And the error rate stays at about 5%, which means audit trail cleanup every quarter.

**GPT**: Quantified: 15-20 hours/quarter, 5% error rate with audit cleanup cost. Solid. Let's move to scope.

### Stage 2: Scope & Boundaries

**GPT**: You said "CSV upload feature." What's the smallest version that solves the problem?

**PM**: Upload a CSV, validate all rows, create the codes. That's it.

**GPT**: Where does the CSV come from? Who creates it?

**PM**: The PM creates it. We already use spreadsheets to plan our codes. We'd just need a standard template.

**GPT**: API-only, or does the Admin UI need to support upload?

**PM**: Good question. For V1, I think API-only is fine. We can curl it or use Postman. The FX PMs are technical enough. Admin UI integration can be a follow-up.

**GPT**: That's a clear V1 boundary. What about updating existing codes or deactivating codes via CSV?

**PM**: No — create only in V1. Updates and deactivations are different workflows with different approval requirements.

**GPT**: Good anti-requirements. Let me push on the API surface: what does this endpoint look like?

**PM**: POST to something like `/api/v1/billing-codes/bulk`. Upload the CSV in the request body. Get back a report of what was created and what failed.

**GPT**: What happens when validation fails? Do you want all-or-nothing (no codes created if any row fails) or partial success?

**PM**: All-or-nothing. If row 15 is bad, I want to fix it and re-upload. Partial creates would be a nightmare to reconcile.

**GPT**: Strong opinion, well-reasoned. What about the downstream consumers? The Billing Engine reads billing codes — will they need to change?

**PM**: No. Bulk-created codes are identical to individually-created codes in the database. The Billing Engine doesn't care how a code was created.

**GPT**: Have you confirmed that with the Billing Engine team?

**PM**: ...not explicitly. But the codes are the same entities.

**GPT**: That's an assumption, not a confirmation. Let's flag it for Stage 3. What other systems care about billing code creation?

**PM**: Reporting Platform pulls billing code data for compliance reports. Same thing though — they read the codes, not the creation method.

**GPT**: Another assumption to validate. Let's move to assumptions and risks.

### Stage 3: Assumptions & Risks

**GPT**: What's the assumption that, if wrong, would kill this feature?

**PM**: That PMs will actually use a CSV template consistently. If they submit garbage data, the validation overhead might not be worth it.

**GPT**: Rate it: how confident are you, and how much does it matter?

**PM**: Confidence: High — we already use spreadsheets to plan codes. Impact: High — if adoption is low, we built this for nothing.

**GPT**: High confidence, high impact — that's a "Monitor" assumption. You believe it but you should track adoption. What about the technical assumption: can you process 500 codes in a single request without timing out?

**PM**: I... haven't confirmed that with Raj. I assumed it would be fine.

**GPT**: That's a technical feasibility assumption with medium confidence and high impact. If the API times out at 100 codes, your design changes significantly. That goes in the "Validate Now" quadrant. What else?

**PM**: I'm assuming the existing validation rules for individual billing codes can be applied to bulk codes without modification.

**GPT**: Confidence?

**PM**: Medium. The validation logic exists for single codes. I assume it can run in a loop, but there might be batch-specific validation (like uniqueness within the batch) that doesn't exist today.

**GPT**: Medium confidence, medium impact. "Accept with caveat" — note it, but don't block on it. The tech lead can assess during planning. What about compliance?

**PM**: Each code needs its own audit event. I know that.

**GPT**: Good. Does the import itself need a special audit event? Like "BULK_IMPORT_INITIATED" with the CSV hash and correlation ID?

**PM**: I hadn't thought about that. That's probably a compliance question for David.

**GPT**: Flag it. What's the rollback plan if an import goes wrong in production?

**PM**: Since it's all-or-nothing, a failed import creates nothing. A successful import... I guess we'd need to deactivate all the codes from that batch if something was wrong. The correlation ID would let us find them all.

**GPT**: Can you deactivate in bulk today?

**PM**: No. We'd deactivate one by one. That's... a problem for large batches.

**GPT**: Important finding. If a 500-code import goes wrong, you need 500 individual deactivations. Consider whether a "bulk deactivation by import ID" is needed for safety. Let's capture that as an open item. Moving to success criteria.

### Stage 4: Success Criteria

**GPT**: How will you measure whether this succeeded?

**PM**: Time to create 50 codes drops from 4 hours to 15 minutes. Zero validation errors in production imports after 30 days.

**GPT**: Those are measurable. What does "done" look like for V1? Walk me through the happy path.

**PM**: PM downloads CSV template, fills it out with code, description, taxonomy node code, billing rule ID, and requiresApproval flag. PM sends POST request to `/api/v1/billing-codes/bulk` with the CSV. System validates all rows — checks taxonomy node exists, billing rule exists, code format is valid, no duplicates within batch or against existing codes. If all valid: creates all codes in DRAFT status (or PENDING_APPROVAL if flagged). Returns import report with success/failure per row plus summary.

**GPT**: What about the error path? PM uploads a CSV with one bad row.

**PM**: No codes are created. The import report shows which row failed and why. PM fixes the CSV and re-uploads.

**GPT**: Idempotency: if PM re-uploads the same CSV after a successful import, what happens?

**PM**: It should detect that those codes already exist and reject them as duplicates. No new codes created.

**GPT**: Performance constraints?

**PM**: 500 codes should process within 60 seconds. If it takes longer, PMs will think it's broken.

**GPT**: 500 codes, 60 seconds, synchronous response. Does the PM wait for the full response, or should this be async?

**PM**: Synchronous for V1. 60 seconds is acceptable for a wait-for-response model.

**GPT**: Who needs to approve this before it ships?

**PM**: James (GPM) is the Approver. Raj (Tech Lead) and David (Compliance) are Contributors.

**GPT**: Have Raj and David been briefed?

**PM**: Raj knows about it informally. David hasn't been briefed yet.

**GPT**: That's a Yellow on stakeholder alignment. David needs to see this before you spec it — compliance implications are real.

---

## Discovery Summary

### Problem Statement
Product managers managing the FX taxonomy experience significant time waste and data entry errors when adding billing codes because the Admin UI only supports one-at-a-time entry, resulting in 4+ hours of manual work per batch and a ~5% error rate requiring audit trail cleanup.

### V1 Scope
**In scope:**
- CSV upload endpoint for bulk billing code creation
- Standard CSV template (downloadable)
- Full validation before creation (all-or-nothing)
- Support up to 500 codes per import
- Individual audit events per code
- Import report with per-row results
- Idempotency (re-upload detection)

**Out of scope:**
- Admin UI for upload (API-only in V1)
- Updating existing codes via CSV
- Deactivation via CSV
- Scheduled/future imports
- Bulk deactivation (flagged as future safety need)

### Key Assumptions
| # | Assumption | Confidence | Impact | Quadrant |
|---|-----------|------------|--------|----------|
| A1 | PMs will adopt CSV template workflow | High | High | Monitor |
| A2 | 500 codes can be processed in <60s synchronously | Medium | High | Validate Now |
| A3 | Existing single-code validation logic works for batch | Medium | Medium | Accept with Caveat |
| A4 | Billing Engine unaffected by bulk-created codes | High | Medium | Monitor |
| A5 | Reporting Platform unaffected by bulk-created codes | High | Low | Accept |

### Success Criteria
- Time to create 50 billing codes: ≤15 minutes (including CSV prep)
- Validation error rate in production imports: 0% after 30 days
- Zero audit trail inconsistencies from bulk imports

### Open Items
1. Tech lead (Raj) needs to confirm 500-code processing performance
2. Compliance lead (David) needs to review: does the import operation itself need a special audit event?
3. Consider whether "bulk deactivation by import ID" is needed as a safety net (future feature, not V1)
4. Explicitly confirm with Billing Engine team that bulk-created codes are compatible

### Recommended Next Step
Resolve open items 1 and 2 (quick conversations), then proceed to Spec Readiness Assessment (Mode 3).
