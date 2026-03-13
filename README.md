# Clay Claude Code Skill Pack

Four Claude Code skill files for building production-grade Clay tables that generate
pipeline, not experiments. Built for VP Sales, founders, and GTM engineers who use
Clay to run outbound at scale and need their AI assistant to give precise, implementable
output instead of generic advice.

These skills encode the decision logic, enrichment sequences, prompt architecture,
and export structures that take a Clay table from idea to a running outbound campaign.

## What's Inside

| File | What it does |
|------|-------------|
| `.claude/skills/clay-table-architect.md` | Design a complete Clay table structure from an ICP and use case. Covers column ordering, naming, data types, ICP filter logic, enrichment waterfall order, conditional column dependencies, and cost estimates. Includes three production templates: outbound prospecting, ABM account enrichment, and champion tracking. |
| `.claude/skills/clay-enrichment-waterfall.md` | Build or audit multi-provider enrichment waterfalls for email, phone, and firmographic data. Covers provider quality and cost comparisons, waterfall configuration methods, conditional enrichment for cost gating, coverage benchmarks, and the most common waterfall mistakes. |
| `.claude/skills/clay-ai-column-prompts.md` | Write Clay AI column prompts that produce consistent, usable output at scale. Includes six production prompt templates (pain identification, ICP scoring, subject lines, email openers, LinkedIn notes, account briefs), a quality checklist, debugging guide, and model selection by use case. |
| `.claude/skills/clay-outbound-export.md` | Prepare Clay tables for export to Apollo, Smartlead, Instantly, and HubSpot. Covers pre-export data quality gates, name cleaning formulas, platform-specific field mappings, merge tag syntax, export column setup, sending volume guidelines, and post-export validation. |
| `tests/` | Prompt-based tests and validation checklists for all four skills. Run these to verify the skills produce correct output before using in production. |

## Prerequisites

- [ ] [Claude Code](https://claude.ai/code) installed and running
- [ ] Active Clay account (clay.com)
- [ ] At least one enrichment provider connected in Clay (Apollo, Hunter, Clearbit, or equivalent)
- [ ] A sequencing tool account if using the export skill (Apollo, Smartlead, or Instantly)

## Installation

1. Clone the repo:
   ```bash
   git clone https://github.com/forma-norden/clay-claude-code-skill-pack.git
   ```
2. Copy the `.claude/` folder into your project root:
   ```bash
   cp -r clay-claude-code-skill-pack/.claude your-project/.claude
   ```
3. Open your project in Claude Code. The skills are now available.

## Usage

Load a skill by telling Claude Code to read it before describing your task:

```
Read .claude/skills/clay-table-architect.md

I need a prospecting table for B2B SaaS companies, 50-300 employees,
US and UK. Persona is VP Sales. Exporting to Apollo. Budget is $150/month
for 2000 rows.
```

Claude will read the skill and produce a complete, production-ready table structure
with column names, data types, enrichment waterfall, ICP filter logic, and cost estimate.

## Running Tests

See `tests/README.md` for full instructions. Short version:

1. Load the skill
2. Run the test prompt from the relevant test file
3. Check the output against the validation checklist

## Who This Is For

GTM engineers, RevOps leads, VP Sales, and founders at B2B companies with 50 to 500 employees who are building or consolidating their
outbound infrastructure and want to reduce tool sprawl through
better-engineered GTM systems.

---

---

## From the Forma NÃ´rden GTM Library

This is a free resource from the Forma NÃ´rden open-source GTM library, built by 
[Yananai A. Chiwuta](https://yananaichiwuta.com/), GTM engineer and founder of 
[Forma NÃ´rden](https://formanorden.com/).

- [Open-source GTM systems](https://github.com/forma-norden): all repos in the library  
- [GTM engineering blog](https://formanorden.com/blog/): strategy, systems, and outbound deep-dives  
- [All resources](https://formanorden.com/resources/): guides, frameworks, and templates  

If this saves you time, star the repo and follow 
[Forma NÃ´rden on LinkedIn](https://www.linkedin.com/company/formanorden/).

Built by [Forma NÃ´rden](https://formanorden.com/): GTM engineering for B2B companies.
