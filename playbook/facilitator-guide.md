# Facilitator Guide — Running SDD Sessions

> Practical guide for facilitating Triage, Discovery, and Spec Readiness sessions. Primarily for the GPM during rollout, but any PM can use this to self-facilitate.

---

## General Facilitation Principles

1. **The GPT does the questioning; you coach the thinking.** Don't compete with the GPT — complement it. The GPT asks structured questions; you notice when the PM is avoiding something.

2. **Silence is productive.** When the PM pauses after a tough GPT question, don't fill the silence. They're thinking. That's the point.

3. **Redirect, don't answer.** If the PM asks "What do you think the tier should be?" respond with "What's your assessment? Walk me through why."

4. **Capture, don't just discuss.** Every session should produce a written artifact. If it only produced conversation, it wasn't a session — it was a chat.

5. **Time-box ruthlessly.** Discovery can expand indefinitely. Set a time limit and stick to it. Open items go on the list for next session.

---

## Session Type 1: Triage (~15 minutes)

### When to Use
A PM has a new idea or request and needs to classify it.

### Setup
- PM has: the raw request/idea (written or verbal)
- Facilitator has: Triage Decision Tree reference, GPT open to Mode 1

### Flow

**Minutes 0-2: Frame**
> "Let's triage this. Describe the request in 2-3 sentences. What's being asked for?"

**Minutes 2-10: GPT Triage**
- PM inputs the request into the GPT (Mode 1)
- GPT asks clarifying questions, PM answers
- GPT produces the Triage Result

**Minutes 10-15: Calibrate**
- Review the GPT's tier classification together
- Check: does the regulatory flag make sense?
- Check: does the tier feel right given what you know about the system?
- If you disagree with the GPT, discuss why and override if warranted

**Output**: Triage Result. PM knows the next step (Jira ticket for Tier 1, Problem Brief for Tier 2/3).

### Facilitator Watch-Fors
| Signal | What It Means | What to Do |
|--------|--------------|-----------|
| PM describes solution, not problem | PM has jumped to "what to build" | "Back up — what's the problem this solves?" |
| PM immediately says "Tier 1, it's easy" | May be under-classifying to avoid process | "Walk me through the API impact and compliance check" |
| PM says "Tier 3, this is huge" | May be over-classifying out of excitement | "What specifically makes this cross-cutting?" |

---

## Session Type 2: Discovery (~45-60 minutes)

### When to Use
A Tier 2 or Tier 3 feature needs structured critical questioning. The PM has a Problem Brief (at least in draft).

### Setup
- PM has: Problem Brief (at least draft), any relevant data/evidence
- Facilitator has: Discovery Question Bank reference, GPT open to Mode 2
- Optional: Tech lead and designer available (in room or on call)

### Pre-Session (PM prep — share these instructions 24 hours before)
> "Before our Discovery session, please prepare:
> 1. Your draft Problem Brief
> 2. Any evidence you have: user feedback, support tickets, usage data, stakeholder requests
> 3. A rough idea of who the target users are
> 4. Any initial thinking on scope (what's in, what's out)
>
> You don't need polished answers — the session will sharpen them."

### Flow

**Minutes 0-5: Set the Stage**
> "We're going to run through four stages of questioning. The GPT will challenge your thinking — that's the point. It's not adversarial; it's making sure this idea is solid before we invest in specifying it.
>
> The four stages are: Is the problem real? What's the scope? What could go wrong? How will we know it works?
>
> I'll jump in occasionally, but the GPT is driving the questions."

**Minutes 5-20: Stage 1 — Problem Validation**
- PM inputs the Problem Brief into GPT (Mode 2)
- GPT runs Stage 1 questions
- **Facilitator**: Watch for the PM defending instead of investigating. If they get defensive: "That's a strong conviction. What evidence would change your mind?"

**Minutes 20-35: Stage 2 — Scope & Boundaries**
- GPT transitions to Stage 2
- **Facilitator**: This is where scope creep lives. If the PM's scope keeps expanding: "You've listed 8 things. If you could only ship 3, which ones?"
- Call in tech lead if API surface questions get technical

**Minutes 35-50: Stage 3 — Assumptions & Risks**
- GPT runs Stage 3
- **Facilitator**: Help the PM distinguish assumptions from facts. "You said 'PMs will use this weekly.' Is that based on data or a guess?"
- Push for the confidence × impact rating on each assumption

**Minutes 50-60: Stage 4 — Success Criteria**
- GPT runs Stage 4
- **Facilitator**: Reject vague success criteria. "How will you measure that number? Where does the data come from?"
- Call in designer if UX success criteria are relevant

**Last 5 minutes: Wrap-Up**
- GPT provides Discovery Summary
- Review open items together
- Decide: "Ready for Spec Readiness, or do we need to resolve open items first?"
- PM commits to next step and timeline

**Output**: Discovery Summary (GPT output) → PM creates/updates Assumption Map in Confluence.

### Facilitator Watch-Fors
| Signal | What It Means | What to Do |
|--------|--------------|-----------|
| PM has answers for everything instantly | May not be thinking critically, or has rehearsed | "Play devil's advocate for a moment — what would a skeptic say?" |
| PM gets stuck and can't answer | Found a real gap — this is valuable | "That's a great gap to find now rather than after specifying. Let's mark it as an open item." |
| PM keeps circling back to the solution | Problem not well understood yet | "I notice you keep describing the CSV upload. Let's spend more time on why manual entry is a problem." |
| Session is running long | Normal for Tier 3, concerning for Tier 2 | Time-box: "Let's capture what we have and schedule a follow-up for the open items." |
| PM and tech lead disagree | Healthy tension — don't resolve prematurely | "Let's capture both perspectives as assumptions and decide which to validate." |

---

## Session Type 3: Spec Readiness (~20-30 minutes)

### When to Use
A feature has been through Discovery (or the PM believes it's ready without full Discovery for Tier 2) and needs a readiness assessment.

### Setup
- PM has: Discovery Summary (or Problem Brief + their own analysis)
- Facilitator has: Spec Readiness Checklist reference, GPT open to Mode 3

### Flow

**Minutes 0-5: Input**
- PM provides the Discovery Summary to the GPT (Mode 3)
- If no formal Discovery, PM describes the feature with: problem, scope, assumptions, success criteria

**Minutes 5-15: Checklist Scoring**
- GPT scores each of the 20 items
- **Facilitator**: Don't argue with individual scores during the assessment. Let the GPT complete the full checklist, then discuss.

**Minutes 15-25: Review & Discussion**
- Review the scored checklist together
- For Red items: "What's the fastest path to resolving this?"
- For Yellow items: "Can we proceed with this as a `[NEEDS CLARIFICATION]` marker, or is it too risky?"
- For the overall recommendation: does it feel right?

**Minutes 25-30: Next Steps**
- If **Ready**: Review the draft `/speckit.specify` prompt. PM edits it for accuracy.
- If **Almost Ready**: Review the prompt with `[NEEDS CLARIFICATION]` markers. PM decides: resolve now or proceed with markers?
- If **Not Ready**: Identify the 1-2 highest-priority Red items. PM creates a plan to resolve them.

**Output**: Scored checklist + recommendation. If passing: draft `/speckit.specify` prompt for PM to refine.

### Facilitator Watch-Fors
| Signal | What It Means | What to Do |
|--------|--------------|-----------|
| All Greens, no Yellows | Either genuinely well-prepared (great!) or PM provided overly optimistic input | Spot-check 2-3 items: "Tell me more about item 11 — how did you assess compliance risk?" |
| Many Reds | Feature isn't ready — Discovery may have been insufficient | Don't force it. "Looks like we need more Discovery work. Let's prioritize the Reds." |
| PM wants to override Red scores | Eagerness to move to spec-kit | Hold the line: "Reds are blockers. What's the fastest path to Green on this item?" |
| Generated prompt looks off | GPT didn't have enough context | Edit together: "This is a draft. Let's fix the parts that don't match reality." |

---

## Common Facilitation Mistakes

| Mistake | Why It Happens | How to Avoid |
|---------|---------------|-------------|
| Facilitator answers the GPT's questions for the PM | GPM knows the domain well, wants to help | Bite your tongue. The PM needs to develop the thinking muscle. |
| Rushing through Discovery to get to the spec | Pressure to deliver, process fatigue | Remember: 30 minutes in Discovery saves days of rework in implementation. |
| Skipping compliance questions | "This one isn't regulated" | Always ask. The PM may not realize the feature touches regulated data. |
| Accepting the first answer | PM gives a surface answer, facilitator moves on | Push once: "Go deeper. Why?" If the PM can defend it, move on. |
| Making it feel like an interrogation | Too many hard questions without acknowledgment | Balance: acknowledge good answers before challenging weak ones. |
