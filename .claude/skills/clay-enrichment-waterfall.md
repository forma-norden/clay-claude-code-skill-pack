# Skill: clay-enrichment-waterfall

You are an expert in Clay enrichment integrations and waterfall logic.
When this skill is active, you design, build, and debug enrichment waterfalls
that maximise data coverage while minimising credit spend.

---

## What Is a Waterfall

A waterfall is a sequence of enrichment providers configured so Clay tries Provider A first,
and only runs Provider B if Provider A returns no result. This maximises coverage without
paying for redundant lookups.

Clay supports waterfalls natively using the "Fallback" column feature or via conditional
column logic (IF EMPTY, run next provider).

---

## Email Finding Waterfall

### Provider quality and cost reference (2026)

| Provider | Quality | Cost per find | Best for |
|----------|---------|--------------|----------|
| Apollo | High | ~$0.01-0.02 | US tech companies, SMB-enterprise |
| Hunter | High | ~$0.02-0.03 | European companies, agencies |
| Prospeo | High | ~$0.02-0.03 | LinkedIn-based finding |
| Anymailfinder | Medium | ~$0.01-0.02 | Broad coverage, lower accuracy |
| Dropcontact | High | ~$0.03-0.05 | French/European companies specifically |
| Datagma | Medium | ~$0.02 | Mobile + email combined |

### Recommended email waterfall (ordered)

```
Step 1: apollo_email          — run always
Step 2: hunter_email          — run IF apollo_email IS EMPTY
Step 3: prospeo_email         — run IF hunter_email IS EMPTY
Step 4: anymailfinder_email   — run IF prospeo_email IS EMPTY
Step 5: verified_email        — formula: COALESCE(apollo, hunter, prospeo, anymailfinder)
Step 6: email_verified        — run NeverBounce or Debounce on verified_email
Step 7: is_valid_email        — Boolean: email_verified = "valid" OR "catch-all"
```

**Do not send to catch-all emails** without additional phone or LinkedIn validation.
**Reject risky** emails (role-based: info@, support@, admin@) before export.

### Role-based email filter

```
is_role_based_email  [Boolean]
Formula: CONTAINS(verified_email, "info@") 
      OR CONTAINS(verified_email, "support@")
      OR CONTAINS(verified_email, "admin@")
      OR CONTAINS(verified_email, "contact@")
      OR CONTAINS(verified_email, "hello@")
```

Filter out `is_role_based_email = true` before sending to any sequence.

---

## Phone Finding Waterfall

| Provider | Quality | Cost | Best for |
|----------|---------|------|----------|
| Datagma | High | ~$0.05 | Mobile direct dials, US/EU |
| Prospeo | Medium | ~$0.03 | Mobile + LinkedIn combined |
| Apollo | Medium | ~$0.02 | US office/direct dials |

```
Step 1: datagma_phone         — run for high-priority accounts only
Step 2: prospeo_phone         — run IF datagma_phone IS EMPTY
Step 3: apollo_phone          — run IF prospeo_phone IS EMPTY
Step 4: verified_phone        — formula: COALESCE(datagma, prospeo, apollo)
```

Only run phone enrichment on accounts that pass ICP filter AND have a verified email.
Phone enrichment is expensive — do not run it on every row.

---

## Firmographic Enrichment Waterfall

### Company data

| Provider | Best data | Cost |
|----------|----------|------|
| Clearbit (Breeze) | Revenue, tech stack, funding, headcount accuracy | ~$0.05-0.10 |
| Apollo | Headcount, industry, basic firmographics | ~$0.01-0.02 |
| LinkedIn (via Clay) | Headcount, description, followers | Free (scrape) |
| Crunchbase | Funding rounds, investors, founding date | ~$0.03-0.05 |

```
Step 1: apollo_headcount       — run always (cheap)
Step 2: clearbit_headcount     — run IF apollo_headcount IS EMPTY OR use_precise = true
Step 3: linkedin_headcount     — run IF clearbit_headcount IS EMPTY
Step 4: final_headcount        — formula: COALESCE(clearbit, apollo, linkedin)
```

For revenue and funding — Clearbit is the primary. Crunchbase as fallback for funded startups.

### Tech stack enrichment

```
Step 1: builtwith_tech_stack   — primary (best coverage for web tech)
Step 2: clearbit_tech_stack    — fallback (good for SaaS tools, CRM)
Step 3: apollo_tech_stack      — fallback (limited but free with Apollo account)
```

---

## Enrichment Waterfall Configuration in Clay

### Method 1: Native Fallback (recommended)

1. Create the first enrichment column (e.g., Apollo Email Finder)
2. Click the column settings → "Add Fallback"
3. Select the fallback provider (e.g., Hunter)
4. Add additional fallbacks in sequence
5. Clay runs each provider in order until it finds a result

### Method 2: Formula-based COALESCE

When providers are in separate columns:
```
=IF(apollo_email <> "", apollo_email,
  IF(hunter_email <> "", hunter_email,
    IF(prospeo_email <> "", prospeo_email, "")))
```

Use Method 1 when possible — it is cleaner and uses fewer column slots.
Use Method 2 when you need to audit which provider found the data.

---

## Conditional Enrichment (Cost Gating)

The single most important cost control in Clay is conditional enrichment.
Never run expensive lookups on rows that will be filtered out anyway.

### Pattern

```
Column: icp_filter_pass      [Boolean]   Set this first
Column: clearbit_data        [Object]    Condition: IF icp_filter_pass = TRUE
Column: phone_data           [Object]    Condition: IF icp_filter_pass = TRUE AND verified_email IS NOT EMPTY
Column: ai_columns           [Text]      Condition: IF icp_filter_pass = TRUE AND is_valid_email = TRUE
```

### Setting conditions in Clay

Each enrichment column has a "Run column when" setting.
Set it to reference the filter Boolean before activating the column.

---

## Enrichment Coverage Audit

When auditing an existing table, check these metrics:

| Metric | Target | Action if below target |
|--------|--------|----------------------|
| Email found rate | > 60% | Add fallback provider |
| Email valid rate | > 85% of found | Switch primary provider |
| Headcount filled | > 90% | Add LinkedIn as fallback |
| Tech stack filled | > 70% | Add BuiltWith |
| AI column run rate | = ICP match rate | Check conditional logic |

---

## Common Waterfall Mistakes

1. **Running all providers in parallel** — wastes credits, no waterfall benefit
2. **Not verifying emails after finding** — leads to bounces and domain damage
3. **Using company name as enrichment input** — use domain or LinkedIn URL
4. **Running phone enrichment on every row** — run only on priority accounts
5. **No fallback on email finding** — single provider rarely exceeds 50-60% coverage
6. **Enriching before filtering** — always filter first, enrich after
