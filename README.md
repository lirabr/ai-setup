# SetUp

Fresh Apple-silicon Mac setup guides for Trading Desk v3, focused on efficient AI-assisted development, lower token usage, and flexible model/provider selection.

## Start here

1. Read the [complete AI development setup guide](ai-development-setup.md) for the tool catalog, official installation links, skills, MCP connections, and compatibility notes.
2. Follow its [staged setup sequence](ai-development-setup.md#11-setup-order-from-an-empty-laptop).
3. Use the [readiness checklist](ai-development-setup.md#14-fresh-laptop-readiness-checklist) before starting implementation.

The guide separates baseline tools from trials, optional additions, and alternatives. It is not an install-everything script.

## Quick navigation

| Topic | Guide section |
|---|---|
| Mac prerequisites and development utilities | [Fresh Mac foundations](ai-development-setup.md#3-fresh-mac-foundations) |
| OpenCode and alternative harnesses | [Coding harnesses and orchestration](ai-development-setup.md#4-coding-harnesses-and-orchestration) |
| RTK, DCP, caching, and context management | [Token and context controls](ai-development-setup.md#5-token-and-context-controls) |
| Model switching and providers | [Model access, routing, and local inference](ai-development-setup.md#6-model-access-routing-and-local-inference) |
| Downloadable and custom skills | [Skills](ai-development-setup.md#7-skills-sources-installation-and-custom-requirements) |
| MCP and CLI connections | [Integration catalog](ai-development-setup.md#8-mcp-and-cli-integration-catalog) |
| Testing and reproducibility tools | [Verification and supporting tools](ai-development-setup.md#9-verification-reproducibility-and-supporting-tools) |
| Measuring real savings | [Token usage, cost, and efficiency](ai-development-setup.md#12-measuring-token-usage-cost-and-efficiency) |
| Research and builder feedback | [Evidence](ai-development-setup.md#13-what-research-and-builder-feedback-support) |

## Project-specific references

The existing project guides remain in their original locations to preserve their links and avoid duplicate instructions:

- [Phase 0: reproducible laptop and project environment](../phase-0-laptop-setup.md)
- [Project tools and resources](../tools-and-resources.md)
- [Testing and acceptance](../testing-and-acceptance.md)
- [Operations runbook](../operations-runbook.md)

Run application verification commands from `trading-desk`, not from this folder. The project can be tested offline without an LLM account or broker credentials; coding-agent provider access is a separate tooling choice.

## Scope

This folder contains documentation, not installed tools, executable installers, active MCP configuration, or authored custom skills. The source research in Obsidian is preserved; this project-local edition is not automatically synchronized with it. Obsidian-only links are omitted so this folder's guide remains portable.
