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
| .agents/skills/SKILL.md | Orchestrator and routing logic |
| .agents/skills/clay-table-architect.md | Design a complete Clay table structure from an ICP and use case. |
| .agents/skills/clay-enrichment-waterfall.md | Build or audit multi-provider enrichment waterfalls for email, phone, and firmographic data. |
| .agents/skills/clay-ai-column-prompts.md | Write Clay AI column prompts that produce consistent, usable output at scale. |
| .agents/skills/clay-outbound-export.md | Prepare Clay tables for export to Apollo, Smartlead, Instantly, and HubSpot. |
| .agents/skills/clay-enrichment-debugger.md | Diagnose and resolve data match failures in Clay. |
| .agents/skills/clay-ai-research-agent.md | Build deep custom research agents within Clay prompts. |
| .agents/skills/clay-credit-optimization.md | Optimize enrichment waterfalls for maximum cost efficiency. |
| esources/formulas/clayscript-library.md | Library of reusable ClayScript and regex formulas for data cleanup. |
| esources/benchmarks/enrichment-benchmarks.md | Industry standards for match rates, data freshness, and coverage by vertical. |
| ECOSYSTEM.md | Cross-repo connectivity map |

## Prerequisites

- [ ] [Claude Code](https://claude.ai/code) installed and running
- [ ] Active Clay account (clay.com)
- [ ] At least one enrichment provider connected in Clay (Apollo, Hunter, Clearbit, or equivalent)
- [ ] A sequencing tool account if using the export skill (Apollo, Smartlead, or Instantly)

## Installation

### Cursor, Windsurf, or Generic AI IDE
1. Clone the repo: `git clone https://github.com/forma-norden/clay-claude-code-skill-pack`
2. Copy the `.agents/skills/` directory into your project's `.agents/skills/` folder.

### Claude Code
1. Clone the repo: `git clone https://github.com/forma-norden/clay-claude-code-skill-pack`
2. Copy the `.agents/skills/` directory into your project's `.claude/skills/` folder.

## Usage

Load a skill by telling Claude Code to read it before describing your task:

```
Read .agents/skills/clay-table-architect.md

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

GTM engineers, RevOps leads, VP Sales, and founders at B2B companies with 50 to
500 employees who are building or consolidating their outbound infrastructure and
want to reduce tool sprawl through better-engineered GTM systems.

---

## From the Forma Nôrden GTM Library

This is a free resource from the Forma Nôrden open-source GTM library, built by
[Yananai A. Chiwuta](https://yananaichiwuta.com/), GTM engineer and founder of
[Forma Nôrden](https://formanorden.com/).

- [Open-source GTM systems](https://github.com/forma-norden) - all repos in the library  
- [GTM engineering blog](https://formanorden.com/blog/) - strategy, systems, and outbound deep-dives  
- [All resources](https://formanorden.com/resources/) - guides, frameworks, and templates  

If this saves you time, star the repo and follow
[Forma Nôrden on LinkedIn](https://www.linkedin.com/company/formanorden/).

Built by [Forma Nôrden](https://formanorden.com/) - GTM engineering for B2B companies.


