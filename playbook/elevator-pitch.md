# SDD Workflow — Elevator Pitch

**Why**: We build first, document never. Decisions live in people's heads. Specs — when they exist — are stale by sprint two.

**What**: Spec-Driven Development. The spec isn't documentation. It's the source of truth.

**The maturity path**:
- **Today → Spec-first.** Define *what* and *why* before anyone codes. Specs guide development.
- **Next → Spec-anchored.** Specs stay current. Living docs that evolve with the product — not artifacts that rot in Confluence.
- **Goal → Spec-as-source.** The spec *is* the system of record. Change the spec, regenerate the code.

**The backward play**: We don't just go forward. We go backward too. Existing undocumented capabilities get reverse-engineered into specs — code to spec. Once we have specs, we can refactor *from* them — spec back to better code. That's the full loop: **spec → code** for new features, **code → spec → code** for legacy. Every capability documented. Every refactor grounded in a formal definition, not gut feel.

**Who**: PMs, tech leads, and design. A Custom GPT for structured thinking, Confluence for memory, GitHub spec-kit (open source) for specs.

**When**: 3 weeks. Setup, pilot, team hackathon. Everyone hands-on with a real idea by day 15.

**How we'll know**: Fewer mid-sprint surprises. Less last-minute gotchas during testing. And an institutional record of *why* our product works the way it does — one that survives when people leave.
