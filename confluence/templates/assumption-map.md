# Assumption Map Template

> Copy this template when starting Discovery for a Problem Brief. Used to systematically capture, rate, and validate assumptions.

---

## Header

| Field | Value |
|-------|-------|
| **Related Problem Brief** | [Link to Problem Brief] |
| **Feature** | [Feature name] |
| **Created By** | [Name] |
| **Date Created** | [YYYY-MM-DD] |
| **Last Updated** | [YYYY-MM-DD] |

---

## Assumptions Table

| ID | Assumption | Category | Confidence | Impact | Priority | Evidence | Validation Method | Status |
|----|-----------|----------|------------|--------|----------|----------|-------------------|--------|
| A1 | [Statement of what you believe to be true] | [User / Technical / Business / Regulatory] | [High / Med / Low] | [High / Med / Low] | [From 2x2 grid] | [What evidence supports this?] | [How will you validate?] | [Unvalidated / Validating / Validated / Invalidated] |
| A2 | | | | | | | | |
| A3 | | | | | | | | |

**Category definitions:**
- **User**: Assumptions about user behavior, needs, adoption
- **Technical**: Assumptions about feasibility, performance, architecture
- **Business**: Assumptions about market, revenue, stakeholders
- **Regulatory**: Assumptions about compliance, legal, audit requirements

---

## 2x2 Priority Grid

Plot each assumption by Confidence (x-axis) and Impact (y-axis):

```
                    HIGH IMPACT
                        │
    ┌───────────────────┼───────────────────┐
    │                   │                   │
    │   VALIDATE NOW    │     MONITOR       │
    │   (Low Conf,      │   (High Conf,     │
    │    High Impact)    │    High Impact)    │
    │                   │                   │
    │   These are       │   These are       │
    │   blockers.       │   important but   │
    │                   │   you believe     │
    │                   │   they're true.   │
    ├───────────────────┼───────────────────┤
    │                   │                   │
    │   ACCEPT WITH     │     ACCEPT        │
    │   CAVEAT          │   (High Conf,     │
    │   (Low Conf,      │    Low Impact)     │
    │    Low Impact)     │                   │
    │                   │   These are safe  │
    │   Note them but   │   to assume.      │
    │   don't block.    │                   │
    │                   │                   │
    └───────────────────┼───────────────────┘
  LOW CONFIDENCE        │         HIGH CONFIDENCE
                    LOW IMPACT
```

### Validate Now (High Impact, Low Confidence)
| ID | Assumption | Validation Plan | Target Date |
|----|-----------|-----------------|-------------|
| | | | |

### Monitor (High Impact, High Confidence)
| ID | Assumption | Monitoring Approach |
|----|-----------|-------------------|
| | | |

### Accept with Caveat (Low Impact, Low Confidence)
| ID | Assumption | Caveat |
|----|-----------|--------|
| | | |

### Accept (Low Impact, High Confidence)
| ID | Assumption |
|----|-----------|
| | |

---

## Validation Priority Queue

Ordered list of assumptions to validate, starting with the highest-priority (high impact, low confidence):

1. **[A#]**: [Assumption] → **Validation method**: [How] → **Owner**: [Who] → **By when**: [Date]
2. **[A#]**: [Assumption] → **Validation method**: [How] → **Owner**: [Who] → **By when**: [Date]
3. ...

---

## Decision Triggers

Define what happens when assumptions are validated or invalidated:

| Assumption ID | If Validated | If Invalidated |
|--------------|-------------|----------------|
| A1 | [Continue as planned] | [Pivot: describe alternative] |
| A2 | [Continue as planned] | [Kill: describe why this invalidates the feature] |
| A3 | | |

---

## Status History

| Date | Update |
|------|--------|
| [YYYY-MM-DD] | Assumption Map created with [N] assumptions |
| | |
