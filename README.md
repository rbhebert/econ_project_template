# econ_project_template

Template for economics research projects in R and Stata.

Only code and project configuration are tracked. Data, output, drafts and documents keep their folder structure (via `.gitkeep`) but their contents are never uploaded.

```
code/             scripts (tracked): .qmd for R analyses, .do + .md for Stata
code/logs/        script logs (not tracked)
data/raw/         source data, read-only (not tracked)
data/cleaned/     analysis data (not tracked)
data/temp/        scratch (not tracked)
output/figures/   (not tracked)
output/tables/    (not tracked)
output/reports/   rendered Quarto reports (not tracked)
drafts/           paper and slides (not tracked)
docs/             budgets, RFPs, correspondence (not tracked)
AGENTS.md         instructions for AI coding agents (edit this)
_quarto.yml       Quarto settings; renders code/*.qmd to output/reports/
CLAUDE.md         one line, @AGENTS.md, so Claude Code reads AGENTS.md
.claude/skills/   agent skills: diagnostics, figures, tables, causal-inference, documentation
```

## Starting a new project

Click **Use this template** on GitHub, then clone the new repo. Add project-specific notes (data sources, sample, main specification) to the bottom of `AGENTS.md`.

## Adding a new folder

Create it with a `.gitkeep`. If it's a top-level folder, add it to the whitelist in `.gitignore`. Anything not whitelisted is ignored.
