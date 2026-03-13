# clay-credit-optimization

Use this skill to manage Clay credit costs, select providers efficiently,
and forecast enrichment budgets.

## Required Inputs

- current monthly credit allocation or budget
- number of rows to process
- enrichment types needed (email, phone, firmographic, AI)
- frequency (one-time batch vs. recurring)

## Credit Cost Reference

### Enrichment Provider Credits (approximate per row)

| Provider | Credits | Best For |
|----------|---------|----------|
| Clearbit | 1-2 | Company data, technographics |
| Apollo | 1 | Email + company basics |
| Hunter | 1 | Email finding |
| Findymail | 1 | Email verification |
| ZoomInfo | 3-5 | Premium contacts + direct dials |
| Lusha | 2-3 | Phone numbers |
| RocketReach | 2 | Email + phone |
| GPT-4 Mini AI | 1 | Simple extraction, classification |
| GPT-4 AI | 3-5 | Complex reasoning |
| Claygent | 2-5 | Web research (varies by complexity) |
| Clayscript | 0 | Formulas, data manipulation |

### Budget Planning Formula

```
Monthly credits needed =
  (rows per month)
  × (providers per row, assuming waterfall)
  × (1 - skip_rate from conditionals)

Example:
  2,000 rows × 4 providers × 0.6 (40% skip from conditionals) = 4,800 credits
```

## Optimization Strategies

### 1. Conditional Enrichment (saves 30-50%)

Add a Clayscript formula before every paid column:
```
IF({target_field} != null && {target_field} != "", "skip", "run")
```
Set enrichment to run only when the formula returns "run".

### 2. Provider Ordering (saves 10-20%)

Order waterfall providers: cheapest first, most expensive last.
Each row should try the cheapest option and only cascade if the
previous provider returned no result.

### 3. Model Downgrade (saves 40-60% on AI)

Use GPT-4 Mini for:
- Extraction tasks (pull a name, title, or number)
- Classification (yes/no, category assignment)
- Simple summarization

Use GPT-4/Claude only for:
- ICP fit judgment
- Complex competitive analysis
- Multi-step reasoning

### 4. Batch Architecture (saves 20-30%)

Process in batches of 200-500 rows instead of row-by-row.
Review output quality between batches.
Stop early if quality drops.

### 5. Data Persistence (prevents re-spend)

Push all enriched data to CRM or Supabase after each batch.
Never re-enrich a row you already have data for.
Supabase cost: ~$30/month for 11.4M+ records.

## Execution Sequence

1. Inventory existing data to calculate skip rate from conditionals.
2. Map the enrichment workflow and count providers per row.
3. Apply the budget planning formula.
4. Identify the biggest cost driver and apply the relevant optimization.
5. Set up monitoring for credit consumption per batch.

## Output Contract

Return:

- projected monthly credit cost (before and after optimization)
- specific optimization actions ranked by savings
- recommended provider ordering for the waterfall
- monitoring setup for ongoing cost tracking
- break-even analysis if switching providers

## Anti-Patterns

- running enrichments without conditional checks
- using GPT-4 for tasks GPT-4 Mini handles
- not ordering waterfall by cost
- re-enriching data already stored in CRM
- processing 10,000+ rows without batch breaks
