# Metrics & KPIs — Measuring SDD Success

> How to measure whether the SDD workflow is delivering value. Organized by category with data sources and targets.

---

## Measurement Philosophy

- **Measure outcomes, not compliance.** "Are we building better products?" matters more than "Did everyone fill out the template?"
- **Lag indicators confirm, lead indicators predict.** Track both.
- **Start simple.** Begin with 3-4 metrics and expand as the workflow matures.
- **No vanity metrics.** Every metric must answer a question that changes behavior.

---

## Priority Metrics (Start Here)

These 4 metrics are the minimum viable measurement set. Begin tracking these from Week 3.

### 1. Cycle Time: Idea → Approved Spec

**What**: Calendar days from Problem Brief creation to specification approval.

**Why**: Measures whether the workflow accelerates or slows down the path from idea to action.

**How to measure**: Date on Problem Brief (created) vs. date on specification (approved).

**Target**:
- Tier 2: ≤10 business days
- Tier 3: ≤20 business days

**Baseline**: Measure the first 3 features to establish the team's starting point.

| Feature | Tier | Brief Created | Spec Approved | Cycle Time |
|---------|------|--------------|---------------|------------|
| | | | | |

---

### 2. Spec Quality: Rework Rate

**What**: Number of significant spec revisions after initial approval (changes to requirements or acceptance criteria, not typo fixes).

**Why**: A spec that needs rework after approval indicates the Discovery/Readiness process didn't catch something.

**How to measure**: Count spec revisions that change requirements or acceptance criteria after the spec was first approved.

**Target**: ≤1 significant revision per spec.

| Feature | Spec Revisions (Post-Approval) | Reason for Revision |
|---------|-------------------------------|-------------------|
| | | |

---

### 3. Triage Accuracy

**What**: Percentage of features that stay at their triaged tier throughout the lifecycle (not re-tiered later).

**Why**: Frequent re-tiering means the triage criteria or the PM's assessment skills need calibration.

**How to measure**: Compare initial triage tier vs. final tier at ship time.

**Target**: ≥80% accurate (no re-tiering).

| Feature | Initial Tier | Final Tier | Accurate? | Notes |
|---------|-------------|-----------|-----------|-------|
| | | | | |

---

### 4. Discovery Effectiveness: Assumption Validation

**What**: Of the high-risk assumptions identified during Discovery, how many were validated before specification?

**Why**: The whole point of Discovery is to find and validate assumptions early. If high-risk assumptions go unvalidated into specs, Discovery isn't working.

**How to measure**: Count high-risk assumptions in Assumption Maps. Count those validated before spec.

**Target**: 100% of high-risk (high impact, low confidence) assumptions validated before specification.

| Feature | High-Risk Assumptions | Validated Before Spec | Rate |
|---------|----------------------|----------------------|------|
| | | | |

---

## Secondary Metrics (Add After Quarter 1)

### 5. Compliance Incident Rate

**What**: Number of compliance issues found in production (missing audit events, broken approval workflows, SOX control failures).

**Why**: The constitution and compliance checkpoints exist to prevent these. If incidents still occur, the compliance process has gaps.

**How to measure**: Count production incidents tagged as compliance/audit/SOX.

**Target**: Zero compliance incidents for features that went through the full SDD workflow.

---

### 6. Constitution Coverage

**What**: Percentage of specifications that reference applicable constitution articles.

**Why**: Ensures the constitution is being used, not ignored.

**How to measure**: Review specs for constitution compliance section completeness.

**Target**: 100% of specs have a completed Constitution Compliance section.

---

### 7. Adoption Rate

**What**: Percentage of Tier 2/3 features that follow the full workflow (Problem Brief → Discovery → Spec Readiness → Specification).

**Why**: Measures whether the team is actually using the process.

**How to measure**: GPM reviews Portfolio Dashboard weekly. Count features with complete artifact chains vs. those that skipped steps.

**Target**: 100% for Tier 3, ≥80% for Tier 2 (some Tier 2 features may legitimately skip Discovery).

---

### 8. Retrofit Progress

**What**: Percentage of existing capabilities documented with Capability Records.

**Why**: Institutional memory for existing capabilities is critical for informed investment decisions.

**How to measure**: Count Capability Records vs. total capabilities in the Product Capability Map.

**Target**: 2-3 new Capability Records per PM per quarter. 80% coverage within 4 quarters.

---

## Tracking & Reporting

### Weekly (GPM)
- Review Portfolio Dashboard for process health indicators
- Note any features that skipped steps or seem stalled

### Monthly (GPM)
- Calculate Priority Metrics (1-4) for features completed that month
- Share metrics in team meeting (2 minutes, not a long review)
- Identify patterns: are certain types of features taking longer? Which assumptions keep invalidating?

### Quarterly (GPM + Team)
- Full metrics review including Secondary Metrics (5-8)
- Compare against targets
- Retrospective: what's improving, what's not
- Adjust targets based on evidence (not ambition)

---

## Metrics Dashboard Template

| Metric | This Month | Last Month | Trend | Target | Status |
|--------|-----------|-----------|-------|--------|--------|
| Cycle Time (Tier 2 avg) | [days] | [days] | [↑/→/↓] | ≤10 days | [On/Off track] |
| Cycle Time (Tier 3 avg) | [days] | [days] | [↑/→/↓] | ≤20 days | [On/Off track] |
| Spec Rework Rate | [revisions/spec] | | [↑/→/↓] | ≤1 | |
| Triage Accuracy | [%] | [%] | [↑/→/↓] | ≥80% | |
| Assumption Validation | [%] | [%] | [↑/→/↓] | 100% | |
| Compliance Incidents | [count] | [count] | [↑/→/↓] | 0 | |
| Constitution Coverage | [%] | [%] | [↑/→/↓] | 100% | |
| Adoption Rate | [%] | [%] | [↑/→/↓] | ≥80% | |
| Retrofit Progress | [count/total] | | [↑/→/↓] | 2-3/PM/Q | |

---

## What to Do When Metrics Are Off

| Metric Off Track | Likely Cause | Remedy |
|-----------------|-------------|--------|
| Cycle time too long | Discovery taking too long, or PMs not prioritizing workflow | Check: are PMs pausing Discovery to gather info? That's OK. Are they just not starting? That's a prioritization issue. |
| High rework rate | Discovery not surfacing the right questions | Review: which questions are being missed? Update the question bank. |
| Low triage accuracy | Triage criteria unclear, or PMs not using the GPT for triage | Recalibrate: review mis-tiered features with the team. Update triage criteria. |
| Assumptions going unvalidated | Validation plans too ambitious, or no time allocated | Simplify: validation doesn't mean a 2-week study. A 15-min conversation with a user counts. |
| Compliance incidents | Compliance checkpoint being skipped or rubber-stamped | Investigate: was the Compliance Impact Assessment complete? Was it reviewed? |
| Low adoption | Process feels like overhead, not value | Return to basics: is the GPT producing useful output? Refine the prompt. Lead with the case study again. |
