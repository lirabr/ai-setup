# SetUp

A standalone setup guide for AI-assisted development on a fresh Apple-silicon Mac. Use it with any AI project: applications, agents, data pipelines, research, or developer tools.

This folder is self-contained. It has no dependency on a parent repository, a particular application, an Obsidian vault, or a fixed Python version. You can move it into its own repository without changing its links.

## Start here

1. Read the [complete AI development setup guide](ai-development-setup.md) for tools, official installation links, skills, model providers, and MCP connections.
2. Choose [iTerm2 or Ghostty](ai-development-setup.md#terminal-choice) as your terminal.
3. Follow the [staged setup sequence](ai-development-setup.md#11-setup-order-from-an-empty-laptop).
4. Use the [readiness checklist](ai-development-setup.md#14-fresh-laptop-readiness-checklist) before implementation.

The guide separates baseline tools from trials, optional additions, and alternatives. It is not an install-everything script.

## Quick navigation

| Topic | Guide section |
|---|---|
| Mac prerequisites, terminals, and utilities | [Fresh Mac foundations](ai-development-setup.md#3-fresh-mac-foundations) |
| OpenCode and alternative harnesses | [Coding harnesses and orchestration](ai-development-setup.md#4-coding-harnesses-and-orchestration) |
| RTK, DCP, caching, and context management | [Token and context controls](ai-development-setup.md#5-token-and-context-controls) |
| Model switching and providers | [Model access, routing, and local inference](ai-development-setup.md#6-model-access-routing-and-local-inference) |
| Downloadable and custom skills | [Skills](ai-development-setup.md#7-skills-sources-installation-and-custom-requirements) |
| MCP and CLI connections | [Integration catalog](ai-development-setup.md#8-mcp-and-cli-integration-catalog) |
| Testing and reproducibility | [Verification and supporting tools](ai-development-setup.md#9-verification-reproducibility-and-supporting-tools) |
| Measuring real savings | [Token usage, cost, and efficiency](ai-development-setup.md#12-measuring-token-usage-cost-and-efficiency) |
| Research and builder feedback | [Evidence](ai-development-setup.md#13-what-research-and-builder-feedback-support) |

## Applying this to a project

- Select runtimes from that project's manifests and supported versions; Python and Node.js are not both mandatory application dependencies.
- Run the project's own documented install, test, lint, typecheck, and build commands from its repository root.
- Keep tooling environments separate from application dependencies.
- Adapt the custom skill specifications to the project's contracts, data, and side effects.
- Configure provider accounts, budgets, permissions, and secrets separately; none are supplied by this folder.

## Scope

This folder contains documentation, not installed tools, executable installers, active MCP configuration, or authored custom skills. All local links stay within the folder; installation and research links point to external sources. Moving the folder does not install anything or connect any service.
