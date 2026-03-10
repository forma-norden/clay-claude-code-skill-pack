# Test: clay-outbound-export

## Setup
Load skill: "Read .claude/skills/clay-outbound-export.md"

---

## Test 1 — Apollo Export Setup

### Prompt
```
I'm ready to export 800 contacts from Clay to Apollo for a cold email sequence.
My table has these columns:
- contact_first_name, contact_last_name, contact_title
- company_name, domain
- verified_email, is_valid_email, is_role_based_email
- contact_linkedin_url
- ai_opener, ai_subject_line, ai_company_pain
- icp_filter_pass, ai_icp_score

Walk me through the full export process including the pre-export filters,
column mapping, and how to use the AI columns as merge tags in Apollo.
```

### Validation Checklist

- [ ] Pre-export filters specified: is_valid_email = TRUE, is_role_based_email = FALSE,
      icp_filter_pass = TRUE, ai_opener not empty
- [ ] Apollo field name mapping provided (First Name, Last Name, Email, etc.)
- [ ] Custom variable mapping explained (customVariable1, customVariable2, customVariable3)
- [ ] Merge tag syntax in Apollo explained ({{customVariable1}})
- [ ] Export column setup described (export_ prefix columns as formula references)
- [ ] Post-export validation steps listed
- [ ] Daily send limits referenced
- [ ] Test email step mentioned before activating sequence

### Pass/Fail notes
__________________________________________________

---

## Test 2 — Name Cleaning

### Prompt
```
My contact_first_name column has bad data. Some examples:
- "john " (trailing space)
- "SARAH" (all caps)
- "Dr. Michael" (title prefix)
- "James3" (number appended)
- " emily" (leading space)

What Clay formula cleans all of these for export?
```

### Validation Checklist

- [ ] PROPER() function used for capitalisation
- [ ] TRIM() function used for whitespace
- [ ] Provides a working Clay formula (not pseudocode)
- [ ] Addresses or notes the Dr. prefix issue (even if manual flag)
- [ ] Addresses the number suffix issue (even if manual flag or REGEX note)
- [ ] Output formula is immediately usable in a Clay formula column

### Pass/Fail notes
__________________________________________________
