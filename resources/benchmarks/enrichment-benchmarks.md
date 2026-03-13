# Enrichment Benchmarks

Performance data for Clay enrichment workflows. Use to set expectations,
compare providers, and diagnose coverage issues.

## Email Coverage by Provider

| Provider | Coverage (Standalone) | Credit Cost | Best For |
|----------|----------------------|-------------|----------|
| Apollo | 35-45% | 1 | B2B email, broad coverage |
| Hunter | 30-40% | 1 | Email finding by domain |
| Findymail | 25-35% | 1 | Email verification |
| ZoomInfo | 50-60% | 3-5 | Premium, highest coverage |
| RocketReach | 35-45% | 2 | Email + phone combo |
| Clearbit | 25-35% | 1-2 | Company data + email |

## Waterfall Performance

| Providers in Waterfall | Expected Coverage | Credit Cost per Row |
|-----------------------|-------------------|-------------------|
| 1 provider | 35-45% | 1-2 |
| 2 providers | 60-70% | 2-3 |
| 3 providers | 75-80% | 3-5 |
| 4 providers | 85%+ | 4-7 |

## Phone Coverage

| Approach | Coverage | Cost |
|----------|----------|------|
| Single provider | 20-30% | 2-3 credits |
| 2-provider waterfall | 35-45% | 3-6 credits |
| 3-provider waterfall | 40-60% | 5-9 credits |

## Data Quality

| Metric | Benchmark |
|--------|-----------|
| Email verification pass rate | 85-92% of found emails |
| Annual email decay rate | 22-30% |
| Re-verification cadence | Every 30 days |
| Catch-all domain rate | 15-25% of B2B domains |
| Catch-all safety | Do not send to catch-all without validation |

## AI Column Performance

| Model | Speed | Quality | Credits | Use When |
|-------|-------|---------|---------|----------|
| GPT-4 Mini | Fast | Good | 1 | Extraction, classification, simple summarization |
| GPT-4 | Moderate | High | 3-5 | Complex reasoning, ICP fit judgment |
| Claude | Moderate | High | 3-5 | Nuanced analysis, competitive intelligence |

## Common Waterfall Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Coverage below 70% with 3+ providers | Provider inputs are wrong | Verify each provider gets the right input field |
| High credit spend, low coverage | Missing conditional checks | Add skip formula before each provider |
| Coverage drops on re-run | Auto-update re-running on filled rows | Add conditional to skip rows with existing data |
| Different results each run | Provider data is live, not cached | Store results to CRM/Supabase after each run |
