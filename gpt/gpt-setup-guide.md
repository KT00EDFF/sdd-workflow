# GPT Setup Guide — Product Catalog SDD Companion

> Step-by-step instructions for creating the Custom GPT in ChatGPT. The GPM should do this during Week 1-2 of rollout.

---

## Prerequisites

- ChatGPT Plus or Team account with Custom GPT access
- All knowledge files from the `gpt/knowledge/` directory
- The system prompt from `gpt/system-prompt.md`

---

## Step 1: Create the GPT

1. Go to [ChatGPT](https://chat.openai.com)
2. Click your profile icon → **My GPTs** → **Create a GPT**
3. Select the **Configure** tab (skip the conversational builder)

---

## Step 2: Basic Settings

| Field | Value |
|-------|-------|
| **Name** | Product Catalog SDD Companion |
| **Description** | SDD workflow companion for the JPMC Product Catalog team. Triage ideas, run Discovery sessions, and assess Spec Readiness. |
| **Instructions** | Paste the entire contents of `gpt/system-prompt.md` |

---

## Step 3: Conversation Starters

Add these three conversation starters (one per mode):

1. `I have a new idea/request to triage`
2. `I have a Problem Brief and need Discovery help`
3. `Is my feature ready for spec-kit?`

---

## Step 4: Upload Knowledge Files

Upload all 6 files from `gpt/knowledge/`:

| File | Purpose |
|------|---------|
| `product-catalog-domain.md` | Domain model, entities, relationships, regulatory context |
| `sdd-methodology.md` | SDD methodology summary |
| `speckit-reference.md` | spec-kit commands, conventions, template structure |
| `triage-criteria.md` | Tier 1/2/3 definitions and classification axes |
| `discovery-question-bank.md` | 4-stage critical questioning library |
| `spec-readiness-checklist.md` | 20-item assessment rubric |

**Upload order doesn't matter** — ChatGPT indexes all files equally.

**Optional additional knowledge files** (add these as they become available):
- Team OKRs or product strategy document
- Constitution document (`speckit/constitution.md`)
- Examples of good Problem Briefs (once real ones exist)

---

## Step 5: Capabilities

Configure these settings:

| Capability | Setting | Reason |
|-----------|---------|--------|
| **Web Browsing** | OFF | GPT should work from uploaded knowledge, not web search |
| **DALL-E Image Generation** | OFF | Not needed for this use case |
| **Code Interpreter** | OFF | Not needed — this GPT doesn't write code |

---

## Step 6: Sharing

| Option | Recommended Setting |
|--------|-------------------|
| **Who can use this GPT** | "Only people with a link" or "Only [team workspace]" |

Share the link with all PMs on the Product Catalog team.

---

## Step 7: Testing

Test each mode with the following inputs:

### Test Mode 1 (Triage)

**Input**:
> "We need to add a new billing code type for cryptocurrency products. It would require a new taxonomy branch and new billing rules."

**Expected behavior**:
- GPT asks clarifying questions about scope, API impact, and regulatory weight
- Classifies as Tier 2 or Tier 3 (likely Tier 3 due to new taxonomy branch + new domain concept)
- Flags regulatory implications
- Recommends creating a Problem Brief

### Test Mode 2 (Discovery)

**Input**:
> "I have a Problem Brief for bulk importing billing codes. PMs currently add codes one at a time through the Admin UI and it takes 4+ hours per quarterly update."

**Expected behavior**:
- GPT enters Discovery mode
- Runs through 4 stages of questioning
- Challenges vague answers
- Produces a structured Discovery Summary
- Identifies assumptions and rates them

### Test Mode 3 (Spec Readiness)

**Input**:
> "Run a spec readiness check. The feature is bulk import of billing codes via CSV. Problem: PMs waste 4+ hours adding codes manually. V1 scope: CSV upload to API, all-or-nothing validation, max 500 codes, audit events per code. Success: 15 min instead of 4 hours. Tech lead confirmed feasibility."

**Expected behavior**:
- GPT scores all 20 checklist items
- Produces a summary with Green/Yellow/Red counts
- Makes a readiness recommendation
- If Ready/Almost Ready: generates a draft `/speckit.specify` prompt
- Prompt references constitution articles

---

## Troubleshooting

### GPT doesn't detect the mode
The mode router depends on trigger phrases. If the PM's input doesn't match, the GPT should ask which mode to use. If it doesn't, check that the system prompt's Layer 2 (Mode Router) section is complete.

### GPT is too passive
The system prompt explicitly instructs directive behavior. If the GPT is just asking questions without challenging, regenerate the response — sometimes GPT falls into a passive pattern. If it persists, add emphasis to the "Stance" sections in the system prompt.

### GPT hallucinates domain details
Ensure all knowledge files are uploaded. If the GPT invents entities or rules not in the domain model, it may not be referencing the uploaded knowledge. Re-upload `product-catalog-domain.md` and test again.

### GPT generates code
The system prompt explicitly says "You are NOT an implementation tool." If this happens, add a reinforcement in the Instructions: "NEVER generate code, SQL, or implementation details. You help with WHAT and WHY, not HOW to code it."

### Spec Readiness prompt doesn't reference constitution
Check that `speckit-reference.md` and the constitution are in the knowledge files. The system prompt's Mode 3 instructions explicitly map features to constitution articles.

---

## Updating the GPT

### When to update
- After rollout retro (Week 6) — incorporate feedback
- When the constitution is amended
- When triage criteria are recalibrated
- When new domain entities are added
- When the question bank is refined

### How to update
1. Go to **My GPTs** → click on the GPT → **Edit**
2. Update the Instructions (system prompt) if behavior needs to change
3. Upload new/updated knowledge files (old files can be deleted and re-uploaded)
4. Test all three modes after any update

### Version tracking
Keep a log of GPT updates:

| Date | Change | Reason |
|------|--------|--------|
| [Date] | Initial creation | Rollout Week 1 |
| | | |
