# 3-Week Fast-Track Rollout Plan

> Aggressive alternative to the [10-week rollout](rollout-plan.md). Compresses foundation + pilot into 2 weeks, then uses a team hackathon in Week 3 to onboard everyone simultaneously. Designed for teams that want to sprint into SDD rather than ease in.

---

## When to Use This Plan

Use the fast-track if:

- The GPM has strong exec support and can dedicate near-full-time for 3 weeks
- The team is small enough (3-6 PMs) for a single hackathon to cover everyone
- There's urgency — a new product initiative, compliance deadline, or team restructuring creates a natural forcing function
- The GPM is comfortable being "one week ahead" of the team rather than months ahead

Use the [standard 10-week plan](rollout-plan.md) if the GPM needs more ramp time, the team is large, or political capital is limited.

---

## Rollout Philosophy

Everything from the standard plan applies, with two additions:

- **Roll out a win, not a system.** Lead with a successful example, not process documentation.
- **Frame constraints as cost-prevention guardrails**, not bureaucratic requirements.
- **The GPT is training wheels** for rigorous product thinking — it becomes optional once the habits are internalized.
- **Start with one trio**, prove it works, then expand with evidence.
- **Sprint mentality.** Week 1 is build, Week 2 is prove, Week 3 is scale. No week is optional.
- **The hackathon is the scaling event.** Instead of onboarding PMs one at a time over weeks, get everyone hands-on in a single day with real ideas and real artifacts.

---

## Week 1: Build the Machine

**Who**: GPM (solo)
**Goal**: All infrastructure in place. End-to-end dry run complete. GPM confident enough to coach.

### Day 1 (Monday): Confluence


| Time       | Task                                                                                                                                   | Reference                                                                                     |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Morning    | Create "Product Development Hub" space (or section)                                                                                    | `[confluence/confluence-setup-guide.md](../confluence/confluence-setup-guide.md)` — Steps 1-2 |
| Morning    | Upload all 6 templates: Problem Brief, Assumption Map, Decision Record, Capability Record, Product Capability Map, Portfolio Dashboard | `[confluence/templates/](../confluence/templates/)`                                           |
| Afternoon  | Create Lifecycle Guide and Triage Decision Tree reference pages                                                                        | `[confluence/reference/](../confluence/reference/)`                                           |
| Afternoon  | Initialize Portfolio Dashboard (empty but structured) and Product Capability Map (list known domain areas)                             | Setup Guide — Steps 4-5                                                                       |
| End of day | Run the setup verification checklist                                                                                                   | Setup Guide — Step 9                                                                          |


### Day 2 (Tuesday): GitHub + spec-kit


| Time      | Task                                                                                 | Reference                                                           |
| --------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| Morning   | Initialize spec-kit in the product catalog spec repository                           | —                                                                   |
| Morning   | Upload `constitution.md` to `.specify/` directory                                    | `[speckit/constitution.md](../speckit/constitution.md)`             |
| Morning   | Upload spec template override and plan template override to `.specify/`              | `[speckit/overrides/](../speckit/overrides/)`                       |
| Afternoon | Verify spec-kit commands work: `/speckit.specify`, `/speckit.plan`, `/speckit.tasks` | `[speckit/constitution-guide.md](../speckit/constitution-guide.md)` |
| Afternoon | Test that the constitution is referenced in generated specs                          | —                                                                   |


### Day 3 (Wednesday): GPT


| Time      | Task                                                                                                                                                                                       | Reference                                                         |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| Morning   | Create Custom GPT in ChatGPT                                                                                                                                                               | `[gpt/gpt-setup-guide.md](../gpt/gpt-setup-guide.md)` — Steps 1-3 |
| Morning   | Upload all 6 knowledge files: `product-catalog-domain.md`, `sdd-methodology.md`, `speckit-reference.md`, `triage-criteria.md`, `discovery-question-bank.md`, `spec-readiness-checklist.md` | `[gpt/knowledge/](../gpt/knowledge/)`                             |
| Morning   | Configure system prompt from `gpt/system-prompt.md`                                                                                                                                        | `[gpt/system-prompt.md](../gpt/system-prompt.md)`                 |
| Afternoon | Configure capabilities (Web Browsing OFF, DALL-E OFF, Code Interpreter OFF) and sharing                                                                                                    | Setup Guide — Steps 5-6                                           |
| Afternoon | Test all three modes using the test inputs from the setup guide                                                                                                                            | Setup Guide — Step 7                                              |


### Day 4 (Thursday): Dry Run


| Time       | Task                                                                | Reference                                                                           |
| ---------- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Full day   | Run the "Bulk Import of Billing Codes" example end-to-end           | `[examples/](../examples/)`                                                         |
| —          | Triage with GPT Mode 1 → create Problem Brief in Confluence         | `[examples/example-problem-brief.md](../examples/example-problem-brief.md)`         |
| —          | Discovery with GPT Mode 2 → create Assumption Map                   | `[examples/example-discovery-session.md](../examples/example-discovery-session.md)` |
| —          | Spec Readiness with GPT Mode 3 → generate `/speckit.specify` prompt | `[examples/example-spec-readiness.md](../examples/example-spec-readiness.md)`       |
| —          | Run `/speckit.specify` and `/speckit.plan` in spec-kit              | `[examples/example-specify-prompt.md](../examples/example-specify-prompt.md)`       |
| End of day | Document friction points, wording issues, rough edges               | —                                                                                   |


### Day 5 (Friday): Refine + Recruit


| Time      | Task                                                                                                                        | Reference                                                                 |
| --------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Morning   | Fix issues found during dry run: template wording, GPT prompt adjustments, Confluence layout                                | —                                                                         |
| Morning   | Re-run any broken parts of the dry run to verify fixes                                                                      | —                                                                         |
| Afternoon | Identify and recruit the pilot PM (pick someone curious, not skeptical)                                                     | —                                                                         |
| Afternoon | Brief the pilot PM: "I need you for 4-5 hours next week. You'll run a real feature through a new workflow. I'll coach you." | —                                                                         |
| Afternoon | Ask pilot PM to come with a real Tier 2 idea (Enhancement — moderate scope, additive API impact)                            | `[gpt/knowledge/triage-criteria.md](../gpt/knowledge/triage-criteria.md)` |


### Week 1 Deliverable

All three systems operational. GPM has personally run the full workflow end-to-end. Pilot PM recruited with a real idea.

---

## Week 2: Pilot with One Trio

**Who**: GPM + one PM (and their tech lead / designer as needed)
**Goal**: One real feature through Triage → Discovery → Spec Readiness (minimum). Hackathon logistics locked down.

> **Protect the pilot.** This is the make-or-break week. If the pilot PM has a bad experience, the hackathon will fail. Clear the pilot PM's calendar. Don't let other work crowd this out.

### Day 6 (Monday): Onboard the Pilot PM


| Time         | Task                                                                                | Duration |
| ------------ | ----------------------------------------------------------------------------------- | -------- |
| Morning      | 60-minute walkthrough with pilot PM                                                 | 60 min   |
|              | — Show the architecture: 3 systems (Confluence, GPT, spec-kit) and how they connect |          |
|              | — Walk through the Bulk Import example end-to-end (show real artifacts, not slides) |          |
|              | — Show each GPT mode live (don't just describe — let them see it in action)         |          |
|              | — Explain the constitution and why it matters                                       |          |
| Late morning | PM triages their real idea with GPT Mode 1, GPM observes (~15 min)                  | 15 min   |
| Afternoon    | PM creates their Problem Brief in Confluence (async, GPM available for questions)   | —        |


### Day 7 (Tuesday): Discovery


| Time      | Task                                                                                          | Duration  |
| --------- | --------------------------------------------------------------------------------------------- | --------- |
| Morning   | PM runs Discovery session with GPT Mode 2                                                     | 45-60 min |
|           | GPM facilitates per the [facilitator guide](facilitator-guide.md) — observes more than drives |           |
|           | If PM needs to pause to gather info, that's expected — schedule continuation for Wednesday    |           |
| Afternoon | PM creates/updates Assumption Map from Discovery output                                       | —         |


### Day 8 (Wednesday): Spec Readiness + Specification


| Time         | Task                                                                        | Duration  |
| ------------ | --------------------------------------------------------------------------- | --------- |
| Morning      | Finish Discovery if it ran over (common for Tier 3, less common for Tier 2) | —         |
| Late morning | PM runs Spec Readiness Assessment with GPT Mode 3 (~20-30 min)              | 20-30 min |
| Afternoon    | If ready: PM runs `/speckit.specify` with the generated prompt              | —         |
| Afternoon    | GPM reviews all artifacts and provides feedback                             | —         |


### Day 9 (Thursday): Hackathon Prep


| Time      | Task                                                                                                                                          |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Morning   | GPM and pilot PM debrief: What worked? What was confusing? What needs adjusting before the hackathon?                                         |
| Morning   | Fix any template or GPT issues uncovered during the pilot                                                                                     |
| Afternoon | Book the hackathon room/space (full day, enough tables for trios)                                                                             |
| Afternoon | Send hackathon invitation to all PMs with pre-work instructions (see [Pre-Hackathon Logistics Checklist](#pre-hackathon-logistics-checklist)) |
| Afternoon | Assign hackathon trios: each PM + their tech lead (+ designer if available)                                                                   |


### Day 10 (Friday): Finalize


| Time      | Task                                                                                                                    |
| --------- | ----------------------------------------------------------------------------------------------------------------------- |
| Morning   | Pilot PM writes a short (1-paragraph) testimonial: "Here's what this workflow did for my idea"                          |
| Morning   | GPM prepares hackathon materials: agenda, printed quick-reference cards, the pilot PM's real artifacts as show-and-tell |
| Afternoon | Verify all hackathon logistics: every PM has GPT access, GitHub repo access, Confluence access (see checklist below)    |
| Afternoon | Print/share the hackathon agenda and quick-reference table                                                              |


### Week 2 Deliverable

One real feature through Triage → Problem Brief → Discovery → Spec Readiness (minimum). Pilot PM is an advocate. Hackathon fully planned and communicated.

---

## Week 3: Hackathon Day

**Who**: All PMs + their tech leads (+ designers if available). GPM facilitates.
**Goal**: Every PM triages, discovers, and begins specifying a real feature — in one day.

> **Ideal hackathon ideas are Tier 2 (Enhancement)** — moderate scope, additive API impact — achievable in a day's work. If a PM has an ambitious Tier 3 (New Capability), that's fine — they'll get through Triage and Discovery but may not reach Spec Readiness by end of day. No Tier 1s — those are Jira tickets and don't need this workflow.

### Pre-Hackathon (sent to PMs 3-5 days before)

> "Come prepared with a real idea — something you've been meaning to propose or a request you've been sitting on. Ideal: a Tier 2 Enhancement (new endpoint, new workflow, new business rule). If you're not sure, bring it anyway and we'll triage it together.
>
> **Before the hackathon:**
>
> 1. Verify you can access the SDD Companion GPT (link in invite)
> 2. Verify you can access the Product Development Hub in Confluence
> 3. Verify you can access the spec repository in GitHub
> 4. Read the Bulk Import example Problem Brief: `examples/example-problem-brief.md`
> 5. Think about your idea — 2-3 sentences on what the problem is (not the solution)"

### Hour-by-Hour Agenda

#### 9:00-9:45 — Opening + Show-and-Tell (45 min)


| Time      | Activity                                                                                                                                                                                                                                                                                   |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 9:00-9:15 | GPM: Why we're doing this. 5-minute version — problem (undocumented decisions, rework, compliance gaps), solution (SDD workflow), what we'll do today.                                                                                                                                     |
| 9:15-9:35 | **Pilot PM presents their experience.** This is the most important 20 minutes of the day. The pilot PM walks through their real feature: Problem Brief → Discovery → Spec Readiness → spec. Show the actual Confluence pages and GPT conversations. Peer credibility > management mandate. |
| 9:35-9:45 | Q&A. Let skepticism surface here — better than during the working sessions.                                                                                                                                                                                                                |


#### 9:45-10:15 — Live Triage Demo + Individual Triage (30 min)


| Time        | Activity                                                                                                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 9:45-10:00  | GPM demos a live triage with one PM's idea (volunteer or pre-arranged). Everyone watches the GPT interaction. ~15 min per the [facilitator guide](facilitator-guide.md). |
| 10:00-10:15 | Each PM triages their own idea with GPT Mode 1. GPM and pilot PM float between tables to help.                                                                           |


**Output**: Every PM has a Triage Result and knows their tier.

#### 10:15-10:30 — Break (15 min)

#### 10:30-12:00 — Discovery Sessions (90 min)


| Time        | Activity                                                                                                                            |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 10:30-10:40 | GPM: 10-minute briefing on Discovery. Show the 4 stages. Explain: "The GPT will challenge you — that's the point."                  |
| 10:40-12:00 | Each PM runs Discovery with GPT Mode 2 at their own pace. GPM and pilot PM circulate.                                               |
|             | **Tier 2 PMs**: Should complete Discovery in 45-60 min, then start on Assumption Map.                                               |
|             | **Tier 3 PMs**: Will likely use the full 80 min and may not finish — that's fine. Mark open items and continue after the hackathon. |
|             | **Tech leads**: Jump in when GPT surfaces technical questions (API surface, architecture, feasibility).                             |


**Output**: Discovery Summaries. Assumption Maps started in Confluence.

#### 12:00-12:45 — Lunch (45 min)

Working lunch optional. Encourage PMs to talk about what they're finding — the cross-pollination is valuable.

#### 12:45-2:15 — Spec Readiness + Specification (90 min)


| Time        | Activity                                                                                                                     |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 12:45-12:55 | GPM: 10-minute briefing on Spec Readiness. Show the 20-item checklist. Explain Green/Yellow/Red.                             |
| 12:55-1:25  | Each PM runs Spec Readiness Assessment with GPT Mode 3 (~20-30 min).                                                         |
| 1:25-2:15   | PMs who pass: refine the generated `/speckit.specify` prompt, then run it in spec-kit.                                       |
|             | PMs who don't pass: identify the Red items, make a plan to resolve them. This is a success — the workflow caught gaps early. |


**Output**: Spec Readiness scores. Some PMs have draft specs. Others have clear lists of what to resolve first.

#### 2:15-2:30 — Break (15 min)

#### 2:30-3:30 — Finish + Polish (60 min)


| Time      | Activity                                                                                                                            |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 2:30-3:30 | Open working time. PMs finish whatever stage they're at.                                                                            |
|           | GPM and pilot PM provide 1:1 coaching as needed.                                                                                    |
|           | PMs who finished early: start documenting an existing capability using the Capability Record template (seeds the retrofit process). |


#### 3:30-4:00 — Lightning Demos + Retro (30 min)


| Time      | Activity                                                                                                                                                                                                                                                |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 3:30-3:50 | **Lightning demos**: Each PM gets 2 minutes to show what they triaged, what Discovery revealed, and where they are. Focus on "what surprised me" — the assumptions they hadn't examined, the compliance gap they didn't know about, the scope they cut. |
| 3:50-4:00 | **Quick retro**: What worked today? What was confusing? What would you change? GPM captures feedback for process refinement.                                                                                                                            |


### Hackathon Day Deliverables

- Every PM has at least: Triage Result + Problem Brief + Discovery Summary
- Most PMs have: Spec Readiness score + draft spec (Tier 2 ideas)
- Some PMs have: clear list of open items to resolve (Tier 3 ideas or "Not Ready" results)
- Portfolio Dashboard updated with all new features
- GPM has retro feedback for refinement

---

## Post-Hackathon: Weeks 4-6

### Week 4: Convert Momentum to Habit


| Task                                                                               | Who              |
| ---------------------------------------------------------------------------------- | ---------------- |
| Follow up individually with each PM: Where did you leave off? What's blocking you? | GPM              |
| PMs who reached spec: proceed to `/speckit.plan` with their tech lead              | PMs + Tech Leads |
| PMs who didn't reach spec: resolve Red items, re-run Spec Readiness                | PMs              |
| First weekly 15-minute office hours (drop-in, not mandatory)                       | GPM              |
| Update Portfolio Dashboard with all hackathon features                             | GPM              |
| Refine templates and GPT prompt based on hackathon retro feedback                  | GPM              |


### Week 5: Steady State Begins


| Task                                                                                       | Who      |
| ------------------------------------------------------------------------------------------ | -------- |
| All PMs should have at least one feature through Spec Readiness by end of week             | PMs      |
| GPM spot-checks: are Problem Briefs being created? Are specs referencing the constitution? | GPM      |
| Second office hours session                                                                | GPM      |
| Pilot PM begins coaching other PMs (distributed expertise)                                 | Pilot PM |


### Week 6: Retrofit Seeding + Retrospective


| Task                                                                                                            | Who            |
| --------------------------------------------------------------------------------------------------------------- | -------------- |
| 60-minute team retrospective (same format as standard plan Week 6)                                              | All            |
| Assign quarterly retrofit targets: each PM documents 2-3 existing capabilities using Capability Record template | GPM            |
| Begin regular Portfolio Dashboard reviews                                                                       | GPM            |
| GPM and pilot PM create a short case study from the pilot feature                                               | GPM + Pilot PM |
| Constitution review: any articles causing friction? Any missing?                                                | GPM            |


### Ongoing (Post Week 6)

Same cadence as the [standard rollout plan](rollout-plan.md#ongoing-post-week-10):

- **Weekly**: Portfolio Dashboard review, 15-min office hours, spot-checks
- **Monthly**: Capability Map progress review, constitution review, GPT prompt refinement
- **Quarterly**: Capability Record documentation sprint, metrics review, process retrospective

---

## Quick Reference


| What                       | Who                        | When              | Duration     |
| -------------------------- | -------------------------- | ----------------- | ------------ |
| Confluence setup           | GPM                        | Week 1, Day 1     | 1 day        |
| GitHub + spec-kit setup    | GPM                        | Week 1, Day 2     | 1 day        |
| GPT creation + testing     | GPM                        | Week 1, Day 3     | 1 day        |
| End-to-end dry run         | GPM                        | Week 1, Day 4     | 1 day        |
| Refine + recruit pilot PM  | GPM                        | Week 1, Day 5     | 1 day        |
| Pilot PM walkthrough       | GPM + Pilot PM             | Week 2, Day 6     | 60 min       |
| Pilot PM triage            | Pilot PM (GPM observes)    | Week 2, Day 6     | ~15 min      |
| Pilot Discovery session    | Pilot PM (GPM facilitates) | Week 2, Day 7     | 45-60 min    |
| Pilot Spec Readiness       | Pilot PM                   | Week 2, Day 8     | 20-30 min    |
| Hackathon prep + logistics | GPM                        | Week 2, Days 9-10 | 2 days       |
| **Hackathon**              | **All PMs + Tech Leads**   | **Week 3**        | **Full day** |
| Post-hackathon follow-up   | GPM + all PMs              | Weeks 4-6         | Ongoing      |


---

## Pre-Hackathon Logistics Checklist

Send this to all PMs 3-5 days before the hackathon. GPM should verify every item the day before.

### Technical Access (every participant)

- **GPT access**: Can open the SDD Companion GPT in ChatGPT (requires ChatGPT Plus or Team account)
- **Confluence access**: Can view and edit pages in the Product Development Hub space
- **GitHub access**: Can access the product catalog spec repository and run spec-kit commands
- **Network**: If on-site, confirm WiFi can reach ChatGPT, Confluence, and GitHub (no VPN/firewall blocks)

### Pre-Reading (every PM)

- Read the example Problem Brief: `[examples/example-problem-brief.md](../examples/example-problem-brief.md)`
- Skim the tier definitions so you know what Tier 1/2/3 means: `[gpt/knowledge/triage-criteria.md](../gpt/knowledge/triage-criteria.md)`
- Prepare your idea: 2-3 sentences describing the **problem** (not the solution)

### Room / Space

- Room booked for full day (9:00-4:00) with enough table space for trios to work independently
- Projector or screen for GPM demos and pilot PM show-and-tell
- Power outlets / charging for laptops
- Lunch ordered or plan communicated

### Materials (GPM prepares)

- Printed or shared hackathon agenda (the hour-by-hour from this document)
- Printed quick-reference cards: tier definitions, GPT modes, template names
- Pilot PM's real artifacts ready for show-and-tell (Problem Brief, Discovery output, Spec Readiness, spec)
- Portfolio Dashboard open and ready to update live during the day
- Spare copies of the Bulk Import example for PMs who didn't do the pre-reading

---

## Anti-Patterns to Watch For

Same as the [standard plan](rollout-plan.md#anti-patterns-to-watch-for), plus these fast-track-specific risks:


| Anti-Pattern                          | Signal                                                         | Remedy                                                                                                                                  |
| ------------------------------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Hackathon feels forced                | PMs are going through motions, not engaging with GPT questions | Slow down. Have the pilot PM share their "aha moment." Remind: this is about better outcomes, not process compliance.                   |
| GPM is bottleneck during hackathon    | GPM can't circulate fast enough, PMs are stuck waiting         | Enlist the pilot PM as a second coach. Pair stuck PMs with PMs who are ahead.                                                           |
| Ideas are too small for the workflow  | PMs bring Tier 1 config changes to avoid the process           | Redirect: "That's a Jira ticket — great, you just triaged it in 2 minutes. Now, what's the Enhancement you've been wanting to propose?" |
| Ideas are too big for a hackathon day | PM brings a Tier 3 moonshot and gets frustrated not finishing  | Reframe: "Getting through Discovery on a Tier 3 in one day is a win. You've identified gaps that would have cost weeks later."          |
| Post-hackathon momentum dies          | No follow-up in Week 4, PMs go back to old habits              | GPM must follow up individually within 48 hours. The office hours aren't optional for the GPM.                                          |


