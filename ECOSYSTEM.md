# Ecosystem: clay-claude-code-skill-pack

How this repo connects to the rest of the Forma Nôrden GTM library.

## Works With

| Repo | Relationship | When to use together |
|------|-------------|---------------------|
| `signal-based-list-building-workflow` | Upstream | Signal-qualified lists feed into Clay tables for enrichment |
| `cold-email-copy-playbook` | Downstream | Enriched data from Clay powers personalized email copy |
| `cold-email-deliverability-playbook` | Downstream | Email verification in Clay connects to deliverability checks |
| `n8n-gtm-workflow-pack` | Parallel | n8n orchestrates Clay enrichment via webhooks and HTTP requests |
| `buying-window-signal-workflow` | Upstream | Signal scoring determines which leads enter Clay for enrichment |
| `linkedin-claude-code-workflow` | Downstream | Enriched company data feeds LinkedIn ad targeting and outbound |
| `outbound-personalization-playbook` | Downstream (planned) | Clay AI columns generate personalization data for outreach |
| `list-building-complete-playbook` | Upstream (planned) | Complete list building workflows feed into Clay for enrichment |

## Suggested Skill Chains

1. **Signal to enriched outbound list**: `buying-window-signal-workflow` (score) > `clay-claude-code-skill-pack/clay-table-architect` (design table) > `clay-enrichment-waterfall` (enrich) > `clay-outbound-export` (push to sequencer)

2. **AI-powered personalization pipeline**: `clay-ai-research-agent` (gather data) > `clay-ai-column-prompts` (analyze and score) > `cold-email-copy-playbook/copy-atl-executive-messaging` (write personalized email)

3. **Credit-optimized batch processing**: `clay-credit-optimization` (plan budget) > `clay-table-architect` (design with conditionals) > `clay-enrichment-waterfall` (run waterfall) > `clay-enrichment-debugger` (troubleshoot if needed)
