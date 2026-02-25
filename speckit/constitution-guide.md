# Constitution Guide — Article Explanations & Amendment Process

> This guide explains each constitution article in plain language and documents how to propose amendments.

---

## How to Read the Constitution

Each article follows a consistent structure:


| Section          | Purpose                                                                        |
| ---------------- | ------------------------------------------------------------------------------ |
| **Title**        | The principle name — this is what you reference in specs (e.g., "Article III") |
| **Summary**      | One-sentence statement of the non-negotiable requirement                       |
| **Requirements** | Specific, testable rules that implementations must satisfy                     |
| **Rationale**    | Why this principle exists — the business/regulatory reason                     |
| **Enforcement**  | How compliance is verified — what will fail if you violate this                |


**Key language**: "MUST" means non-negotiable. "SHOULD" means strongly recommended with documented exceptions. "MAY" means optional. This follows RFC 2119 conventions.

---

## Article-by-Article Explanations

### Article I: API-First Principle

**In plain language**: If it's not accessible through a documented API, it doesn't exist. No backdoors, no "just query the database" shortcuts.

**Why this matters for your specs**: Every feature you specify must describe the API surface — endpoint, method, request/response schemas. If you can't describe the API, the feature isn't ready to spec.

**Common questions**:

- *"Can we use GraphQL?"* — Not currently. REST/OpenAPI 3.1 is the standard. Proposing GraphQL would require a constitution amendment.
- *"What about internal-only endpoints?"* — Still require OpenAPI documentation. Internal doesn't mean undocumented.
- *"Can we add a database view for reporting?"* — No direct database access. Expose a reporting API endpoint instead.

---

### Article II: Domain Integrity

**In plain language**: Use the vocabulary defined in the domain model. Don't invent new terms for existing concepts. Don't modify immutable things.

**Why this matters for your specs**: If your spec mentions a "product type" but the domain model calls it "ProductCategory," you're introducing confusion. Use canonical names.

**Common questions**:

- *"Can I add a new field to an entity?"* — Yes, propose it in your spec. It'll be reviewed against the domain model.
- *"Can I rename a taxonomy node?"* — No. Deprecate the old node, create a new one. This preserves historical references.
- *"What if the domain model is wrong?"* — Propose a domain model amendment alongside your spec. Both get reviewed together.

---

### Article III: Compliance by Design

**In plain language**: Every change leaves a permanent, traceable record. Financial operations require two people to agree.

**Why this matters for your specs**: Your spec must identify which operations are SOX-regulated (hint: anything touching billing codes or product lifecycle transitions in production). The compliance impact assessment isn't optional — it's how we stay out of regulatory trouble.

**Common questions**:

- *"Does a read-only endpoint need audit events?"* — No. Only state-changing operations (create, update, delete, status transitions).
- *"What's a correlation ID?"* — A UUID that connects all the events and logs from a single business operation. If a PM uploads 100 billing codes, they all share one correlation ID.
- *"Is the maker-checker pattern required for UAT?"* — Only for PROD. UAT has relaxed approval workflows.

---

### Article IV: Test-First Development

**In plain language**: Write tests before or alongside your code. No shipping untested features.

**Why this matters for your specs**: Acceptance criteria in your spec become acceptance tests. If your criteria are vague, the tests will be vague (or nonexistent). Write them in Given/When/Then format to make them directly translatable.

**Common questions**:

- *"Why can't I mock the database?"* — Mocks hide bugs. A test that mocks the data layer tests your mock, not your code. Use test containers for realistic database testing.
- *"80% coverage seems high."* — It's 80% on new/changed code, not the entire codebase. Focus coverage on business logic, not boilerplate.
- *"Do I need contract tests for internal APIs?"* — Yes. Internal consumers break just as easily as external ones.

---

### Article V: Backwards Compatibility

**In plain language**: Don't break things that other teams depend on. If you must change, give them time and a migration path.

**Why this matters for your specs**: If your feature changes an existing API, your spec must include a Downstream Consumer Impact section. Identify who's affected and what they need to do.

**Common questions**:

- *"Can I add a new optional field?"* — Yes, additive changes to response bodies are non-breaking. Adding required fields to request bodies is breaking.
- *"How long is a deprecation period?"* — Minimum 2 release cycles. For widely-used endpoints, expect longer.
- *"What if the breaking change is urgent?"* — Document it as an emergency change with an expedited migration plan. It still needs consumer communication.

---

### Article VI: Schema Governance

**In plain language**: Database changes are versioned, reversible, and never destroy data.

**Why this matters for your specs**: If your feature requires new tables or columns, your plan must include migration scripts. If it touches taxonomy, remember: append-only.

**Common questions**:

- *"Can I drop a column that's definitely unused?"* — Only after verifying no consumer reads it (including reporting queries). Requires DBA review and a deprecation period.
- *"What about soft deletes vs. hard deletes?"* — Always soft deletes in this domain. Financial data has 7-year retention requirements.
- *"Can I rename a column?"* — Treat it as adding a new column + deprecating the old one. Never rename in place.

---

### Article VII: Simplicity / YAGNI

**In plain language**: Build what the spec says. Nothing more.

**Why this matters for your specs**: If the spec doesn't require it, don't build it. If you discover a need during implementation, update the spec first — don't just add it.

**Common questions**:

- *"Can I add a feature flag for optional behavior?"* — Only if the spec requires variability. Don't add flags "just in case."
- *"Should I build a plugin system for future extensibility?"* — Not unless the spec calls for it. Build the concrete thing.
- *"What about error handling for edge cases?"* — Handle documented edge cases. Don't build elaborate error handling for scenarios that are theoretically possible but practically impossible.

---

### Article VIII: Observability

**In plain language**: Make the system debuggable. Logs, health checks, and traces are not optional.

**Why this matters for your specs**: When you specify a new service or endpoint, you're also specifying its observability surface. Your plan should identify what to log, what to trace, and what to alert on.

**Common questions**:

- *"Is structured logging really necessary for every service?"* — Yes. Unstructured logs are unsearchable at scale.
- *"What trace context format?"* — W3C Trace Context. It's the standard.
- *"Who defines alert thresholds?"* — The tech lead, informed by the spec's SLA/performance requirements.

---

### Article IX: Security Boundaries

**In plain language**: Authenticate everyone, authorize everything, store secrets safely.

**Why this matters for your specs**: If your feature introduces a new role or permission, your spec must describe the RBAC changes. If it integrates with a new external system, your plan must describe the authentication pattern.

**Common questions**:

- *"Can I store an API key in an environment variable?"* — Not if it's committed to version control. Use the secrets vault.
- *"Do internal APIs need authentication?"* — Yes. Zero-trust architecture means no implicit trust based on network location.
- *"Can I use a service account with broad permissions?"* — Only if scoped to the minimum required. Document the permissions in the plan.

---

## Amendment Process

The constitution is a living document. It evolves as the Product Catalog domain evolves. But amendments require deliberation — these are foundational principles, not features.

### When to Propose an Amendment

- A principle is blocking valid work without providing proportionate value
- A new regulatory requirement demands a new principle
- A principle is ambiguous and teams are interpreting it inconsistently
- Technology changes make a principle obsolete or require a new one

### How to Propose an Amendment

1. **Draft**: Write the proposed change in the same format as existing articles (statement, requirements, rationale, enforcement)
2. **Sponsor**: Get a tech lead and a PM to co-sponsor the amendment
3. **Review**: Present at the architecture review meeting with:
  - The problem the current constitution creates
  - The proposed change
  - Impact analysis: which existing specs/code would be affected
4. **Decision**: Architecture review votes (requires majority + GPM approval)
5. **Implementation**: If approved, update the constitution and propagate to all active specs

### Amendment History


| Version | Date       | Change               | Rationale                               |
| ------- | ---------- | -------------------- | --------------------------------------- |
| 1.0     | 2026-02-24 | Initial constitution | Baseline principles for Product Catalog |


---

## Quick Reference: Which Articles Apply?


| If your feature...              | Check these articles                                                 |
| ------------------------------- | -------------------------------------------------------------------- |
| Adds a new API endpoint         | I (API-First), IV (Test-First), VIII (Observability)                 |
| Changes billing codes           | II (Domain Integrity), III (Compliance), VI (Schema Governance)      |
| Modifies product lifecycle      | II (Domain Integrity), III (Compliance), V (Backwards Compatibility) |
| Introduces new entity types     | II (Domain Integrity), VI (Schema Governance)                        |
| Changes database schema         | VI (Schema Governance), V (Backwards Compatibility)                  |
| Adds new user roles             | IX (Security Boundaries), III (Compliance)                           |
| Integrates with external system | IX (Security), VIII (Observability), I (API-First)                   |
| Modifies taxonomy               | II (Domain Integrity — immutability!), VI (Schema Governance)        |


