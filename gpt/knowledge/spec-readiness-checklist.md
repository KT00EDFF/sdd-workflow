# Spec Readiness Checklist

> 20-item assessment rubric used by the SDD Companion GPT (Mode 3: Spec Readiness Assessment). Each item is scored Red/Yellow/Green. The checklist determines whether an idea is ready for `/speckit.specify`.

---

## Scoring Guide

| Score | Meaning | Action |
|-------|---------|--------|
| 🟢 **Green** | Fully addressed. Evidence or clarity is sufficient. | Ready for spec |
| 🟡 **Yellow** | Partially addressed. Some gaps but not blocking. | Can proceed with `[NEEDS CLARIFICATION]` markers in spec |
| 🔴 **Red** | Missing or critically insufficient. | Must be resolved before specifying |

**Readiness threshold**:
- **Ready**: 0 Reds, ≤3 Yellows → Generate `/speckit.specify` prompt
- **Almost Ready**: 0 Reds, 4-6 Yellows → Generate prompt with prominent `[NEEDS CLARIFICATION]` markers
- **Not Ready**: Any Reds → Provide specific remediation steps, do not generate prompt

---

## The Checklist

### Problem Clarity (Items 1-4)

**1. Problem statement is specific and evidence-based**
- 🟢 Clear "[User] experiences [problem] when [context], resulting in [impact]" format with evidence
- 🟡 Problem described but vague on impact or evidence
- 🔴 No clear problem statement, or pure solution description

**2. Target user segment is identified**
- 🟢 Specific user role/segment named with context (e.g., "PMs managing FX taxonomy billing codes")
- 🟡 User identified but too broad (e.g., "product managers")
- 🔴 No user identified, or "everyone"

**3. Current workaround is documented**
- 🟢 Specific current process described with pain points
- 🟡 General description of current state
- 🔴 No information about how users handle this today

**4. Impact is quantified**
- 🟢 Measurable impact stated (time, errors, cost, frequency)
- 🟡 Impact described qualitatively but not measured
- 🔴 No impact assessment

### Scope Definition (Items 5-8)

**5. V1 scope is explicitly bounded**
- 🟢 Clear list of what's included AND what's excluded
- 🟡 Inclusions listed but exclusions not addressed
- 🔴 Scope is open-ended or not defined

**6. API surface is sketched**
- 🟢 Endpoints, methods, and high-level request/response described
- 🟡 API interaction described conceptually but not as endpoints
- 🔴 No API surface consideration

**7. System interactions are mapped**
- 🟢 Upstream/downstream systems identified with impact
- 🟡 Some systems mentioned but interaction unclear
- 🔴 No system interaction analysis

**8. Backward compatibility impact assessed**
- 🟢 Breaking vs. non-breaking changes identified, migration considered
- 🟡 Acknowledged but not analyzed in detail
- 🔴 Not considered, or feature has obvious compatibility implications that are unaddressed

### Assumptions & Risks (Items 9-12)

**9. Key assumptions are documented**
- 🟢 Assumptions listed with confidence and impact ratings
- 🟡 Some assumptions identified but not rated
- 🔴 No assumptions documented (everything presented as fact)

**10. High-risk assumptions have validation plans**
- 🟢 Each high-risk assumption has a specific validation approach
- 🟡 Validation plans exist for some but not all high-risk assumptions
- 🔴 No validation plans for high-risk assumptions

**11. Compliance risks are identified**
- 🟢 SOX/audit/approval implications explicitly assessed
- 🟡 Compliance mentioned but not analyzed
- 🔴 Compliance not considered for a feature that touches financial data

**12. Rollback/failure scenarios considered**
- 🟢 Failure modes identified with mitigation strategies
- 🟡 Some failure scenarios mentioned
- 🔴 No failure scenario analysis

### Success Criteria (Items 13-16)

**13. Success metrics are measurable**
- 🟢 Specific, measurable outcomes defined (e.g., "reduce time from 4h to 15min")
- 🟡 Outcomes described but not measurable
- 🔴 No success metrics, or vanity metrics only

**14. Acceptance criteria are testable**
- 🟢 Given/When/Then scenarios for happy path and key edge cases
- 🟡 Acceptance criteria described but not in testable format
- 🔴 No acceptance criteria

**15. Performance constraints are stated**
- 🟢 Response time, throughput, and data volume limits specified
- 🟡 Performance mentioned but not quantified
- 🔴 No performance requirements for a feature with obvious performance implications

**16. Non-functional requirements addressed**
- 🟢 Availability, scalability, security, auditability explicitly covered
- 🟡 Some non-functional requirements mentioned
- 🔴 Non-functional requirements not considered

### Technical & Stakeholder Readiness (Items 17-20)

**17. Tech lead has assessed feasibility**
- 🟢 Tech lead confirmed feasibility and identified technical risks
- 🟡 Informal tech discussion but no formal assessment
- 🔴 No technical feasibility check

**18. Domain model impact understood**
- 🟢 New/modified entities, fields, and relationships documented
- 🟡 Domain impact described conceptually
- 🔴 Domain model impact not considered

**19. DACI roles assigned**
- 🟢 Driver, Approver, Contributors, Informed all named
- 🟡 Some roles assigned
- 🔴 No role assignment

**20. Stakeholder alignment confirmed**
- 🟢 Key stakeholders briefed and aligned
- 🟡 Some stakeholder discussions held
- 🔴 No stakeholder engagement

---

## Output Format

The GPT produces a scored checklist followed by a summary and recommendation:

```
## Spec Readiness Assessment: [Feature Name]

### Scores

| # | Item | Score | Notes |
|---|------|-------|-------|
| 1 | Problem statement | 🟢 | Clear problem with evidence from support tickets |
| 2 | Target user segment | 🟢 | "PMs managing FX taxonomy billing codes" |
| 3 | Current workaround | 🟢 | Manual one-by-one entry described |
| ... | ... | ... | ... |

### Summary
- **Green**: 14/20
- **Yellow**: 4/20
- **Red**: 2/20

### Recommendation
**Not Ready** — 2 items require resolution before specifying:
- Item 11 (Compliance risks): Feature creates billing codes — SOX implications must be assessed
- Item 17 (Tech feasibility): No tech lead input on CSV parsing at scale

### Remediation Steps
1. Schedule 30-min session with compliance lead to assess SOX implications
2. Get tech lead confirmation on CSV processing limits and error handling approach
3. Re-run assessment after resolving red items

### [If Ready] Draft /speckit.specify Prompt
[Only included if threshold is met — see spec-readiness-to-specify bridge below]
```

---

## Spec Readiness → `/speckit.specify` Bridge

When the checklist passes (0 Reds, ≤3 Yellows), the GPT generates a draft `/speckit.specify` prompt that:

1. **Opens with the problem** (from Items 1-4)
2. **Defines requirements** (from Items 5-8, including API surface)
3. **Lists acceptance criteria** (from Items 13-16, in Given/When/Then format)
4. **States constraints** (from Items 9-12, including compliance requirements)
5. **References constitution articles** (based on feature type — see constitution-guide.md quick reference)
6. **Marks open items** with `[NEEDS CLARIFICATION]` for Yellow items
7. **Provides domain context** (relevant entities from the domain model)

The PM reviews and edits this prompt before running it through spec-kit. The GPT's draft is a starting point, not a finished product.
