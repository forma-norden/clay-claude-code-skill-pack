# Test: clay-table-architect

## Setup
Load skill: "Read .claude/skills/clay-table-architect.md"

---

## Test 1 — Outbound Prospecting Table

### Prompt
```
I need a Clay table for outbound prospecting.

ICP: B2B SaaS companies, 50-300 employees, US and UK, with a sales team.
Persona: VP Sales, Head of Sales, Sales Director.
Outbound destination: Apollo for email sequences.
Budget: approx 2000 rows per month, want to keep cost under $150/month.

Design the full table structure.
```

### Validation Checklist

- [ ] Column list includes at minimum: domain, company firmographics, contact identity,
      at least 2 email enrichment providers, verified_email, is_valid_email,
      at least 2 AI columns, and export_ columns
- [ ] Columns are in the correct order (input → firmographic → contact → enrichment → AI → export)
- [ ] Column names use snake_case
- [ ] An ICP filter column is present before expensive enrichment
- [ ] Email waterfall has at least 2 providers with fallback logic described
- [ ] AI columns are gated behind ICP filter
- [ ] Export columns map to Apollo field names
- [ ] Cost estimate is provided (should be under $150 for 2000 rows if designed correctly)
- [ ] No column suggests running all enrichment providers in parallel

### Pass/Fail notes
__________________________________________________

---

## Test 2 — ABM Account Enrichment Table

### Prompt
```
I have a list of 200 target accounts from our CRM. I need to enrich them with
firmographic data, tech stack, funding info, and identify the best outreach angle
for each account before handing to our AEs.

ICP: Series A-C SaaS companies, 100-500 employees, using Salesforce or HubSpot.
No outbound sequence — this goes directly to AEs in HubSpot as account intelligence.

Design the table.
```

### Validation Checklist

- [ ] Table uses `domain` as primary identifier
- [ ] Includes tech stack enrichment (BuiltWith or Clearbit)
- [ ] Includes funding enrichment (Clearbit or Crunchbase)
- [ ] Includes an `account_uses_target_tech` Boolean checking for Salesforce/HubSpot
- [ ] Includes at least one AI research column (account pain or outreach angle)
- [ ] ICP fit score column present
- [ ] Export structure maps to HubSpot properties
- [ ] Cost estimate provided

### Pass/Fail notes
__________________________________________________
