# Product Catalog Lifecycle Guide

> Full process reference explaining how Confluence, the Custom GPT, and GitHub spec-kit connect into a closed-loop development system. This is the canonical "how we work" document.

---

## System Overview

The Product Catalog development lifecycle uses three integrated systems:

```
┌──────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  PROBLEM                 THINKING               SPECIFICATION        │
│  ┌───────────┐          ┌───────────┐          ┌───────────┐        │
│  │           │          │           │          │           │        │
│  │ Confluence │◄────────►│  Custom   │─────────►│  GitHub   │        │
│  │           │          │   GPT     │          │ spec-kit  │        │
│  │ (Memory)  │          │ (Sharpener)│          │  (Specs)  │        │
│  │           │          │           │          │           │        │
│  └───────────┘          └───────────┘          └───────────┘        │
│       ▲                                              │               │
│       │              ┌───────────┐                   │               │
│       │              │           │                   │               │
│       └──────────────│Development│◄──────────────────┘               │
│                      │           │                                    │
│                      └─────┬─────┘                                   │
│                            │                                         │
│                      ┌─────▼─────┐                                   │
│                      │  Release  │──── Outcomes ────► Confluence     │
│                      └───────────┘                                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Key principle**: Data flows between systems via copy-paste. This is deliberate — regulated environments require human review at every transition, and the friction forces people to read output before moving it.

---

## Roles

| Role | Primary Responsibilities |
|------|------------------------|
| **GPM (Group Product Manager)** | Portfolio oversight, quality checks via Portfolio Dashboard, coaching, calibration |
| **PM (Product Manager)** | Drives ideas through lifecycle: triage, discovery, specification, validation |
| **Tech Lead** | Technical feasibility assessment, architecture decisions, constitution compliance |
| **Designer** | UX research, design exploration, design system compliance |
| **Compliance Lead** | SOX control review, audit trail verification, regulatory sign-off |

---

## The Lifecycle: Step by Step

### Step 1: Problem Surfaces

**Where**: Anywhere — user feedback, support tickets, stakeholder requests, team ideas, data analysis.

**Who**: Anyone on the team.

**Output**: A raw problem or idea description, communicated to the PM.

---

### Step 2: Triage (GPT Mode 1)

**Where**: Custom GPT.

**Who**: PM.

**Input**: Raw problem/idea description.

**Process**:
1. PM describes the idea to the GPT
2. GPT assesses on three axes: Scope, API Surface Impact, Regulatory Weight
3. GPT applies the regulatory escalation rule
4. GPT produces a structured Triage Result with tier classification

**Output**: Triage Result with tier (1, 2, or 3) and recommended next steps.

**Next step by tier**:
- **Tier 1**: Create Jira ticket. Done. (~2 minutes)
- **Tier 2**: Create Problem Brief in Confluence → optional Discovery → Spec Readiness
- **Tier 3**: Create Problem Brief in Confluence → full Discovery → Spec Readiness

---

### Step 3: Problem Brief (Confluence)

**Where**: Confluence — Product Development Hub space.

**Who**: PM.

**Applies to**: Tier 2 and Tier 3 only.

**Input**: Triage Result from GPT.

**Process**:
1. Create new page from Problem Brief template
2. Fill in: problem statement, current/desired state, impact assessment, constraints, DACI
3. Paste Triage Result into the Links section
4. Set status to "Draft"

**Output**: Problem Brief page in Confluence.

**Quality check** (GPM):
- Problem statement follows the structured format?
- Impact is quantified?
- DACI roles assigned?
- Tier seems correct?

---

### Step 4: Discovery (GPT Mode 2)

**Where**: Custom GPT.

**Who**: PM (with tech lead and designer available for questions).

**Applies to**: Required for Tier 3. Optional but recommended for Tier 2.

**Input**: Problem Brief (PM describes or pastes content into GPT).

**Process**:
1. GPT runs 4-stage questioning: Problem Validation → Scope & Boundaries → Assumptions & Risks → Success Criteria
2. GPT challenges assumptions, redirects solution-talk, demands evidence
3. PM may need to pause and gather information between stages
4. GPT produces a structured Discovery Summary

**Output**: Discovery Summary with problem statement, V1 scope, assumptions (rated), success criteria, open items.

**Confluence update**:
1. Create Assumption Map page from template, populate with assumptions from Discovery
2. Update Problem Brief status to "In Discovery" or "Ready for Spec"
3. Create Decision Records for any decisions made during Discovery

---

### Step 5: Spec Readiness Assessment (GPT Mode 3)

**Where**: Custom GPT.

**Who**: PM.

**Applies to**: Tier 2 and Tier 3.

**Input**: Discovery Summary (or Problem Brief if Discovery was skipped for Tier 2).

**Process**:
1. GPT scores 20 checklist items: Red/Yellow/Green
2. GPT evaluates readiness: Ready / Almost Ready / Not Ready
3. If ready: GPT generates a draft `/speckit.specify` prompt
4. If not ready: GPT provides specific remediation steps

**Output**:
- Scored checklist
- Recommendation
- Draft `/speckit.specify` prompt (if passing)

**PM actions**:
- If Not Ready: resolve red items, return to GPT
- If Ready: review and edit the draft prompt, then proceed to Step 6

---

### Step 6: Specification (GitHub spec-kit)

**Where**: GitHub repository with spec-kit.

**Who**: PM (with tech lead review).

**Input**: Edited `/speckit.specify` prompt from GPT.

**Process**:
1. PM runs `/speckit.specify` with the (edited) prompt
2. spec-kit generates a specification document in `.specify/specs/`
3. PM reviews the spec — edits for accuracy, adds detail where needed
4. Tech lead reviews for technical feasibility and constitution compliance
5. Compliance lead reviews the Compliance Impact Assessment section (for features touching financial data)

**Output**: Approved specification in `.specify/specs/`.

**Confluence update**: Update Problem Brief status to "Specifying", add spec link.

---

### Step 7: Planning (GitHub spec-kit)

**Where**: GitHub repository.

**Who**: Tech lead (with PM input).

**Input**: Approved specification.

**Process**:
1. Tech lead runs `/speckit.plan` referencing the spec
2. spec-kit generates a plan document in `.specify/plans/`
3. Tech lead reviews: architecture, data model, test strategy, compliance mapping
4. Compliance checkpoint: compliance lead reviews SOX controls, audit events, approval workflows
5. Architecture review for Tier 3 features

**Output**: Approved plan in `.specify/plans/`.

---

### Step 8: Implementation

**Where**: GitHub repository.

**Who**: Development team.

**Input**: Approved plan and spec.

**Process**:
1. Run `/speckit.tasks` to generate work items
2. Implement in small increments, each reviewed against spec and plan
3. Write tests per the plan's test strategy
4. Code review checks constitution compliance

**Output**: Working code with tests.

---

### Step 9: Validation

**Where**: CI/CD pipeline and review process.

**Who**: Tech lead, PM, QA.

**Process**:
1. Automated tests pass (unit, integration, contract, acceptance)
2. PM validates acceptance criteria against spec
3. Compliance review confirms audit events and approval workflows work correctly
4. Production readiness review (observability, rollback plan)

**Output**: Validated, deployable code.

---

### Step 10: Release & Outcomes

**Where**: Production → Confluence.

**Who**: PM, Tech lead.

**Process**:
1. Deploy to production
2. Monitor: observe metrics, logs, traces
3. Record outcomes in Confluence:
   - Update Problem Brief status to "Shipped"
   - Record actual vs. expected metrics
   - Update Portfolio Dashboard
   - Update Product Capability Map (if new capability)
   - Create/update Capability Record

**Output**: Closed loop — outcomes feed back into institutional memory.

---

## Artifact Relationships

```
Problem Brief (Confluence)
├── Triage Result (from GPT Mode 1)
├── Assumption Map (Confluence)
│   └── Fed by GPT Mode 2 Discovery
├── Decision Records (Confluence)
├── Spec Readiness Assessment (from GPT Mode 3)
│   └── Draft /speckit.specify prompt
├── Specification (GitHub .specify/specs/)
│   ├── References Constitution articles
│   └── Includes Compliance Impact Assessment
├── Plan (GitHub .specify/plans/)
│   ├── Implements Specification
│   ├── Includes Compliance Review Checkpoint
│   └── Includes OpenAPI Specification
└── Tasks (GitHub .specify/tasks/)
    └── Derived from Plan
```

---

## Compliance Touchpoints

Compliance review is required at these points:

| Step | Compliance Action | Who |
|------|------------------|-----|
| Triage | Identify regulatory flag | PM (GPT assists) |
| Discovery | Surface compliance risks in Assumption Map | PM |
| Specification | Complete Compliance Impact Assessment | PM, Compliance Lead reviews |
| Planning | Complete Compliance Review Checkpoint, SOX Control Mapping | Tech Lead, Compliance Lead reviews |
| Implementation | Implement audit events and approval workflows | Dev team |
| Validation | Verify audit trail completeness, test approval workflows | QA, Compliance Lead |
| Release | Production compliance checklist | Tech Lead, Compliance Lead |

---

## Retrofitting Existing Capabilities

For capabilities already in production without documentation:

1. **Build inventory**: Populate the Product Capability Map with all known capabilities
2. **Prioritize**: Focus on capabilities with upcoming investment, poor performance, or knowledge flight risk
3. **Document**: Each PM documents 2-3 Capability Records per quarter using the template
4. **Connect**: Iteration opportunities from Capability Records become Problem Briefs in the main lifecycle

See the Product Capability Map and Capability Record templates in Confluence.

---

## Quick Reference: "What Do I Do When..."

| Situation | Action |
|-----------|--------|
| Someone asks for a small config change | GPT Triage → likely Tier 1 → Jira ticket |
| Someone requests a new feature | GPT Triage → Problem Brief → Discovery (if Tier 3) → Spec Readiness → spec-kit |
| I need to validate my thinking on a Problem Brief | GPT Mode 2 (Discovery) |
| I think my idea is ready for spec-kit | GPT Mode 3 (Spec Readiness) → get the draft prompt |
| I need to make a decision between options | Create a Decision Record in Confluence |
| I need to document an existing capability | Use the Capability Record template |
| The GPM asks about portfolio status | Portfolio Dashboard in Confluence |
| A feature has compliance implications | Flag it in Triage, include in Assumption Map, complete Compliance Impact Assessment in Spec |
