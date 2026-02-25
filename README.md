# SDD Workflow System — JPMC Product Catalog

A closed-loop product development system combining **Confluence** (institutional memory), a **Custom GPT** (thinking sharpener), and **GitHub spec-kit** (executable specifications). Built for the Product Catalog team managing taxonomy, billing codes, and product configurations.

---

## How It Works

```
Problem → Triage (GPT) → Problem Brief (Confluence) → Discovery (GPT)
→ Spec Readiness (GPT) → /speckit.specify (GitHub) → Development → Release
→ Outcomes (Confluence) → loop
```

The GPT has three modes:
1. **Triage** — Classify requests into Tier 1 (config change), Tier 2 (enhancement), or Tier 3 (new capability)
2. **Discovery** — Structured critical questioning across 4 stages: problem validation, scope, assumptions, success criteria
3. **Spec Readiness** — 20-item checklist scored Red/Yellow/Green. If passing, generates a draft `/speckit.specify` prompt

Data flows between systems via copy-paste. This is deliberate — regulated environments require human review at every transition.

---

## Repository Structure

### [`gpt/`](gpt/) — Custom GPT Configuration
| File | Description |
|------|-------------|
| [`system-prompt.md`](gpt/system-prompt.md) | 4-layer GPT system prompt (Identity, Mode Router, Mode Instructions, Guardrails) |
| [`gpt-setup-guide.md`](gpt/gpt-setup-guide.md) | Step-by-step Custom GPT creation instructions |
| [`knowledge/product-catalog-domain.md`](gpt/knowledge/product-catalog-domain.md) | Domain model: entities, relationships, regulatory context |
| [`knowledge/sdd-methodology.md`](gpt/knowledge/sdd-methodology.md) | SDD methodology summary |
| [`knowledge/speckit-reference.md`](gpt/knowledge/speckit-reference.md) | spec-kit commands and conventions |
| [`knowledge/triage-criteria.md`](gpt/knowledge/triage-criteria.md) | Tier 1/2/3 definitions and classification axes |
| [`knowledge/discovery-question-bank.md`](gpt/knowledge/discovery-question-bank.md) | 4-stage critical questioning library (23 questions) |
| [`knowledge/spec-readiness-checklist.md`](gpt/knowledge/spec-readiness-checklist.md) | 20-item readiness assessment rubric |

### [`confluence/`](confluence/) — Confluence Templates & Reference
| File | Description |
|------|-------------|
| [`confluence-setup-guide.md`](confluence/confluence-setup-guide.md) | Confluence space setup instructions |
| [`templates/problem-brief.md`](confluence/templates/problem-brief.md) | Problem Brief template (structured problem capture) |
| [`templates/assumption-map.md`](confluence/templates/assumption-map.md) | Assumption Map template (2x2 grid, validation tracking) |
| [`templates/decision-record.md`](confluence/templates/decision-record.md) | Decision Record template (DACI-based, immutable) |
| [`templates/capability-record.md`](confluence/templates/capability-record.md) | Capability Record template (for retrofitting existing features) |
| [`templates/product-capability-map.md`](confluence/templates/product-capability-map.md) | Product Capability Map (full surface area inventory) |
| [`templates/portfolio-dashboard.md`](confluence/templates/portfolio-dashboard.md) | Portfolio Dashboard (GPM operational view) |
| [`reference/lifecycle-guide.md`](confluence/reference/lifecycle-guide.md) | Full process reference: how all three systems connect |
| [`reference/triage-decision-tree.md`](confluence/reference/triage-decision-tree.md) | Structured triage flowchart |

### [`speckit/`](speckit/) — spec-kit Constitution & Overrides
| File | Description |
|------|-------------|
| [`constitution.md`](speckit/constitution.md) | 9-article software constitution (API-first, compliance, testing, etc.) |
| [`constitution-guide.md`](speckit/constitution-guide.md) | Article explanations, FAQ, amendment process |
| [`overrides/spec-template.md`](speckit/overrides/spec-template.md) | Spec template with Compliance Impact, API Preview, Consumer Impact sections |
| [`overrides/plan-template.md`](speckit/overrides/plan-template.md) | Plan template with Compliance Checkpoint, OpenAPI, Migration Plan sections |

### [`playbook/`](playbook/) — Rollout & Operations
| File | Description |
|------|-------------|
| [`system-architecture.md`](playbook/system-architecture.md) | Architecture diagram and three-pillar explanation |
| [`rollout-plan.md`](playbook/rollout-plan.md) | 10-week rollout sequence |
| [`metrics-and-kpis.md`](playbook/metrics-and-kpis.md) | 8 metrics for measuring SDD success |
| [`facilitator-guide.md`](playbook/facilitator-guide.md) | Guide for running Triage, Discovery, and Spec Readiness sessions |

### [`examples/`](examples/) — Worked Example: "Bulk Import of Billing Codes"
| File | Description |
|------|-------------|
| [`example-problem-brief.md`](examples/example-problem-brief.md) | Filled-in Problem Brief |
| [`example-discovery-session.md`](examples/example-discovery-session.md) | Simulated GPT Discovery transcript |
| [`example-spec-readiness.md`](examples/example-spec-readiness.md) | Scored 20-item checklist |
| [`example-specify-prompt.md`](examples/example-specify-prompt.md) | Generated `/speckit.specify` prompt |

---

## Getting Started

### For the GPM (setup)

1. Read the [System Architecture](playbook/system-architecture.md) to understand the three pillars
2. Follow the [Confluence Setup Guide](confluence/confluence-setup-guide.md) to create templates and reference pages
3. Follow the [GPT Setup Guide](gpt/gpt-setup-guide.md) to create the Custom GPT
4. Initialize spec-kit with the [Constitution](speckit/constitution.md)
5. Run the [Rollout Plan](playbook/rollout-plan.md) starting Week 1

### For PMs (using the workflow)

1. Read the [Lifecycle Guide](confluence/reference/lifecycle-guide.md) for the full process
2. Start with the GPT: describe your idea → get a triage result
3. For Tier 2/3: create a [Problem Brief](confluence/templates/problem-brief.md)
4. Run Discovery and Spec Readiness through the GPT
5. Use the generated prompt with `/speckit.specify`
6. Review the [examples](examples/) to see what good output looks like

### For Tech Leads

1. Read the [Constitution](speckit/constitution.md) and [Constitution Guide](speckit/constitution-guide.md)
2. Review specs for constitution compliance
3. Run `/speckit.plan` to create implementation plans
4. Use the [Plan Template](speckit/overrides/plan-template.md) for JPMC-specific sections

---

## Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| **GPT is advisory, not automated** | Regulated environment requires human review at every transition |
| **Constitution defines principles, not tools** | No tech stack in the constitution — keeps it durable |
| **Templates are markdown** | Version-controllable, diffable, portable. Convert to Confluence manually. |
| **Manual data transfer between systems** | Copy-paste friction forces humans to read output before using it |
| **Triage uses 3 axes** | Scope + API Impact + Regulatory Weight → comprehensive classification |
| **GPT output = next phase's input** | Mode 3 produces a draft `/speckit.specify` prompt — useful output, not just a gate |

---

## Methodology

This system implements **Spec-Driven Development (SDD)** with a **software constitution** (Constitutional SDD). For background:

- SDD: formal specifications as the source of truth; code is a secondary artifact
- Constitutional SDD: non-negotiable principles every specification must satisfy
- Four phases: SPECIFY → PLAN → IMPLEMENT → VALIDATE

Based on research from:
- Piskala (2026): "Spec-Driven Development: From Code to Contract" (arXiv:2602.00180)
- Marri (2026): "Constitutional SDD: Security by Construction" (arXiv:2602.02584)
