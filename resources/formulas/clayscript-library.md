# Clayscript Formula Library

Copy-paste formulas for Clay workflows. Clayscript costs 0 credits.
Always prefer formulas over AI columns for data manipulation.

## Data Cleaning

### Clean Email Domain
```
IF({email} != null, LOWER(SPLIT({email}, "@")[1]), "")
```

### Extract First Name
```
IF({full_name} != null, SPLIT(TRIM({full_name}), " ")[0], "")
```

### Normalize Company Name (remove Inc, LLC, etc.)
```
IF({company} != null,
  TRIM(REPLACE(REPLACE(REPLACE(REPLACE(LOWER({company}),
    " inc.", ""), " llc", ""), " ltd", ""), " corp", "")),
  "")
```

### Clean URL to Domain
```
IF({url} != null,
  REPLACE(REPLACE(REPLACE(LOWER({url}),
    "https://", ""), "http://", ""), "www.", ""),
  "")
```

## Conditional Enrichment Gates

### Skip if Email Already Exists
```
IF({email} != null && {email} != "", "skip", "run")
```

### Skip if Phone Already Exists
```
IF({phone} != null && {phone} != "", "skip", "run")
```

### Skip if Any Enrichment Data Exists
```
IF({email} != null || {phone} != null || {company_size} != null, "skip", "run")
```

### Run Only for Target Segments
```
IF({employee_count} >= 50 && {employee_count} <= 500 && {industry} != null, "run", "skip")
```

## Lead Scoring Formulas

### Simple ICP Score (0-100)
```
(IF({employee_count} >= 50 && {employee_count} <= 500, 30, 0))
+ (IF({industry} == "SaaS" || {industry} == "Technology", 25, 0))
+ (IF({title} CONTAINS "VP" || {title} CONTAINS "Director" || {title} CONTAINS "Head", 25, 0))
+ (IF({signal_type} != null, 20, 0))
```

### Signal Freshness Score
```
IF({signal_date} != null,
  IF(DATEDIFF(NOW(), {signal_date}, "days") <= 7, 100,
    IF(DATEDIFF(NOW(), {signal_date}, "days") <= 14, 80,
      IF(DATEDIFF(NOW(), {signal_date}, "days") <= 30, 50,
        IF(DATEDIFF(NOW(), {signal_date}, "days") <= 60, 25, 10)))),
  0)
```

## Waterfall Result Selection

### Pick First Non-Empty Email
```
IF({email_provider_1} != null && {email_provider_1} != "", {email_provider_1},
  IF({email_provider_2} != null && {email_provider_2} != "", {email_provider_2},
    IF({email_provider_3} != null && {email_provider_3} != "", {email_provider_3},
      "not_found")))
```

### Combine Phone Results
```
IF({phone_provider_1} != null, {phone_provider_1},
  IF({phone_provider_2} != null, {phone_provider_2},
    IF({phone_provider_3} != null, {phone_provider_3}, "not_found")))
```

## Data Formatting for Export

### Format for Apollo Import
```
{first_name} & "," & {last_name} & "," & {email} & "," & {company} & "," & {title}
```

### Boolean to Ready Status
```
IF({email} != null && {email} != "not_found" && {icp_score} >= 50, "ready_to_send", "needs_review")
```
