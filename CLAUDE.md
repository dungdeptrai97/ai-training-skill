## Skill Creator Ultra
When user asks to "create skill", "turn workflow into skill", or "automate this":
- Read `.claude/commands/skill-creator-ultra/SKILL.md`
- Follow the 8 Phase pipeline
- Reference resources/ for templates and best practices

## Financial Services Plugins (Anthropic)

### Core: Financial Analysis
When user needs financial modeling, valuation, or analysis:
- Read skills in `financial-analysis/skills/` for domain knowledge
- Available skills: comps-analysis, dcf-model, lbo-model, 3-statements, check-deck, competitive-analysis, ppt-template-creator, check-model
- Commands: `/fa-comps`, `/fa-dcf`, `/fa-lbo`, `/fa-3-statements`, `/fa-check-deck`, `/fa-competitive-analysis`, `/fa-debug-model`, `/fa-ppt-template`

### Investment Banking
When user needs IB workflows (CIM, teasers, M&A, deal tracking):
- Read skills in `investment-banking/skills/`
- Available skills: cim-builder, teaser, buyer-list, deal-tracker, merger-model, pitch-deck, process-letter, strip-profile, datapack-builder
- Commands: `/ib-cim`, `/ib-teaser`, `/ib-buyer-list`, `/ib-deal-tracker`, `/ib-merger-model`, `/ib-one-pager`, `/ib-process-letter`

### Equity Research
When user needs equity research (earnings, coverage, thesis):
- Read skills in `equity-research/skills/`
- Available skills: earnings-analysis, earnings-preview, initiating-coverage, thesis-tracker, catalyst-calendar, morning-note, idea-generation, model-update, sector-overview
- Commands: `/er-earnings`, `/er-earnings-preview`, `/er-initiate`, `/er-thesis`, `/er-catalysts`, `/er-morning-note`, `/er-screen`, `/er-sector`, `/er-model-update`

### Private Equity
When user needs PE workflows (deal sourcing, DD, IC memo, portfolio):
- Read skills in `private-equity/skills/`
- Available skills: deal-sourcing, deal-screening, dd-checklist, dd-meeting-prep, ic-memo, portfolio-monitoring, returns-analysis, unit-economics, value-creation-plan
- Commands: `/pe-source`, `/pe-screen-deal`, `/pe-dd-checklist`, `/pe-dd-prep`, `/pe-ic-memo`, `/pe-portfolio`, `/pe-returns`, `/pe-unit-economics`, `/pe-value-creation`

### Wealth Management
When user needs wealth management workflows:
- Read skills in `wealth-management/skills/`
- Available skills: client-review, financial-plan, portfolio-rebalance, tax-loss-harvesting, client-report, investment-proposal
- Commands: `/wm-client-review`, `/wm-financial-plan`, `/wm-rebalance`, `/wm-tlh`, `/wm-client-report`, `/wm-proposal`

### Partner: LSEG
Fixed income, FX, options, macro analysis with LSEG data:
- Read skills in `partner-built/lseg/skills/`
- Commands: `/lseg-analyze-bond-basis`, `/lseg-analyze-bond-rv`, `/lseg-analyze-fx-carry`, `/lseg-analyze-option-vol`, `/lseg-analyze-swap-curve`, `/lseg-macro-rates`, `/lseg-research-equity`, `/lseg-review-fi-portfolio`

### Partner: S&P Global
Company tearsheets, earnings previews, funding digests:
- Read skills in `partner-built/spglobal/skills/`

### MCP Data Connectors (requires subscription/API keys)
- Daloopa, Morningstar, S&P Global, FactSet, Moody's, MT Newswires, Aiera, LSEG, PitchBook, Chronograph, Egnyte
- Config in `financial-analysis/.mcp.json`
