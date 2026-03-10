# Testing the Clay Claude Code Skill Pack

Each test file contains:
1. A test prompt to run in Claude Code with the skill active
2. A validation checklist — what the output must contain to pass
3. Common failure modes and what they indicate

## How to Run a Test

1. Open Claude Code in a project where this repo's `.claude/skills/` folder is accessible
2. Load the skill: "Read .claude/skills/[skill-name].md"
3. Run the test prompt from the relevant test file
4. Check the output against the validation checklist

## Test Files

| File | Skill it tests |
|------|---------------|
| `test-clay-table-architect.md` | clay-table-architect |
| `test-clay-enrichment-waterfall.md` | clay-enrichment-waterfall |
| `test-clay-ai-column-prompts.md` | clay-ai-column-prompts |
| `test-clay-outbound-export.md` | clay-outbound-export |

## Pass Criteria

A skill passes if:
- All checklist items are present in the output
- Output is immediately usable (no editing required to implement)
- No hallucinated providers, tools, or features that do not exist in Clay
- Output matches the format specified in the skill
