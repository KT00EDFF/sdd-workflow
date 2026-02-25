# 10-Week Rollout Plan

> Sequence for deploying the SDD workflow system to the Product Catalog team. Designed for the GPM to execute, starting solo and expanding incrementally.

---

## Rollout Philosophy

- **Roll out a win, not a system.** Lead with a successful example, not process documentation.
- **Frame constraints as cost-prevention guardrails**, not bureaucratic requirements.
- **The GPT is training wheels** for rigorous product thinking — it becomes optional once the habits are internalized.
- **Start with one trio**, prove it works, then expand with evidence.

---

## Week 1-2: Foundation

**Who**: GPM (solo or with one trusted PM)

### Goals
- All infrastructure in place
- End-to-end dry run complete
- GPM confident in the workflow

### Tasks

#### Confluence Setup
- [ ] Create "Product Development Hub" space (or section within existing space)
- [ ] Upload all templates: Problem Brief, Assumption Map, Decision Record, Capability Record, Product Capability Map, Portfolio Dashboard
- [ ] Create the Lifecycle Guide reference page
- [ ] Create the Triage Decision Tree reference page
- [ ] Initialize the Portfolio Dashboard (empty but structured)
- [ ] Initialize the Product Capability Map (start listing known capabilities)

#### GitHub Setup
- [ ] Initialize spec-kit in the product catalog spec repository
- [ ] Upload `constitution.md` to `.specify/` directory
- [ ] Upload spec template override and plan template override
- [ ] Verify spec-kit commands work (`/speckit.specify`, `/speckit.plan`, `/speckit.tasks`)

#### GPT Setup
- [ ] Create Custom GPT in ChatGPT (see GPT Setup Guide)
- [ ] Upload knowledge files: domain model, SDD methodology, spec-kit reference, triage criteria, discovery question bank, spec readiness checklist
- [ ] Configure system prompt
- [ ] Test all three modes with synthetic inputs

#### End-to-End Dry Run
- [ ] Invent a hypothetical feature (or use the "Bulk Import of Billing Codes" example)
- [ ] Run it through: Triage → Problem Brief → Discovery → Assumption Map → Spec Readiness → /speckit.specify → /speckit.plan
- [ ] Document friction points, rough edges, wording issues
- [ ] Refine templates and GPT prompt based on dry run findings

### Deliverable
GPM has personally experienced the full workflow end-to-end and is ready to coach others.

---

## Week 3-4: Pilot with One Trio

**Who**: GPM + one PM (and their tech lead / designer)

### Goals
- One real feature triaged and through Discovery
- PM has formed their own opinion of the workflow
- Identify real-world friction vs. dry run friction

### Tasks

#### Week 3: Onboarding
- [ ] 60-minute walkthrough session with the pilot PM:
  - Show the architecture (3 systems, how they connect)
  - Walk through the "Bulk Import" example end-to-end
  - Show (don't just describe) each GPT mode
  - Explain the constitution and why it matters
- [ ] PM identifies a real current idea/request to use as the pilot feature
- [ ] PM triages the idea with GPT (Mode 1), GPM observes

#### Week 4: First Real Cycle
- [ ] PM creates Problem Brief in Confluence
- [ ] PM runs Discovery session with GPT (Mode 2)
  - GPM available for questions but doesn't drive
  - PM may need to pause Discovery to gather info (this is expected)
- [ ] PM creates Assumption Map from Discovery output
- [ ] PM runs Spec Readiness Assessment (Mode 3)
- [ ] If ready: PM runs `/speckit.specify` with the generated prompt
- [ ] GPM reviews all artifacts, provides feedback

### Deliverable
One real feature has gone through Triage → Problem Brief → Discovery → Spec Readiness (minimum). Possibly through to specification.

---

## Week 5-6: Full Cycle Completion

**Who**: GPM + pilot trio

### Goals
- Pilot feature reaches spec + plan (or further)
- Confluence artifacts are populated with real content
- Template issues identified and fixed

### Tasks

#### Week 5: Specification & Planning
- [ ] Complete specification (if not done in Week 4)
- [ ] Tech lead reviews spec for feasibility and constitution compliance
- [ ] Compliance lead reviews Compliance Impact Assessment (if applicable)
- [ ] Tech lead runs `/speckit.plan`
- [ ] Review plan: architecture, test strategy, compliance checkpoint

#### Week 6: Retrospective & Refinement
- [ ] Run a 60-minute retrospective with the pilot trio:
  - What worked well?
  - What was confusing or felt like busywork?
  - What would you change about the templates?
  - Did the GPT add value or create friction?
  - How does this compare to how you worked before?
- [ ] Document findings
- [ ] Refine: templates, GPT prompt, constitution wording based on feedback
- [ ] PM writes a short (1-paragraph) testimonial: "Here's what this workflow did for me"

### Deliverable
Pilot feature fully specified and planned. Retrospective findings documented. Templates refined.

---

## Week 7-8: Internal Case Study

**Who**: GPM + pilot PM

### Goals
- Produce a compelling internal case study
- Portfolio Dashboard shows real data
- Begin building the Capability Map

### Tasks

#### Week 7: Case Study
- [ ] GPM and pilot PM create a short case study:
  - The feature: what it is, why it matters
  - Before: how the PM would have approached it without the workflow
  - After: how the workflow changed the process
  - Concrete outcomes: assumptions validated, scope clarified, compliance gaps caught
  - PM's perspective: what they'd tell another PM
- [ ] Update Portfolio Dashboard with pilot feature data
- [ ] Pilot PM documents 1-2 existing capabilities using the Capability Record template (start the retrofit process)

#### Week 8: Prepare for Expansion
- [ ] Schedule walkthrough sessions for remaining PMs
- [ ] Prepare materials: case study, example artifacts, quick-reference card
- [ ] Plan weekly office hours format

### Deliverable
Internal case study ready. Portfolio Dashboard has real data. Capability Map has initial entries.

---

## Week 9-10: Expand to All Trios

**Who**: GPM + all PMs

### Goals
- All PMs onboarded and using the workflow
- Weekly office hours established
- Retrofit process started across the team

### Tasks

#### Week 9: Onboarding
- [ ] 60-minute walkthrough per trio (or combined if schedules allow):
  - **Lead with the case study**, not the process docs
  - Show the pilot PM's real artifacts (Problem Brief, Discovery output, spec)
  - Have the pilot PM present their experience (peer credibility > management mandate)
  - Walk through the three GPT modes
  - Each PM identifies their first feature to run through the workflow
- [ ] Each PM triages their first feature (Mode 1) by end of week
- [ ] First weekly office hours session (15 minutes, drop-in)

#### Week 10: First Cycle + Retrofit Kickoff
- [ ] Each PM creates their first Problem Brief
- [ ] PMs who are ready proceed to Discovery
- [ ] Assign quarterly retrofit targets: each PM documents 2-3 Capability Records this quarter
- [ ] GPM begins regular Portfolio Dashboard reviews
- [ ] Second office hours session

### Deliverable
All PMs actively using the workflow. Retrofit targets assigned. Office hours running.

---

## Ongoing (Post Week 10)

### Weekly
- [ ] GPM reviews Portfolio Dashboard
- [ ] 15-minute office hours (drop-in, not mandatory)
- [ ] GPM spot-checks: are Problem Briefs being created? Are specs referencing the constitution?

### Monthly
- [ ] GPM reviews Capability Map progress
- [ ] Constitution review: any articles causing friction? Any missing?
- [ ] GPT prompt refinement based on accumulated usage patterns

### Quarterly
- [ ] Capability Record documentation sprint (each PM: 2-3 records)
- [ ] Metrics review: cycle time, spec quality, triage accuracy, compliance incidents
- [ ] Process retrospective: what's working, what needs adjustment
- [ ] Constitution amendment review (if proposals exist)

---

## Anti-Patterns to Watch For

| Anti-Pattern | Signal | Remedy |
|-------------|--------|--------|
| Workflow as checkbox exercise | Problem Briefs are vague, copied from Jira tickets | Coach: "Show me the evidence for this impact statement" |
| GPT as rubber stamp | PMs skip Discovery, go straight to Spec Readiness | Coach: "Walk me through your assumptions. Which ones have you validated?" |
| Constitution ignored | Specs don't reference articles | Add constitution compliance as a PR review checklist item |
| Retrofitting deprioritized | No Capability Records created | Include in quarterly OKRs, review in 1:1s |
| Over-processing Tier 1s | Config changes going through full Discovery | Recalibrate: "Remember, Tier 1 = Jira ticket. 2 minutes." |
| Skipping compliance | Features ship without Compliance Impact Assessment | Block: this is non-negotiable in a regulated environment |
