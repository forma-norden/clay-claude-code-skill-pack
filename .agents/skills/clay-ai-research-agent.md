# clay-ai-research-agent

Use this skill to configure Claygent (Clay's AI research agent) for web research,
custom data extraction, and prospect qualification at scale.

## Required Inputs

- research objective (what information you need)
- target data source (LinkedIn, company website, review site, job board)
- output format (text, boolean, number, list)
- quality threshold (what counts as a valid result)

## Claygent Setup Principles

1. One research task per Claygent column. Do not combine multiple extractions.
2. Specify the exact data point, not a vague research question.
3. Include examples of good and bad output in the prompt.
4. Set a fallback value for when research fails.
5. Use GPT-4 Mini for factual extraction. Use GPT-4 for judgment calls.

## Production Prompt Templates

### Prospect Qualification

```
Visit {website_url}. Determine if this company sells to {target_market}.
Look at their homepage, about page, and pricing page.

Return: "Yes" if they clearly sell to {target_market}, "No" if they do not,
"Unclear" if you cannot determine.

Do not guess. If the website is down or blocked, return "Unavailable".
```

### Pain Point Identification

```
Visit {website_url} and read their product page.
Identify one specific problem their product solves for {persona}.

Return the problem in one sentence, 15 words or fewer.
If you cannot identify a clear problem, return "No clear pain identified".

Good output: "Reduces manual data entry for sales reps by auto-syncing CRM fields."
Bad output: "They help with sales stuff."
```

### Tech Stack Detection

```
Visit {website_url}. Check the homepage source code and any integration pages.
Identify if they use {tool_name}.

Return: "Uses {tool_name}" or "Does not use {tool_name}" or "Cannot determine".
Do not infer usage from general language. Look for logos, integration pages,
or source code references.
```

### Competitive Intelligence

```
Search for reviews of {company_name} on G2, Capterra, or Trustpilot.
Find the most negative review from the past 6 months.

Return: The core complaint in one sentence, plus the review source.
If no negative reviews exist in that timeframe, return "No recent negative reviews found".
```

### Personalization Data Extraction

```
Visit {linkedin_url}. Extract the following:
1. Most recent post topic (one phrase)
2. Current headline
3. Most recent role change (if within 12 months)

Return as three separate lines. If any field is not available, return "N/A" for that line.
```

## Model Selection Guide

| Task | Model | Credit Impact |
|------|-------|--------------|
| Factual extraction (yes/no, name, number) | GPT-4 Mini | Low |
| Classification and categorization | GPT-4 Mini | Low |
| Summarization from web content | GPT-4 Mini | Low |
| Judgment call (ICP fit assessment) | GPT-4 or Claude | Medium |
| Complex reasoning (competitive analysis) | GPT-4 or Claude | High |

## Execution Sequence

1. Define the exact data point needed and the source to check.
2. Select the prompt template closest to the task.
3. Customize the template with specific values and constraints.
4. Set the model based on task complexity.
5. Test on 10 rows and review output quality.
6. Add fallback value and quality gate before scaling.

## Output Contract

Return:

- configured Claygent prompt ready to paste
- recommended model
- expected credit cost per row
- quality gate criteria for valid vs. invalid results
- fallback value for failed lookups

## Anti-Patterns

- vague prompts without examples of good/bad output
- combining multiple research tasks in one column
- using GPT-4 for simple factual extraction
- no fallback value for unavailable data
- skipping the 10-row test before full-table execution
