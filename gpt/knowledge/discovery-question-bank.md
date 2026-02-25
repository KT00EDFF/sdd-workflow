# Discovery Question Bank

> Four-stage critical questioning library used by the SDD Companion GPT (Mode 2: Discovery Companion). The GPT selects questions contextually — it does not ask every question in order.

---

## How Discovery Works

Discovery is a **directed conversation** where the GPT acts as a critical thinking partner. It actively challenges the PM's assumptions, redirects solution-talk to problem-talk, and surfaces unknowns.

**Stance**: Directive, not passive. The GPT doesn't just ask questions — it pushes back, flags gaps, and demands evidence.

**Key behaviors**:
- Recognize when the PM is describing a solution and redirect: *"That's a solution. Let's back up — what's the problem it solves?"*
- Distinguish convictions from evidence: *"That sounds like a conviction. What evidence supports it?"*
- Flag missing perspectives: *"You've described the PM view. What would the tech lead say? The end user?"*
- Acknowledge when something is well-validated: *"That's a strong signal. Let's capture it and move on."*

---

## Stage 1: Problem Validation — "Is this real?"

**Goal**: Confirm the problem is real, significant, and worth solving. Kill ideas that fail this gate.

### Core Questions

1. **"Who specifically experiences this problem?"**
   - Reject: "our users" / "the business" / "everyone"
   - Accept: "PMs managing 50+ billing codes in the FX taxonomy" / "the billing operations team during quarter-end reconciliation"

2. **"How do they experience the problem today?"**
   - Looking for: current workaround description, frequency, pain intensity
   - Follow-up: *"Walk me through the last time this happened. What did they actually do?"*

3. **"What evidence do you have that this is a real problem?"**
   - Strong signals: support tickets, user interviews, metrics, direct observation
   - Weak signals: "I think...", "it makes sense that...", "we got a request from..."
   - Challenge: *"One request isn't a pattern. How many times has this come up?"*

4. **"What happens if we don't solve this?"**
   - Looking for: quantified impact (time wasted, errors caused, revenue at risk)
   - Challenge vague impact: *"'It's slow' isn't impact. How slow? For how many people? How often?"*

5. **"Why hasn't this been solved before?"**
   - Looking for: was it deprioritized (why?), is it newly relevant, is it harder than it looks?
   - Red flag: "We tried before and it didn't work" → *"What specifically failed? Has anything changed?"*

### Stage 1 Exit Criteria
- [ ] Specific user segment identified
- [ ] Current workaround described
- [ ] Evidence beyond a single anecdote
- [ ] Impact quantified (even roughly)
- [ ] No prior failed attempts that haven't been accounted for

---

## Stage 2: Scope & Boundaries — "What are we NOT doing?"

**Goal**: Draw a clear boundary around the solution space. Prevent scope creep before it starts.

### Core Questions

6. **"What's the smallest version of this that would still solve the problem?"**
   - Forces prioritization and V1 thinking
   - Challenge: *"You listed 8 requirements. Which 3 are non-negotiable for V1?"*

7. **"What are we explicitly NOT doing?"**
   - Looking for: anti-requirements, out-of-scope features
   - Prompt if blank: *"If someone asked 'does it also do X?', what are the top 3 things where the answer is no?"*

8. **"Which existing system capabilities does this interact with?"**
   - Looking for: entity relationships, API dependencies, data flows
   - Follow-up: *"You mentioned billing codes. Does this change how existing billing codes work, or just add new ones?"*

9. **"What downstream systems would be affected?"**
   - Looking for: consumer impact awareness (billing engine, client portal, reporting)
   - Challenge: *"Have you talked to the billing engine team about this?"*

10. **"What's the API surface of this feature?"**
    - Looking for: endpoints, methods, request/response shapes
    - Challenge: *"You described a UI workflow. What's happening at the API level?"*

11. **"Is this a change to an existing capability or a new capability?"**
    - Determines: spec approach, backwards compatibility needs, consumer impact
    - Follow-up: *"If it's new, does it need to coexist with the old behavior during transition?"*

### Stage 2 Exit Criteria
- [ ] V1 scope explicitly defined
- [ ] Anti-requirements listed (what's NOT included)
- [ ] System interactions identified
- [ ] Downstream impact assessed
- [ ] API surface sketched (even roughly)

---

## Stage 3: Assumptions & Risks — "What could go wrong?"

**Goal**: Surface hidden assumptions and assess risks. This stage feeds directly into the Assumption Map in Confluence.

### Core Questions

12. **"What are you assuming is true that, if wrong, would invalidate this whole idea?"**
    - The "killer assumption" question
    - Example: *"You're assuming PMs will actually use a CSV template. What if they won't?"*

13. **"What assumptions are you making about user behavior?"**
    - Looking for: adoption assumptions, workflow assumptions, frequency assumptions
    - Challenge: *"You said PMs will do this weekly. Based on what?"*

14. **"What assumptions are you making about technical feasibility?"**
    - Looking for: performance assumptions, integration assumptions, data quality assumptions
    - Follow-up: *"Has the tech lead confirmed this is feasible within the current architecture?"*

15. **"What are the compliance risks?"**
    - Looking for: SOX implications, audit trail requirements, approval workflow changes
    - Prompt: *"Does this create, modify, or deactivate any financial data? If yes, what audit implications?"*

16. **"What's the rollback plan if this goes wrong in production?"**
    - Looking for: reversibility awareness
    - Challenge: *"If this creates bad billing codes, how do you undo it?"*

17. **"Rate each assumption: how confident are you (high/medium/low) and how much does it matter (high/medium/low)?"**
    - Creates the 2x2 assumption matrix:
      - **High confidence, High impact**: Monitor
      - **Low confidence, High impact**: Validate immediately (these are blockers)
      - **High confidence, Low impact**: Accept
      - **Low confidence, Low impact**: Accept with caveat

### Stage 3 Exit Criteria
- [ ] Killer assumptions identified
- [ ] Each assumption rated (confidence × impact)
- [ ] High-risk assumptions have validation plans
- [ ] Compliance risks flagged
- [ ] Rollback scenario considered

---

## Stage 4: Success Criteria & Constraints — "How will we know it works?"

**Goal**: Define measurable success criteria and bridge to Spec Readiness. This is the final stage before the PM moves to Mode 3.

### Core Questions

18. **"How will you measure whether this succeeded?"**
    - Reject: vanity metrics ("more users"), unmeasurable outcomes ("better experience")
    - Accept: "Time to create 50 billing codes drops from 4 hours to 15 minutes" / "Zero validation errors in bulk imports after 30 days"

19. **"What does 'done' look like for V1?"**
    - Looking for: concrete acceptance criteria (Given/When/Then if possible)
    - Challenge: *"You said 'it should work.' What specifically does 'work' mean? Walk me through a happy path."*

20. **"What are the performance constraints?"**
    - Looking for: response time expectations, throughput requirements, data volume limits
    - Example: *"If someone uploads a CSV with 500 rows, what's the acceptable processing time?"*

21. **"What are the non-functional requirements?"**
    - Prompts: availability, scalability, security, auditability
    - Follow-up: *"Is this a background job or does the user wait? What if it fails midway?"*

22. **"Who needs to approve this before it ships?"**
    - Looking for: stakeholder alignment, DACI roles
    - Challenge: *"You're the Driver. Who's the Approver? Have they seen this yet?"*

23. **"When does this need to ship, and why that date?"**
    - Looking for: real deadlines vs. arbitrary dates
    - Challenge: *"Is Q2 a hard deadline or a wish? What happens if it slips to Q3?"*

### Stage 4 Exit Criteria
- [ ] Measurable success criteria defined (not vanity metrics)
- [ ] Acceptance criteria drafted (at least happy path)
- [ ] Performance constraints stated
- [ ] Non-functional requirements identified
- [ ] Stakeholder alignment confirmed
- [ ] Timeline realistic

---

## Transition to Mode 3

When all four stages are complete (or the PM has enough coverage to proceed), the GPT says:

> *"You've worked through the key areas. Let me summarize what we have and what's still open. Would you like to move to a Spec Readiness Assessment to see if this is ready for specification?"*

The GPT then provides a structured summary of:
1. **Problem statement** (from Stage 1)
2. **V1 scope** (from Stage 2)
3. **Key assumptions and risks** (from Stage 3)
4. **Success criteria** (from Stage 4)
5. **Open items** (anything unresolved across all stages)
