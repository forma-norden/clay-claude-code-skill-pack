# Test: clay-enrichment-waterfall

## Setup
Load skill: "Read .claude/skills/clay-enrichment-waterfall.md"

---

## Test 1 — Email Waterfall Design

### Prompt
```
I'm building an email finding waterfall for a prospecting table.
My list has 3000 contacts from US and European tech companies.
I have accounts with Apollo and Hunter. Budget is flexible but I want
to maximise valid email coverage. Walk me through the full waterfall setup.
```

### Validation Checklist

- [ ] Waterfall includes Apollo as Step 1
- [ ] Hunter as fallback (Step 2)
- [ ] At least one additional fallback provider suggested
- [ ] COALESCE formula or native fallback method explained
- [ ] Email verification step included (NeverBounce or Debounce)
- [ ] Role-based email filter defined
- [ ] is_valid_email Boolean column described
- [ ] Coverage target stated (should reference 60%+ find rate)
- [ ] Sends to catch-all emails flagged as a risk

### Pass/Fail notes
__________________________________________________

---

## Test 2 — Enrichment Audit

### Prompt
```
I have an existing Clay table with 1500 rows. Email found rate is 38%.
Valid email rate is 71% of found emails. Tech stack data is missing for
60% of rows. What's wrong and how do I fix it?
```

### Validation Checklist

- [ ] 38% email find rate correctly diagnosed as below target (target is 60%+)
- [ ] Suggests adding additional fallback providers to email waterfall
- [ ] 71% valid rate correctly diagnosed as low (target 85%+)
- [ ] Suggests switching primary provider or improving source data quality
- [ ] 60% missing tech stack correctly diagnosed
- [ ] Suggests BuiltWith as primary and Clearbit as fallback
- [ ] Does not suggest running all providers simultaneously
- [ ] Provides concrete fix steps, not just diagnosis

### Pass/Fail notes
__________________________________________________
