# clay-enrichment-debugger

Use this skill to troubleshoot Clay workflow issues, prevent credit waste,
and fix common mistakes.

## Common Issues and Fixes

### 1. Enrichment Not Returning Results

Check sequence:
1. Verify the input column has valid data (no nulls, correct format)
2. Check if conditional run formula is blocking execution
3. Test with 5 rows manually to isolate the provider
4. Review provider-specific input requirements (some need domain, some need full URL)

### 2. Credits Wasting on Existing Data

Root cause: missing conditional check before enrichment.

Fix: Add a Clayscript formula column before every paid enrichment:
```
IF({email} != null && {email} != "", "skip", "run")
```
Set the enrichment to run conditionally on this column = "run".

### 3. Auto-Update Running on All Rows

Root cause: auto-update is on without row-level change detection.

Fix:
1. Turn off auto-update on production tables
2. Use manual triggers or n8n webhooks for controlled updates
3. If auto-update is needed, add a "last_updated" date column and conditional formula

### 4. AI Column Producing Inconsistent Output

Common causes:
- Prompt too vague (missing examples of good/bad output)
- Wrong model selected (using GPT-4 for simple extraction)
- Input data quality varies (some rows have full data, others are sparse)

Fix:
1. Add 3 examples of expected output directly in the prompt
2. Use GPT-4 Mini for extraction and classification tasks
3. Add a fallback value for rows with insufficient input data
4. Test on 10 rows before running full table

### 5. Waterfall Returning Low Coverage

Check sequence:
1. Verify provider ordering is cheapest first, highest quality last
2. Check that conditional runs are passing data between providers correctly
3. Confirm each provider is receiving the correct input field
4. Review coverage by provider to identify which one is underperforming

## Execution Sequence

1. Identify the symptom from the user's description.
2. Match to the diagnostic checklist above.
3. Walk through the checklist steps in order.
4. Provide the exact fix with Clayscript formula or configuration change.
5. Estimate credits already wasted and suggest prevention controls.

## Output Contract

Return:

- diagnosed root cause with confidence level
- exact fix (formula, setting change, or workflow restructure)
- credits at risk if unfixed
- prevention checklist to avoid recurrence

## Anti-Patterns

- running enrichments without conditional checks
- using auto-update on tables with 1,000+ rows
- selecting GPT-4 or Claude for tasks GPT-4 Mini handles
- not testing with a small batch before full-table runs
- storing enriched data only in Clay without CRM/Supabase backup
