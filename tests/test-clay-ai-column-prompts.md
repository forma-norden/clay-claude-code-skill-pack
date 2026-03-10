# Test: clay-ai-column-prompts

## Setup
Load skill: "Read .claude/skills/clay-ai-column-prompts.md"

---

## Test 1 — Subject Line Prompt

### Prompt
```
Write a Clay AI column prompt to generate a cold email subject line.

Available columns in my table:
- contact_first_name
- contact_title
- company_name
- company_headcount
- company_tech_stack
- ai_company_pain (from a previous AI column)

The sequence is for a GTM systems agency. Targets are VP Sales and founders.
Subject lines must be under 8 words, no questions, lowercase preferred.
```

### Validation Checklist

- [ ] Prompt follows ROLE / CONTEXT / TASK / FORMAT / FALLBACK structure
- [ ] Uses at least 3 of the available column variables via {{column_name}} syntax
- [ ] Output format explicitly specified (plain text, one line, under 8 words)
- [ ] Fallback instruction present for when ai_company_pain is empty
- [ ] Prompt explicitly bans generic phrases ("Quick question", "Touching base", etc.)
- [ ] Prompt does not ask for multiple outputs
- [ ] Output of the prompt would be directly usable as a subject line with no editing
- [ ] No em dashes in any example subject lines generated

### Pass/Fail notes
__________________________________________________

---

## Test 2 — ICP Score Prompt

### Prompt
```
Write a Clay AI column prompt to score accounts from 0-100 on ICP fit.

ICP: B2B SaaS, 50-500 employees, using Apollo or HubSpot or Salesforce,
active outbound motion (hiring SDRs or using Outreach/Salesloft).

Available columns: company_name, company_headcount, company_industry,
company_tech_stack, company_open_roles, company_funding_stage.

Output must be a number only.
```

### Validation Checklist

- [ ] Prompt includes all ICP criteria explicitly
- [ ] Positive signals listed (tech stack match, hiring signals)
- [ ] Negative signals listed (wrong industry, wrong size, etc.)
- [ ] Output instruction says "Return only the number. No explanation. No text."
- [ ] Fallback for insufficient data returns a neutral score (not 0, not 100)
- [ ] All available columns used as context variables
- [ ] Prompt would be cheap to run (scoring = Haiku/Mini appropriate, noted or implied)

### Pass/Fail notes
__________________________________________________

---

## Test 3 — Prompt Debugging

### Prompt
```
My AI opener column keeps returning generic output like:
"I noticed your company is growing and I wanted to reach out about our services."

Here is my current prompt:
"Write a personalised opener for {{contact_first_name}} at {{company_name}}."

Fix it.
```

### Validation Checklist

- [ ] Diagnoses the problem: only 2 variables, no specific data injected
- [ ] Rewrites prompt with at least 4-5 column variables
- [ ] Adds explicit instruction: be specific to this company, not generic
- [ ] Adds explicit ban on phrases like "I noticed" and "I wanted to reach out"
- [ ] Adds format constraint (max word count)
- [ ] Adds fallback instruction for empty columns
- [ ] New prompt would produce noticeably more specific output than original

### Pass/Fail notes
__________________________________________________
