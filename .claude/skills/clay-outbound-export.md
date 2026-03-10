# Skill: clay-outbound-export

You are an expert in preparing Clay table exports for B2B outbound sequencing tools.
When this skill is active, you structure export columns, map merge tags, and validate
data quality before any export to a sequencing platform.

---

## Export Preparation Checklist

Run through this before every export. Fix failures before proceeding.

### Data quality gates

- [ ] `verified_email` is populated for every row to be exported
- [ ] `is_valid_email = TRUE` (NeverBounce or Debounce result)
- [ ] `is_role_based_email = FALSE` (no info@, support@, admin@ addresses)
- [ ] `contact_first_name` is not null
- [ ] `contact_first_name` has no trailing spaces, numbers, or special characters
- [ ] `company_name` is not null
- [ ] All AI columns have run (no blank ai_ fields in rows to be exported)
- [ ] `icp_filter_pass = TRUE` for all rows in export
- [ ] No duplicate emails in the export set

### Name cleaning

Before export, run these checks on `contact_first_name`:
- Strip leading/trailing whitespace
- Remove numbers (e.g., "John2" → "John")
- Remove titles (e.g., "Dr. Sarah" → "Sarah")
- Capitalise first letter

Clay formula for cleaned first name:
```
=PROPER(TRIM(contact_first_name))
```

---

## Platform-Specific Export Structures

### Apollo

Apollo requires specific field names for import. Map your Clay columns to these exactly.

| Apollo field | Clay column | Notes |
|-------------|-------------|-------|
| First Name | export_first_name | Required |
| Last Name | export_last_name | Required |
| Email | export_email | Required, must be valid |
| Title | contact_title | |
| Company | company_name | |
| Company Domain | domain | Improves matching |
| LinkedIn URL | contact_linkedin_url | Helps deduplication |
| Phone | verified_phone | Optional |
| Custom Variable 1 | ai_opener | Maps to {{customVariable1}} in sequences |
| Custom Variable 2 | ai_subject_line | Maps to {{customVariable2}} |
| Custom Variable 3 | ai_company_pain | Maps to {{customVariable3}} |

**Apollo import notes:**
- Import as a static list, not a dynamic segment, when using Clay enrichment
- Set sequence step delay to minimum 3 hours after import before first send
- Custom variables in Apollo sequences use `{{customVariable1}}` format

### Smartlead

| Smartlead field | Clay column | Notes |
|----------------|-------------|-------|
| email | export_email | Required |
| first_name | export_first_name | Required |
| last_name | export_last_name | |
| company | company_name | |
| custom_opener | ai_opener | Used as {{custom_opener}} in sequence |
| custom_subject | ai_subject_line | Used as {{custom_subject}} |
| custom_pain | ai_company_pain | Used as {{custom_pain}} |
| linkedin_url | contact_linkedin_url | |
| phone | verified_phone | |

**Smartlead import notes:**
- CSV column headers must match exactly (lowercase, underscores)
- Smartlead merge tags use `{{field_name}}` syntax matching the column header
- Enable email validation in Smartlead settings before campaign launch
- Set sending limit per mailbox to max 30 emails/day for new domains

### Instantly

| Instantly field | Clay column | Notes |
|----------------|-------------|-------|
| email | export_email | Required |
| firstName | export_first_name | camelCase required |
| lastName | export_last_name | |
| companyName | company_name | |
| customVariable1 | ai_opener | {{customVariable1}} in sequence |
| customVariable2 | ai_subject_line | {{customVariable2}} |
| customVariable3 | ai_company_pain | {{customVariable3}} |
| phone | verified_phone | |
| linkedinUrl | contact_linkedin_url | |

**Instantly import notes:**
- Instantly uses camelCase headers — `firstName` not `first_name`
- Verify sending domain warmup score is above 90 before activating campaign
- Set daily send limit per mailbox to 20-25 for domains under 60 days old

### HubSpot (CRM import)

| HubSpot property | Clay column | Notes |
|-----------------|-------------|-------|
| Email | export_email | Required, maps to contact record |
| First Name | export_first_name | |
| Last Name | export_last_name | |
| Company Name | company_name | |
| Job Title | contact_title | |
| LinkedIn Bio | contact_linkedin_url | |
| Phone Number | verified_phone | |
| Website URL | domain | |
| ICP Score | ai_icp_score | Map to custom numeric property |
| Outreach Opener | ai_opener | Map to custom text property |
| Company Pain | ai_company_pain | Map to custom text property |
| Industry | company_industry | |
| Number of Employees | company_headcount | |

**HubSpot import notes:**
- Create custom contact properties in HubSpot before importing if they don't exist
- Map `ai_icp_score` to a numeric property for lead scoring workflows
- Use list membership (not workflow enrollment) for sequence triggering from Clay imports

---

## Export Column Setup in Clay

Create a dedicated export section in your table (far right columns) with these naming conventions:

```
export_email         =verified_email
export_first_name    =PROPER(TRIM(contact_first_name))
export_last_name     =PROPER(TRIM(contact_last_name))
export_opener        =ai_opener
export_subject       =ai_subject_line
export_pain          =ai_company_pain
```

Keep export columns as formula references to source columns.
Do not copy-paste values — if source data is corrected, export columns update automatically.

---

## Pre-Export View Filter

Before exporting, apply these filters in Clay table view:

```
Filter 1: is_valid_email = TRUE
Filter 2: is_role_based_email = FALSE
Filter 3: icp_filter_pass = TRUE
Filter 4: ai_opener IS NOT EMPTY
Filter 5: export_first_name IS NOT EMPTY
```

Export only the rows visible after all five filters are applied.
Export only the `export_` prefixed columns plus `contact_linkedin_url` for deduplication.

---

## Volume and Sending Guidelines

| Sequence tool | Max import size | Daily send limit per mailbox | Recommended batch size |
|--------------|----------------|------------------------------|------------------------|
| Apollo | No hard limit | 50-100 (warmed) | 500-1000 per campaign |
| Smartlead | No hard limit | 30-50 (warmed) | 200-500 per mailbox/day |
| Instantly | No hard limit | 20-30 (new), 40-50 (warmed) | 200-400 per mailbox/day |

For ABM campaigns: max 50-100 accounts per sequence, highly personalised.
For broad outbound: 500-2000 per wave, with A/B subject line testing.

---

## Post-Export Validation

After importing to sequencing tool, verify before activating:

1. Open 10 random contacts and check merge tags render correctly
2. Confirm subject line field is mapped (not blank)
3. Confirm opener field is mapped
4. Send a test email to yourself with all merge tags
5. Check that no merge tag shows as `{{undefined}}` or `{{null}}`
6. Confirm unsubscribe link is present in sequence footer
