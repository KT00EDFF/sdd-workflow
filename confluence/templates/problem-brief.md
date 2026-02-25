# Problem Brief Template

> Copy this template when creating a new Problem Brief in Confluence. Fill in all sections. Delete placeholder text.

---

## Header

| Field | Value |
|-------|-------|
| **Title** | [Short, descriptive title — e.g., "Bulk Import of Billing Codes via CSV"] |
| **Domain Area** | [Product Catalog domain area — e.g., Billing Codes, Taxonomy, Product Lifecycle, Configuration] |
| **Tier** | [1 / 2 / 3 — from GPT Triage or manual assessment] |
| **Regulatory Flag** | [Yes / No — if Yes, explain in Constraints section] |
| **Date Created** | [YYYY-MM-DD] |
| **Author** | [Name] |
| **Status** | [Draft / In Discovery / Ready for Spec / Specifying / In Development / Shipped / Closed] |

---

## Problem Statement

> Use this structure: **[User]** experiences **[problem]** when **[context]**, resulting in **[impact]**.

[Write the problem statement here. Be specific about who is affected, what the problem is, when it occurs, and what impact it has. Avoid describing solutions.]

**Example**: *Product managers managing the FX taxonomy experience significant time waste when adding billing codes because the admin UI only supports one-at-a-time entry, resulting in 4+ hours of manual work and frequent data entry errors during quarterly taxonomy updates.*

---

## Current State

**How do users handle this today?**

[Describe the current workaround or process. Be specific — walk through the actual steps users take today.]

**Pain points with current approach:**

- [Pain point 1 — e.g., "Manual entry takes 4+ hours for 50 codes"]
- [Pain point 2 — e.g., "No validation until after submission, causing rework"]
- [Pain point 3]

---

## Desired State

**What does the improved experience look like?**

[Describe the desired outcome from the user's perspective. Focus on the experience, not the technical solution.]

**Key improvements:**

- [Improvement 1 — e.g., "50 billing codes created in under 15 minutes"]
- [Improvement 2 — e.g., "All codes validated before any are created"]
- [Improvement 3]

---

## Impact Assessment

| Dimension | Assessment |
|-----------|-----------|
| **Affected Users** | [Who and how many — e.g., "12 PMs, quarterly frequency"] |
| **Time Impact** | [Current vs. desired — e.g., "4 hours → 15 minutes per batch"] |
| **Error Impact** | [Current error rate/consequence — e.g., "~5% error rate, each requiring manual correction + audit trail cleanup"] |
| **Revenue/Cost Impact** | [If applicable — e.g., "Delayed billing code activation delays revenue recognition"] |
| **Frequency** | [How often this occurs — e.g., "Quarterly taxonomy updates, ad-hoc new product launches"] |

---

## Constraints

**Regulatory / Compliance:**
- [List any SOX, audit, or regulatory constraints — e.g., "Each billing code must have an individual audit event per SOX requirements"]

**Technical:**
- [List known technical constraints — e.g., "Must use existing billing code activation workflow"]

**Business:**
- [List business constraints — e.g., "Must be available before Q3 taxonomy update"]

**Out of Scope (V1):**
- [Explicitly list what is NOT included — e.g., "No support for updating existing codes via CSV"]
- [More exclusions]

---

## DACI

| Role | Person | Responsibility |
|------|--------|---------------|
| **Driver** | [Name] | Owns the Problem Brief through to spec completion |
| **Approver** | [Name] | Final decision authority — typically GPM or senior PM |
| **Contributors** | [Names] | Tech lead, designer, domain experts consulted |
| **Informed** | [Names] | Stakeholders who need to know but don't decide |

---

## Status History

| Date | Status | Notes |
|------|--------|-------|
| [YYYY-MM-DD] | Draft | Initial Problem Brief created |
| | | |

---

## Links

- **GPT Triage Output**: [link or paste]
- **Discovery Session Notes**: [Confluence link]
- **Assumption Map**: [Confluence link]
- **Decision Records**: [Confluence links]
- **Spec (when created)**: [GitHub link]
