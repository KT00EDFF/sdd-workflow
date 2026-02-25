# Capability Record Template

> Copy this template when documenting an existing (already-shipped) capability. Used in the retrofitting process to create institutional memory for undocumented features.

---

## Header

| Field | Value |
|-------|-------|
| **Capability Name** | [Name — e.g., "Single Billing Code Creation"] |
| **Status** | [Live / Degraded / Sunsetting] |
| **Original Ship Date** | [YYYY-MM-DD or approximate — e.g., "~Q2 2024"] |
| **Current PM Owner** | [Name] |
| **Current Tech Owner** | [Name] |
| **Tier** | [Bet / Enhancement / Incremental — classification at time of build] |
| **Last Updated** | [YYYY-MM-DD] |

---

## What It Does

> Describe the capability from the customer/user experience perspective. Present tense. Write as if explaining to a new team member.

[What does this capability allow users to do? What is the user workflow? What are the key behaviors?]

---

## Why It Exists

> Describe the original problem or opportunity that led to building this. Be honest about knowledge gaps.

**Original problem/opportunity**: [What triggered this work?]

**Original context**: [What was happening in the business/market that made this relevant?]

**Knowledge gaps**: [What do we NOT know about why this was built? Flag honestly — e.g., "Original PM left the team; rationale reconstructed from Jira tickets"]

---

## How It's Performing

> Current usage metrics and health indicators. Flag data gaps explicitly.

| Metric | Value | Source | Notes |
|--------|-------|--------|-------|
| [Usage frequency] | [Value] | [Where this data comes from] | [Any caveats] |
| [Error rate] | [Value] | [Source] | [Caveats] |
| [User satisfaction] | [Value or "No data"] | [Source] | [Caveats] |

**Data gaps**: [What metrics are we missing? What would we want to measure but can't currently?]

---

## Known Issues & Limitations

> From support tickets, user feedback, internal knowledge, tech debt assessments.

| Issue | Severity | Source | Notes |
|-------|----------|--------|-------|
| [Issue description] | [High / Medium / Low] | [Support tickets / User feedback / Internal knowledge] | [Any context] |
| | | | |

---

## Technical Landscape

> Tech lead fills this section.

**Services involved**: [List services/APIs that power this capability]

**Data model**: [Key entities, tables, relationships]

**API endpoints**: [List relevant endpoints]

**Key dependencies**: [What this capability depends on — upstream services, external systems]

**Tech debt**: [Known technical debt, architectural concerns]

**Infrastructure**: [Deployment model, scaling characteristics]

---

## Design State

> Designer fills this section.

**Figma files**: [Links to design files]

**UX known issues**: [Usability problems, accessibility gaps]

**Design system compliance**: [How well does this follow the design system? Any deviations?]

**User research**: [Any past research related to this capability — links to studies, interview notes]

---

## Iteration Opportunities

> Improvement candidates. These may flow into the greenfield lifecycle as Problem Briefs.

| Opportunity | Type | Priority | Notes |
|------------|------|----------|-------|
| [Description of improvement] | [Enhancement / Fix / Rebuild] | [High / Medium / Low] | [Why this matters] |
| | | | |

**Connection to greenfield workflow**: [If any of these opportunities become Problem Briefs, link them here]

---

## Links

| Resource | Link |
|----------|------|
| Existing documentation | [Links to any existing docs] |
| Jira epics/stories | [Links] |
| Figma designs | [Links] |
| spec-kit specs | [Links, if any] |
| Code repositories | [Links] |
| Monitoring dashboards | [Links] |
| Support ticket queue | [Links] |
