# SDD Methodology Summary

> Concise reference on Spec-Driven Development for the SDD Companion GPT. For full research, see the project's research archive.

---

## What Is SDD?

**Spec-Driven Development (SDD)** is a methodology where a formal specification serves as the authoritative source of truth. Code is a generated or verified secondary artifact. The specification declares intent; the code realizes it.

> "Code is the implementation detail of the specification — not the other way around."

---

## Three Specification Levels

| Level | Description | Spec-Code Relationship | Best For |
|-------|-------------|----------------------|----------|
| **Spec-First** | Specs written before implementation; code becomes primary afterward | Spec may drift post-implementation | Prototypes, initial AI-assisted dev |
| **Spec-Anchored** | Specs maintained alongside code throughout lifecycle | Automated tests enforce alignment | Production systems, long-term maintenance |
| **Spec-as-Source** | Only specs edited by humans; code entirely generated | Drift eliminated by construction | API stubs, model-driven systems |

**Our target**: Spec-Anchored. Specs are living documents maintained alongside code, with automated contract testing to prevent drift.

---

## Four Workflow Phases

### 1. SPECIFY — "What should the software do?"
- Functional specifications, acceptance criteria, behavioral requirements
- Given/When/Then format where applicable
- Behavior-focused, testable, unambiguous
- Uses domain-specific ubiquitous language

### 2. PLAN — "How should we build it?"
- Technical architecture, data models, interfaces, technology choices
- Bridges "what" (spec) and "how" (implementation)
- Encodes implementation constraints and non-functional requirements

### 3. IMPLEMENT — "Build it."
- Working code realizing the spec according to the plan
- Small, validated increments
- Code reviewed against both spec and plan

### 4. VALIDATE — "Does the code meet the spec?"
- Automated tests: unit, integration, acceptance, contract
- Spec remains the authority; violations are addressed
- Decision: fix code OR revise spec based on evidence

---

## Constitutional SDD (CSDD)

An extension of SDD that adds a **constitution** — a set of non-negotiable principles that every specification must satisfy.

**Constitution encodes**:
- Architectural constraints (e.g., API-first, no direct DB access)
- Security requirements (e.g., authentication, authorization, audit)
- Compliance mandates (e.g., SOX audit trails, data retention)
- Engineering standards (e.g., TDD, backwards compatibility)

**Why it works**: Specs reference constitution articles, ensuring every feature inherits organizational principles. AI agents can check generated code against constitutional constraints.

**Empirical results** (banking case study): 73% reduction in CWE violations, 56% faster time to secure build, 4.3x compliance documentation coverage.

---

## SDD + AI Agents

- Specs serve as **"super-prompts"** — structured, unambiguous input for AI code generation
- Error reductions up to 50% compared to unstructured prompts
- Enables parallel agent execution on non-overlapping spec sections
- Prevents "vibe coding" where loose prompts force AI to guess requirements
- Constitution provides guardrails AI agents cannot violate

---

## GitHub Spec Kit

The tooling we use for executable specifications.

**Three commands**:
1. `/speckit.specify` — Define what the software should do (requirements, acceptance criteria)
2. `/speckit.plan` — Define how to build it (architecture, implementation approach)
3. `/speckit.tasks` — Break the plan into executable work items

**Key concepts**:
- Constitution document defines non-negotiable principles
- Specs live in `.specify/` directory alongside code
- Plans reference specs and constitution articles
- Tasks are derived from plans with clear acceptance criteria

---

## Key Principles for This Workflow

1. **Specs are the source of truth** — not Jira tickets, not Slack messages, not meeting notes
2. **Constitution before specification** — principles are established before features are specified
3. **Human review at every transition** — AI assists but humans approve
4. **Iterative refinement** — specs improve through discovery, not through implementation surprises
5. **Compliance by construction** — bake regulatory requirements into the process, don't bolt them on
