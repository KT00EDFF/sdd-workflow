# System Architecture — Closed-Loop Overview

> High-level architecture of the SDD workflow system: how Confluence, the Custom GPT, and GitHub spec-kit connect to form a closed-loop development process.

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         SDD WORKFLOW SYSTEM                             │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                      HUMAN LAYER                                │    │
│  │  PM  ·  Tech Lead  ·  Designer  ·  GPM  ·  Compliance Lead    │    │
│  └──┬──────────┬──────────────┬──────────────┬─────────────────┬──┘    │
│     │          │              │              │                 │        │
│     ▼          ▼              ▼              ▼                 ▼        │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │                      SYSTEM LAYER                                │   │
│  │                                                                  │   │
│  │  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐      │   │
│  │  │              │    │              │    │              │      │   │
│  │  │  CONFLUENCE   │◄──►│  CUSTOM GPT  │───►│   SPEC-KIT   │      │   │
│  │  │              │    │              │    │   (GitHub)   │      │   │
│  │  │  Institutional│    │   Thinking   │    │  Executable  │      │   │
│  │  │  Memory      │    │   Sharpener  │    │  Specs       │      │   │
│  │  │              │    │              │    │              │      │   │
│  │  └──────┬───────┘    └──────────────┘    └──────┬───────┘      │   │
│  │         │                                        │              │   │
│  │         │            ┌──────────────┐            │              │   │
│  │         │            │              │            │              │   │
│  │         └───────────►│ DEVELOPMENT  │◄───────────┘              │   │
│  │                      │              │                           │   │
│  │                      └──────┬───────┘                           │   │
│  │                             │                                   │   │
│  │                      ┌──────▼───────┐                           │   │
│  │                      │   RELEASE    │───── Outcomes ──► Confluence│  │
│  │                      └──────────────┘                           │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Three Pillars

### 1. Confluence — Institutional Memory

**Role**: Stores all durable artifacts. The system of record for decisions, problem briefs, capability maps, and outcomes.

**Contains**:
| Artifact | Purpose | Updated When |
|----------|---------|-------------|
| Problem Briefs | Capture and track ideas | Created at Triage, updated through lifecycle |
| Assumption Maps | Track and validate assumptions | Created during Discovery, updated during validation |
| Decision Records | Immutable decision history | Created when decisions are made |
| Portfolio Dashboard | GPM operational view | Weekly |
| Product Capability Map | Full product surface area | Quarterly (retrofit) + when new capabilities ship |
| Capability Records | Documentation of existing capabilities | Quarterly (retrofit target: 2-3 per PM) |

**Integration points**:
- GPT references Confluence artifacts (PM pastes content)
- Specs link back to Problem Briefs
- Outcomes recorded after release

### 2. Custom GPT — Thinking Sharpener

**Role**: Forces structured thinking at three critical moments. Bridges the gap between a raw idea and a well-formed specification.

**Three modes**:
| Mode | Input | Output | Takes ~10 min |
|------|-------|--------|------|
| 1. Triage | Raw idea/request | Tier classification + next steps | 2-5 min |
| 2. Discovery | Problem Brief | Structured assumptions, scope, success criteria | 20-45 min |
| 3. Spec Readiness | Discovery output | Scored checklist + draft `/speckit.specify` prompt | 10-15 min |

**Integration points**:
- Input comes from Confluence (Problem Briefs) or raw requests
- Output goes to Confluence (Assumption Maps) and GitHub (spec-kit prompts)
- Human reviews and edits all GPT output before using it

### 3. GitHub spec-kit — Executable Specifications

**Role**: Converts validated requirements into structured specifications, plans, and tasks. The bridge from "what" to "how" to "do."

**Three commands**:
| Command | Input | Output |
|---------|-------|--------|
| `/speckit.specify` | Requirements + acceptance criteria | Specification document |
| `/speckit.plan` | Specification | Implementation plan |
| `/speckit.tasks` | Plan | Discrete work items |

**Key artifact**: Constitution (9 articles) — non-negotiable principles every spec must satisfy.

**Integration points**:
- Input: draft prompts from GPT (reviewed by PM)
- Specs reference constitution articles
- Plans include compliance checkpoints
- Output feeds development

---

## Data Flow

### Forward Flow (Idea → Code)

```
1. Raw idea
   │
   ▼
2. GPT Triage ──► Tier classification
   │
   ▼
3. Problem Brief (Confluence) ──► Structured problem
   │
   ▼
4. GPT Discovery ──► Assumptions, scope, criteria
   │
   ▼
5. Assumption Map (Confluence) ──► Validated assumptions
   │
   ▼
6. GPT Spec Readiness ──► Draft /speckit.specify prompt
   │
   ▼
7. /speckit.specify ──► Specification
   │
   ▼
8. /speckit.plan ──► Implementation plan
   │
   ▼
9. /speckit.tasks ──► Work items
   │
   ▼
10. Development ──► Working code
```

### Feedback Loop (Outcomes → Memory)

```
Release
   │
   ▼
Monitor outcomes (metrics, incidents, feedback)
   │
   ├──► Update Problem Brief (actual vs. expected)
   ├──► Update Portfolio Dashboard
   ├──► Update/Create Capability Record
   └──► Inform future Discovery sessions
```

---

## Trust Boundaries

Data crossing between systems requires human review:

```
┌────────────────────────────────────────────────────────┐
│                                                        │
│  Confluence ◄────── HUMAN REVIEW ──────► Custom GPT    │
│      ▲               REQUIRED                │         │
│      │                                       │         │
│      │         HUMAN REVIEW                  ▼         │
│      └──────── REQUIRED ──────────────── spec-kit      │
│                                                        │
└────────────────────────────────────────────────────────┘
```

**Why manual transfer?** In a regulated environment:
1. Humans must review all artifacts before they move between systems
2. Copy-paste friction forces people to actually read the output
3. No automated data flow means no risk of unreviewed artifacts entering the spec pipeline
4. Audit trail is the human decision to move an artifact forward

---

## What Each System Does NOT Do

| System | Does NOT |
|--------|----------|
| **Confluence** | Make decisions, generate specs, assess readiness |
| **Custom GPT** | Store data, track status, make final decisions |
| **spec-kit** | Validate requirements, manage portfolio, enforce process |

Each system has a clear, bounded responsibility. Process enforcement is the GPM's job, not the tooling's.

---

## Scaling Considerations

**Current design**: 1 product (Product Catalog), 1 team (~4-5 PMs), 1 GPM.

**If scaling to multiple products**:
- Each product gets its own constitution (principles may differ)
- Confluence space can be shared or separated per product
- GPT system prompt can be versioned per product context
- Portfolio Dashboard aggregates across products

**If scaling to more automation**:
- Phase 1 (current): manual transfer between all systems
- Phase 2 (future): Confluence webhooks notify GPM of status changes
- Phase 3 (future): GPT API integration for programmatic triage (still human-approved)
- Constitution is the governance layer that makes automation safe
