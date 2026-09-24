---
title: Trading Desk v3 - Fresh Mac AI Development Setup
created: 2026-09-24
updated: 2026-09-24
researched: 2026-09-24
status: setup-guide
target: Fresh Apple-silicon Mac
tags:
  - project/trading-desk
  - ai/tooling
  - ai/context-engineering
  - development/setup
  - research
---

# Trading Desk v3 — fresh Mac AI development setup

## 1. Scope and recommendation

This guide assumes a **fresh Apple-silicon Mac**: no development tools, model accounts, API credentials, coding-agent configuration, skills, MCP connections, Git checkout or memory service. Recommendations do not depend on what is installed on another laptop.

Goals:

1. Reduce unnecessary tokens and total cost per correctly completed task.
2. Improve implementation efficiency and verification quality.
3. Switch models/providers without rebuilding the development workflow.

**Recommended baseline:** OpenCode + two model choices + uv/Python 3.12 + pytest/Ruff + targeted retrieval/Python LSP + a small set of on-demand skills + one documentation route. Measure this baseline, then evaluate RTK independently.

OpenCode is already a **harness**: the application connecting a model to tools, permissions, context management and the edit/test loop. An additional orchestration framework is not required.

This is a complete catalog, **not an instruction to install everything**. Tools such as Pi, Aider, OpenRouter, DCP, Serena, memory servers and orchestration packages are alternatives or conditional additions. No installations, account connections, paid benchmarks or project-code changes were performed in preparing this guide.

### Adoption tiers

| Tier | Meaning |
|---|---|
| Baseline | Establish before normal implementation |
| Trial | Compare independently against the baseline; retain only if useful |
| Optional | Add when a particular capability is needed |
| Alternative | Choose instead of another option, or compare in isolation |
| Resource/custom | Documentation, a service, or content to author—not an assumed downloadable tool |

## 2. Installation policy and Apple-silicon prerequisites

- Use native macOS **arm64** tools. Homebrew's supported Apple-silicon prefix is `/opt/homebrew`.
- Start with a macOS version supported by the selected tools. Do not assume Rosetta is required; add it only for an identified dependency.
- Prefer reviewed stable releases published at least seven days earlier. Record exact versions, plugin revisions and image digests where practical.
- Official documentation sometimes recommends `@latest`, prereleases, global changes or remote shell one-liners. Treat those as upstream examples, not a reproducible install manifest. Review the selected release and installer before execution.
- Choose one installation owner per runtime/tool. Do not accumulate competing Homebrew, npm, uv, mise and standalone installations of the same executable.
- Keep Python tooling isolated from application dependencies. A tool requiring Python 3.13 does not change this project's Python 3.12 requirement.
- Preserve the project's lockfile and dependency-release cutoff. Do not relax security controls to make installation succeed.
- Authentication, subscriptions, API spending and permission grants are separate user decisions. Never place secrets in prompts, command history, shared notes, committed configuration or skill files.
- No broker credentials, order-placement MCP tools or production database-write access belong in this development setup.

## 3. Fresh Mac foundations

All links below lead to official installation guidance, project sources or package-maintainer documentation. The setup/check columns describe future actions, not completed installation evidence.

| Tool | Tier | Installation link | Setup route and verification |
|---|---|---|---|
| Apple Command Line Tools | Baseline | [Requirements](https://docs.brew.sh/Installation#macos-requirements), [Apple downloads](https://developer.apple.com/download/all/) | Use Apple's supported CLT installation flow; verify `xcode-select -p`. Full Xcode is not required solely for this Python workflow. |
| Homebrew | Baseline | [Install Homebrew](https://docs.brew.sh/Installation) | Native arm64 installation and documented shell setup; verify `brew --prefix` is `/opt/homebrew`. |
| Git | Baseline | [Homebrew package](https://formulae.brew.sh/formula/git) | Use Apple's supplied Git or the reviewed Homebrew package. Verify `git --version`; identity and authentication remain user-owned. |
| GitHub CLI (`gh`) | Baseline if using GitHub | [Install GitHub CLI](https://cli.github.com/) | Homebrew package `gh`. Verify version; perform authorized login separately and then check authentication status. |
| uv | Baseline | [Install uv](https://docs.astral.sh/uv/getting-started/installation/) | Project environments and isolated Python tools; verify `uv --version`. Do not modify macOS system Python. |
| Python 3.12 | Baseline | [Install/manage Python with uv](https://docs.astral.sh/uv/guides/install-python/) | Choose and record a reviewed 3.12 interpreter patch release; verify the project actually uses it. |
| Node.js LTS + npm | Baseline for selected JS-based tools | [Official downloads](https://nodejs.org/en/download) | Use one arm64 LTS installation route. Verify `node --version` and `npm --version`; npm is included with Node.js. |
| ripgrep (`rg`) | Baseline | [Homebrew package](https://formulae.brew.sh/formula/ripgrep) | Fast scoped content search; verify version and a bounded search in a test fixture. |
| fd | Baseline utility | [Official installation instructions](https://github.com/sharkdp/fd#installation) | Fast filename discovery; Homebrew package `fd`. Verify version and scoped lookup. |
| jq | Baseline | [Download/install](https://jqlang.org/download/) | Filter JSON before it enters model context; verify `jq --version` and a small JSON transformation. |
| VS Code | Optional visual editor | [macOS installation](https://code.visualstudio.com/docs/setup/mac) | Select arm64 or Universal build; use it for code/diff review and the project's Python environment. No additional AI subscription is required for ordinary editing. |
| Obsidian | Optional persistent notes | [Mac download](https://obsidian.md/download?os=mac) | Create or deliberately select a vault. Use project-scoped notes with tags and links; no broad agent access to all notes is assumed. |

macOS Terminal is sufficient. A different terminal application can improve personal ergonomics but is not a token-saving prerequisite.

## 4. Coding harnesses and orchestration

| Tool | Tier | Installation/setup link | Role and adoption check |
|---|---|---|---|
| **OpenCode** | **Baseline primary harness** | [Download/install](https://opencode.ai/download), [model setup](https://opencode.ai/docs/models/) | Multiple providers, model switching, skills, MCP and LSP. Verify selected version, a bounded read-only task, permissions and a disposable edit/test task. |
| **Pi** | Alternative | [Quickstart/install](https://pi.dev/docs/latest/quickstart) | Minimal extensible harness. Current npm package is `@earendil-works/pi-coding-agent`; its official npm instructions use `--ignore-scripts`. Compare the same model/task against OpenCode. |
| **Aider** | Alternative | [Installation](https://aider.chat/docs/install.html), [repository maps](https://aider.chat/docs/repomap.html) | Good for explicit file selection and bounded edits. Install `aider-chat` in an isolated uv tool environment. Review auto-commit behavior; architect/editor mode makes additional model requests. |
| **Devin CLI** | Alternative | [Official quickstart](https://docs.devin.ai/cli) | Local coding workflow with model choices; account/plan requirements must be checked. Configure only if selected, not because another machine has it. |
| **Superpowers** | Selected skills first; full plugin optional | [OpenCode installation guide](https://github.com/obra/superpowers/blob/main/docs/README.opencode.md) | Full plugin adds a skill collection and bootstrap context. Evaluate its workflow overhead; importing selected skills is a different setup. |
| **GSD Core** | Optional phase management | [Current installation guide](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/install-on-your-runtime.md) | Current project linked from the old GSD repository; package `@opengsd/gsd-core`. Use the selected runtime's transformations, not raw Claude-format agent files. |
| **Oh My OpenCode / Oh My OpenAgent** | Optional heavy orchestration | [Current installation guide](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/guide/installation.md) | Upstream now uses Oh My OpenAgent. Inspect bundled agents, hooks, MCPs and provider settings. Not a default token-reduction layer. |
| **Bun** | Optional prerequisite | [Installation](https://bun.com/docs/installation) | Add only if the chosen supported runtime/installer requires it; verify version. Not a mandatory second package manager. |
| **Autoresearch / Ralph-style loops** | Optional bounded experiment | [Shopify builder account](https://shopify.engineering/autoresearch), [Pi autoresearch project](https://github.com/davebcn87/pi-autoresearch) | Require a measurable objective, iteration/spend limits, isolated state and correctness gates. Do not use an unlimited autonomous loop as baseline setup. |

### Harness decision

Choose **one primary harness**. Pi is a promising lean comparison; Aider is useful for a different, explicitly scoped editing workflow. Do not install all of them to solve model switching: OpenCode already supports multiple providers.

Pi's official design does not include built-in MCP, plan mode or permission popups. Extensions/external controls are needed for equivalent capabilities. Minimal overhead comes with integration work; it is not a guarantee of lower total engineering effort.

If parallel work becomes useful, use separate worktrees and explicit file ownership. Multiple agents should not write to the same checkout simultaneously. Parallelism can improve elapsed time while increasing aggregate tokens.

## 5. Token and context controls

| Tool/practice | Tier | Installation/setup resource | Use and verification |
|---|---|---|---|
| Built-in OpenCode compaction/pruning | Baseline | [OpenCode docs](https://opencode.ai/docs/), [v2 compaction](https://opencode.ai/v2/docs/compaction) | Use the selected major's documented behavior before third-party compression. Compaction is lossy; preserve exact evidence needed for the active task. |
| Prompt caching | Baseline practice | [Caching guidance](https://openrouter.ai/docs/guides/best-practices/prompt-caching) | Stable prompt/tool prefixes can lower billed input cost. Verify provider-reported cache usage; this is not a separate installer. |
| RTK | Trial | [Install package](https://formulae.brew.sh/formula/rtk), [integration and analytics](https://github.com/rtk-ai/rtk/) | Filter supported verbose CLI output. Preview integration changes; verify one rewrite per command, useful failures, preserved exit codes and raw-output recovery. |
| Dynamic Context Pruning (DCP) | Later trial | [Official installation/README](https://github.com/Opencode-DCP/opencode-dynamic-context-pruning/blob/master/README.md) | Consider for long sessions after testing native behavior. Pin a compatible version; compare billing, cache reads and correctness. |
| Repomix | Occasional handoff/review | [Installation](https://repomix.com/guide/installation), [token-budget options](https://repomix.com/guide/command-line-options) | Pack an allowlisted subset with a token budget. Keep secret checks on and inspect before sharing. Current install docs require Node.js 22+. |
| QMD | Optional local docs search | [Official install/setup](https://github.com/tobi/qmd) | Current package `@tobilu/qmd`; begin with a project collection and bounded CLI results. Local model downloads/indexing are additional setup actions. |
| Short project instructions | Baseline practice | [Agent Skills format](https://agentskills.io/specification) | Include verification commands, unusual constraints and links—not an encyclopedia of the repository. |
| Explicit session/model handoff | Baseline practice | Custom skill in section 7 | Transfer a short factual checkpoint instead of the entire conversation. |

### Practical defaults

- Search first, then read the relevant symbols/sections. Avoid repeated full-file and full-repository reads.
- Keep datasets, SQLite files, environments, caches, generated PDFs/ZIPs and large logs out of automatic context. Read them deliberately when relevant.
- Retrieval exclusions and `.gitignore` are not security boundaries.
- Keep unrelated domain instructions out of this project's startup context. Retain general safety and approval rules.
- Keep model/tool selection stable within a task where practical. Aggressive message rewriting and frequent provider changes can lose caching benefits.
- Prefer deterministic tests and lint feedback to repeated speculative model reviews.
- Bound retries, subagent fan-out, response sizes and research scope.
- Do not force all outputs into cryptic abbreviations or remove diagnostic evidence merely to report fewer tokens.

### Version compatibility

Do not copy another laptop's version or config. Choose a vetted stable OpenCode release, then use its matching documentation:

- [OpenCode v1 docs](https://opencode.ai/docs/)
- [OpenCode v2 docs](https://opencode.ai/v2/docs/)

Permission names, MCP fields, plugin keys and compaction options differ. Superpowers' current instructions distinguish v1 `plugin` and v2 `plugins`; its v2 integration requires supported host/plugin versions. These examples identify a compatibility issue, not a universal configuration template.

RTK installers can configure additional assistants or global hooks, depending on release/flags. Preview the chosen installer and use one integration—not overlapping native and third-party rewrite plugins. DCP installers likewise need to match the chosen host major.

## 6. Model access, routing and local inference

### Recommended model roles

| Role | Work | Selection |
|---|---|---|
| Fast | Narrow edits, fixtures, straightforward tests, documentation | Compare an economical mini/Flash/Haiku-class or open-model candidate |
| Build | Normal multi-file implementation | Best measured quality/cost result on representative project tasks |
| Review | Architecture, difficult debugging, accounting/risk invariants | Strong reasoning model; Build and Review can initially share one model |

Start with **two models**, not a large collection. GLM/Kimi/DeepSeek and current OpenAI/Anthropic/Google model families are evaluation candidates, not permanent recommendations independent of task and price.

| Option | Tier | Setup/resource link | Requirements and verification |
|---|---|---|---|
| Direct providers through OpenCode | Baseline option | [Provider setup](https://opencode.ai/docs/providers/) | Approved account, supported authentication and budget. Verify actual tool use, privacy terms and model availability. No extra gateway is required for switching. |
| OpenRouter | Optional convenience | [Quickstart](https://openrouter.ai/docs/quickstart), [provider routing](https://openrouter.ai/docs/guides/routing/provider-selection.md) | One account for many models. Restrict allowed providers and fallbacks; verify data policies, serving endpoint and actual charges. |
| Models.dev | Resource | [Catalog/API](https://github.com/anomalyco/models.dev) | No installation. Check IDs, tool support, limits, releases and prices against the provider's current catalog. |
| LiteLLM | Optional later gateway | [Docker quickstart](https://docs.litellm.ai/docs/proxy/docker_quick_start), [budget controls](https://docs.litellm.ai/docs/proxy/users) | Separate service with reviewed pinned release/image and required storage. Validate enforcement rather than assuming an advertised budget is a hard cap. |
| Ollama | Optional local inference | [macOS installation](https://docs.ollama.com/macos) | Select an arm64-compatible model only after assessing RAM, context, throughput and tool-call quality. Keep endpoint local and approve large downloads separately. |

### Routing rules

- Choose exact model IDs and reasoning settings and record them for comparisons. Floating aliases can change without an intentional evaluation.
- Switch primarily at task boundaries or after a concise handoff. A new model/provider can have different limits, tool semantics and cache behavior.
- An OpenAI-compatible API does not guarantee identical streaming, structured outputs, reasoning controls or tool calls.
- OpenCode v1's `small_model` setting is for lightweight auxiliary work such as titles; it is not automatic routing of every simple coding task.
- Direct API billing and consumer subscriptions are different products. Use officially supported authentication; do not base the setup on unofficial subscription-token reuse.
- For OpenRouter, review provider allowlists, data-collection restrictions, retention options and fallbacks. Sticky caching can help, but a fallback can change the endpoint and cache.
- Local inference avoids per-request API billing but is not zero-cost, automatically fast, or necessarily capable enough for the task.
- Monthly budget and hardware-memory decisions remain open until actual setup; they do not require installing a gateway now.

LiteLLM requires normal supply-chain scrutiny. Its [March 2026 advisory](https://docs.litellm.ai/blog/security-update-march-2026) identifies compromised PyPI versions 1.82.7 and 1.82.8; exclude those releases. This does not imply every later release is compromised. Its budget documentation also warns that database-backed caps do not enforce limits in a DB-less deployment.

## 7. Skills: sources, installation and custom requirements

Skills are reusable instructions/workflows, not additional models. The [Agent Skills specification](https://agentskills.io/specification) loads metadata initially, instructions on invocation and supporting resources as needed. Catalog metadata is not free, so installing hundreds of unused skills adds overhead.

### Installable/adaptable skills

| Skill | Official source/setup link | Trigger and expected result |
|---|---|---|
| `test-driven-development` | [Superpowers source](https://github.com/obra/superpowers/tree/main/skills/test-driven-development) | Behavioral change: failing regression, minimal fix, passing verification and negative cases. Do not inherit automatic commits or destructive deletion/reset behavior. |
| `systematic-debugging` | [Superpowers source](https://github.com/obra/superpowers/tree/main/skills/systematic-debugging) | Failure: reproduce, trace data flow, test a hypothesis and verify the root-cause fix. |
| `verification-before-completion` | [Superpowers source](https://github.com/obra/superpowers/tree/main/skills/verification-before-completion) | Completion claim: fresh commands/results and explicit limits, not an untested assertion. |
| `differential-review` | [Trail of Bits source](https://github.com/trailofbits/skills/tree/main/plugins/differential-review) | Sensitive diff: evidence-backed security findings, changed trust boundaries, impact and missing regressions. Avoid a whole-repo audit on every turn. |
| `property-based-testing` | [Trail of Bits source](https://github.com/trailofbits/skills/tree/main/plugins/property-based-testing) | Suitable invariants: identify properties and counterexamples; Hypothesis is a separately approved dependency. |
| Documentation lookup | [Context7 CLI/skill setup](https://context7.com/docs/clients/cli) | Version-specific library question: retrieve a bounded relevant answer with source/version. |
| Browser workflow | [Playwright CLI and skills](https://github.com/microsoft/playwright-cli/blob/main/README.md) | Local browser-facing behavior: targeted assertions and bounded snapshots; add only when needed. |

Installation resources:

- [Superpowers OpenCode integration](https://github.com/obra/superpowers/blob/main/docs/README.opencode.md): complete plugin installation, with version-specific instructions.
- [Trail of Bits skills repository](https://github.com/trailofbits/skills): source collection and packaging instructions; audit runtime-specific tool assumptions.
- [Skills.sh discovery and CLI](https://www.skills.sh/docs): discover/install skills; popularity is not a security approval.
- [Agent Skills specification](https://agentskills.io/specification): portable folder/frontmatter/reference structure.

The full Superpowers plugin registers a collection and injects bootstrap instructions. Selecting only a few reviewed workflows is a separate manual or supported adaptation—not an assumed selective-install switch. Do not install the entire collection and describe it as a minimal three-skill setup.

### Custom project skills to author

These do **not** have public installation packages. Author them using the [Agent Skills format](https://agentskills.io/specification), project contracts and documented checks.

| Skill | Trigger | Required behavior/output |
|---|---|---|
| `project-verify` | Before completion or review | Select relevant offline tests and Ruff; report commands, exit results, expected positive/negative behavior and unverified scope. |
| `time-series-leakage-review` | Features, training or backtests change | Check point-in-time availability, future leakage, label maturity, fold separation, holdout use and costs. |
| `ledger-risk-invariants` | Ledger, risk, approvals or simulator changes | Check idempotency, reservations, expiry, HALT, stale marks, UNKNOWN states and forbidden economic effects. |
| `session-handoff` | Task/model/session switch | Concise goal, constraints, decisions, changed files, verified tests, unresolved issues and next action. Exclude secrets and full logs. |

### Skill storage and acceptance

1. Keep newly authored shared/Devin project content in `.devin/skills/<name>/SKILL.md`; global Devin configuration belongs under `~/.config/devin/`.
2. OpenCode does not automatically discover `.devin/skills`. Use the selected release's documented explicit skill/config path support and verify discovery. Do not assume cross-harness configuration is portable merely because Markdown is portable.
3. Do not create redundant `.claude/` or `.cursor/` compatibility configurations. Inspect what third-party installers write and choose scope deliberately.
4. Review scripts, network behavior, allowed tools, source licenses and revisions. Keep detailed reference material separate from a small skill body.
5. Verify that a relevant task loads the skill, an unrelated task does not, and the skill cannot silently authorize commits, pushes, destructive operations, subagents or paid calls.
6. Skills may be model-independent at the content level while tool names, permissions, hooks and model overrides remain harness-specific.

## 8. MCP and CLI integration catalog

[MCP](https://modelcontextprotocol.io/docs/getting-started/intro) standardizes connections to tools and data. It is not inherently a token-saving mechanism. Tool schemas, discovery and returned content have costs that depend on the client.

| Integration | Tier | Official setup link | Scope and acceptance |
|---|---|---|---|
| Context7 CLI + skill | Recommended first docs route | [CLI install/setup](https://context7.com/docs/clients/cli) | Package `ctx7`, Node.js prerequisite. Select CLI/skill mode, inspect generated rules/paths, resolve the library then query its appropriate version. |
| Context7 MCP | Alternative docs route | [Client setup](https://context7.com/docs/resources/all-clients) | Approved remote connection/OAuth or key flow. Verify discovery and one docs request; do not enable both routes by default. |
| GitHub MCP | Optional, CLI first | [Official remote server](https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md) | Repository-scoped authorization and required read-only toolsets. Use `gh` when it already covers the task. |
| Atlassian Rovo MCP | Optional | [Official setup](https://support.atlassian.com/atlassian-rovo-mcp-server/docs/getting-started-with-the-atlassian-remote-mcp-server/) | Requires authorized account/org access. Match client/server versions; enable for Jira/Confluence work, not every implementation session. |
| Serena | Optional semantic trial | [Installation](https://oraios.github.io/serena/02-usage/010_installation.html) | Current docs install `serena-agent` as an isolated Python 3.13 tool. Start with its language-server backend; validate Python symbols/references before semantic edits. |
| Playwright CLI + skills | Optional for browser work | [Installation/skills](https://github.com/microsoft/playwright-cli/blob/main/README.md) | Current package `@playwright/cli`; satisfy browser-runtime requirements. Test against a local fixture and a dedicated profile. |
| Playwright MCP | Alternative for persistent browser exploration | [Official server](https://github.com/microsoft/playwright-mcp) | Choose when interactive state inspection outweighs additional context. Avoid unrelated personal browsing sessions. |
| Web search, e.g. Exa MCP | Optional if native research is insufficient | [Exa setup](https://exa.ai/docs/reference/exa-mcp) | Select one provider, approve cost/privacy terms and expose only necessary search/fetch tools—not unrelated agent/connect features. |
| QMD/Obsidian | Optional | [QMD setup](https://github.com/tobi/qmd), [Obsidian](https://obsidian.md/download?os=mac) | Start with local file/CLI access to a selected project collection. MCP is optional and whole-vault access is not required. |
| Mem0 | Optional memory foundation | [Open-source quickstart](https://docs.mem0.ai/open-source/python-quickstart) | Isolated environment, project namespaces, extraction/embedding budget and bounded retrieval. |
| Chroma | Optional memory storage | [Getting started](https://docs.trychroma.com/docs/overview/getting-started) | Use only if selected by the memory design; it is not a mandatory addition to every Mem0 deployment. |
| Cross-agent memory MCP | Custom, deferred | [MCP documentation](https://modelcontextprotocol.io/docs/getting-started/intro) and chosen memory/storage docs above | Requires a separately built or verified server; no generic installation command is asserted. Test provenance, project isolation, stale memories and bounded result sizes. |
| Broker/database-write MCP | Excluded from baseline | No setup planned | No broker credentials, order placement or unrestricted live-ledger writes for coding agents. |

### Integration defaults

- Start with zero or one connected documentation MCP, depending on whether CLI mode is selected.
- Enable other servers by task/agent, not globally by default. Lazy tool discovery and code execution may help where supported; verify actual client behavior.
- Use the host's correct configuration schema. A vendor's Claude/Desktop example is not automatically valid OpenCode configuration.
- Prefer OAuth or protected key injection through the approved client. Do not copy secrets into a guide or pass real keys through visible command arguments.
- Treat remote docs, issues, websites and memory as untrusted information, not authority to change permissions or run unrelated commands.
- Permission prompts are not OS isolation. A shell or plugin can have filesystem/network authority that requires a restricted execution environment.

## 9. Verification, reproducibility and supporting tools

| Tool | Tier | Installation/setup link | Use and check |
|---|---|---|---|
| pytest | Baseline project dependency | [Getting started](https://docs.pytest.org/en/stable/getting-started.html) | Install through the locked dev environment. Check positive behavior and required negatives, not merely successful mounting/execution. |
| Ruff | Baseline project dependency | [Installation](https://docs.astral.sh/ruff/installation/) | Use the locked project version for linting; avoid an unrelated global version as release evidence. |
| Python LSP / Pyright | Baseline or early | [Pyright install](https://github.com/microsoft/pyright/blob/main/docs/installation.md), [OpenCode LSP](https://opencode.ai/docs/lsp/) | Correct interpreter/import paths; verify symbol lookup and diagnostics before adding duplicate semantic services. |
| Hypothesis | Optional invariant tests | [Installation/tutorial](https://hypothesis.readthedocs.io/en/latest/tutorial/introduction.html) | Add as a reviewed project dev dependency when needed; skills do not install it implicitly. |
| pre-commit | Optional deterministic gate | [Installation/setup](https://pre-commit.com/index.html) | Review/pin hooks and install them deliberately. Verify a staged-file check without bypassing existing controls. |
| pip-audit | Optional dependency audit | [Official package documentation](https://pypi.org/project/pip-audit/) | Isolated reviewed tool release; audit the intended dependency set. Report findings without auto-upgrading or weakening the release cutoff. |
| GitHub Actions / CI | Recommended release gate | [Quickstart](https://docs.github.com/en/actions/get-started/quickstart) | A hosted service, not a laptop package. Mirror locked offline tests/lint; creating or enabling workflows is a separate approved change. |
| just | Optional task aliases | [Installation](https://just.systems/man/en/installation.html) | Thin aliases around documented commands. Do not maintain a second implementation of build/test logic. |
| mise | Optional tool-version manager | [Getting started](https://mise.jdx.dev/getting-started.html) | An alternative runtime-management route; avoid competing Node/Python owners. Review executable project configuration before trust. |
| direnv | Optional environment loading | [Installation](https://direnv.net/docs/installation.html) | Shell hook and `.envrc` approval are explicit. Avoid injecting secrets into every agent process. |
| Docker Desktop | Optional; important for selected unattended work | [Apple-silicon install](https://docs.docker.com/desktop/setup/install/mac-install) | Review license/resource needs, use arm64 images and restrict mounts/credentials. Installing Docker alone does not sandbox an agent running on the host. |
| OpenCode usage reporting | Baseline built-in capability | [CLI docs](https://opencode.ai/docs/cli/) | Use the selected version's reporting command and compare with provider billing. No monitoring server is necessary initially. |
| RTK gain | Trial telemetry | [Analytics](https://github.com/rtk-ai/rtk/) | Observe compressed command output; never substitute this metric for task billing. |

## 10. Later application tooling—not coding prerequisites

| Tool | Installation/setup link | Adoption condition |
|---|---|---|
| Promptfoo | [Getting started](https://www.promptfoo.dev/docs/getting-started/) | Actual application prompt/provider regression testing. Paid eval calls need an approved budget. |
| Langfuse | [Docker Compose setup](https://langfuse.com/self-hosting/deployment/docker-compose), [cost tracking](https://langfuse.com/docs/observability/features/token-and-cost-tracking) | Instrument application LLM traces and usage; redact sensitive data. It does not automatically observe every coding-agent call. Choose hosted or local intentionally. |
| LiteLLM | [Gateway setup](https://docs.litellm.ai/docs/proxy/docker_quick_start) | Centralized routing/accounting justifies another service; see section 6 for storage/security caveats. |
| LangGraph | [Installation](https://docs.langchain.com/oss/python/langgraph/install) | Use the project's graph extra/lockfile for the relevant application workflow, not as an additional coding harness. |
| MLflow | [Tracking quickstart](https://mlflow.org/docs/latest/ml/getting-started/quickstart/) | Experiment UI/tracking is needed; use the project's tracking extra and preserve artifact/evidence requirements. |

Application libraries such as pandas, NumPy, scikit-learn, PyArrow and yfinance come from the project lockfile, not separate global installs. Verify SQLite features through the selected Python distribution. PostgreSQL, Redis, vector databases, cloud accounts, market-data subscriptions and broker SDKs are not universal prerequisites for starting development.

## 11. Setup order from an empty laptop

### Stage 0 — Select versions, accounts and boundaries

1. Choose OpenCode as the primary harness and a vetted stable major release. Select compatible plugins only after that choice.
2. Record installation sources and exact reviewed versions. Do not silently inherit another machine's pinned or floating versions.
3. Choose direct provider APIs or OpenRouter, establish privacy requirements and an owner-approved budget. Do not buy multiple subscriptions before testing the workflow.
4. Define accessible project paths and execution boundaries. Keep production/broker credentials absent.

**Exit check:** the install inventory is explicit; optional tools remain optional; authentication and spending decisions are understood.

### Stage 1 — Bootstrap native macOS tooling

1. Install Apple CLT and Homebrew.
2. Install uv and a reviewed Python 3.12 interpreter, one Node LTS route, Git and selected retrieval utilities.
3. Install GitHub CLI if using GitHub; add VS Code/Obsidian if desired.
4. Verify arm64 tooling, Homebrew prefix and shell PATH. Do not add Rosetta unless an actual dependency requires it.

**Exit check:** the commands resolve from their intended installation owner; versions are recorded.

### Stage 2 — Obtain the project and verify it offline

1. Obtain the approved repository/archive. Establish the intended Git boundary before agent edits; do not initialize a nested repository accidentally.
2. Use the project's lockfile and release cutoff. Keep standalone coding tools outside its environment.
3. From `trading-desk`, use the documented verification flow:

```bash
uv sync --locked --python 3.12 --extra dev --extra graph
uv run --no-sync pytest -q
uv run --no-sync ruff check src tests
uv run --no-sync desk-research --help
```

These commands are future checks, not results from this research. Synchronization can download dependencies; the verification suite should use offline/synthetic fixtures and no broker keys.

**Exit check:** the intended interpreter/dependency set is active, tests/lint pass, and any remaining acceptance gaps are recorded rather than inferred green.

### Stage 3 — Establish plain OpenCode

1. Install the selected stable version and authenticate approved providers through supported flows.
2. Configure Fast and Build/Review choices explicitly; capture exact model/reasoning settings.
3. Keep native context management first. Set bounded retries, output sizes and spending controls available in the chosen services.
4. Add concise project instructions: commands, unusual invariants, relevant document links and approval boundaries.
5. Exercise a bounded read-only request, a disposable edit/test task and a model switch with a short handoff.

**Exit check:** tool use and model switching work; no plugin is required to obtain a useful baseline.

### Stage 4 — Add relevant skills and documentation

1. Enable Python LSP with the correct interpreter.
2. Add the selected reviewed skills and author the required project-specific ones.
3. Choose Context7 CLI+skill or MCP; verify one version-aware lookup.
4. Confirm skills load on demand and unrelated MCP servers remain disconnected.

**Exit check:** the agent can retrieve relevant evidence without re-reading the whole project; startup context/tool overhead is recorded.

### Stage 5 — Compare efficiency additions independently

1. Measure plain OpenCode on representative tasks.
2. Add RTK alone; check integration, exit codes and raw diagnostic access.
3. Try DCP only if long-session context is a measured problem; assess cache effects and missing evidence.
4. Compare Pi or Aider separately with the same model/task conditions if desired.

**Exit check:** retain an add-on only when the chosen metric improves without degrading correctness or safeguards.

### Stage 6 — Add capabilities as needed

- Serena for a demonstrated semantic-navigation problem.
- Playwright for actual browser-facing behavior.
- GitHub/Atlassian/search MCP for specific external workflows.
- QMD or a memory service for a sufficiently large, evaluated retrieval need.
- Docker/VM isolation before unattended or untrusted execution, with the agent actually operating inside the boundary.
- GSD/Superpowers orchestration/Oh My OpenAgent for a concrete coordination need; do not stack them indiscriminately.
- LiteLLM/Promptfoo/Langfuse/LangGraph/MLflow only for their distinct gateway, evaluation or application responsibilities.

## 12. Measuring token usage, cost and efficiency

Track these separately:

| Metric | Purpose |
|---|---|
| Correctly accepted tasks | Tests and human review define success |
| Total cost across all attempts / accepted tasks | Includes failures, retries and subagents |
| Fresh input, cache reads/writes, output/reasoning usage | Explains different billing rates instead of conflating token totals |
| Elapsed time and human correction effort | Measures practical developer efficiency |
| Repeated reads, tool calls and working-context size | Reveals retrieval waste and plugin overhead |
| Missing diagnostics or regressions | Detects lossy compression and premature completion |

For an OpenCode version exposing the documented `stats` command, run from the intended project:

```bash
opencode stats --days 7 --models --project ""
```

Check the selected major's CLI documentation before using this example. Compare tool estimates with provider billing. Request-based quotas and token-based charges are not interchangeable. `rtk gain` measures its command-output counterfactual, not the entire invoice.

### Small initial evaluation

Use 5–10 representative tasks, repeating important comparisons:

- Narrow fixture/test change.
- Bug requiring a failing regression.
- Multi-file Python change.
- Ledger/risk-invariant review.
- Version-specific library/API question.

Use equivalent isolated starting states. Fix model, reasoning setting, task inputs and acceptance criteria; record versions and cold/warm-cache conditions. Keep all retry and subagent costs. Do not overwrite a working tree to reset an experiment.

Compare the baseline, then RTK alone, then DCP if relevant. Compare another harness separately. A small pilot is directional evidence, not statistical proof or a universal savings guarantee.

## 13. What research and builder feedback support

### Harness choice can materially change cost

[Databricks' July 2026 engineering benchmark](https://www.databricks.com/blog/benchmarking-coding-agents-databricks-multi-million-line-codebase) used actual coding tasks and held-out tests. Some same-model/same-effort harness comparisons differed by more than 2× in cost; Pi sent about 3× less context per turn in its analysis. The authors explicitly reject the conclusion that one harness is always cheapest.

**Decision:** OpenCode is the balanced primary option for model flexibility and integrations; Pi deserves a controlled lean-harness comparison, not a guaranteed-winner label.

### RTK output compression does not guarantee bill savings

The [RTK README](https://github.com/rtk-ai/rtk/) distinguishes reduced Bash output from reduced bills and estimates tokens from bytes divided by four.

[JetBrains' RTK study](https://blog.jetbrains.com/ai/2026/07/rtk-claude-code-token-savings/) covered 425 billed trials with Claude Code, Sonnet 5 and RTK 0.43.0. It reported a 7.6% median cost increase at low reasoning effort and approximately no change at high effort, without a significant task-quality difference. Much built-in file/search output never crossed the RTK hook.

This is independent of RTK but conducted by another developer-tool vendor. It is not an OpenCode benchmark or proof that every later RTK release behaves identically.

**Decision:** trial RTK for supported noisy output; no promised 60–90% whole-session savings.

### Minimal useful instructions are preferable to boilerplate

[ETH Zurich / LogicStar's AGENTS.md evaluation](https://arxiv.org/abs/2602.11988) found no general success-rate improvement and over 20% higher average inference cost in its evaluated settings.

**Decision:** retain essential commands, safety boundaries and non-obvious conventions; avoid autogenerated repository encyclopedias and irrelevant always-on instructions.

### Tools and integrations have context costs

[Anthropic's context-engineering guidance](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) emphasizes small high-signal working context. Its [MCP/code-execution article](https://www.anthropic.com/engineering/code-execution-with-mcp) explains schema and intermediate-result overhead; benefits depend on client implementation.

[Microsoft's Playwright CLI guidance](https://github.com/microsoft/playwright-cli/blob/main/README.md) favors CLI+skills for many coding-agent workflows while retaining MCP for richer persistent interactions. [DCP's documentation](https://github.com/Opencode-DCP/opencode-dynamic-context-pruning/blob/master/README.md) acknowledges that pruning can invalidate cached prefixes.

**Decision:** prefer the smallest useful tool interface and measure actual cost, not just raw tokens.

### Builder feedback is useful but not a ranking by itself

[Pi's Hacker News discussion](https://news.ycombinator.com/item?id=47143754) includes praise for customization and concerns about integration/sandboxing effort. [Shopify's autoresearch account](https://shopify.engineering/autoresearch) shows useful metric-driven work, alongside unacceptable shortcuts that required human rejection.

[OpenCode issue 9858](https://github.com/anomalyco/opencode/issues/9858) reports fixed overhead from tools and an older DCP version; [issue 15660](https://github.com/anomalyco/opencode/issues/15660) reports concerns about tool/context usage. These are version-specific reports, not proof of current universal defects.

**Decision:** use anecdotes to identify experiments. Use verified outcomes and billing to choose the stack.

## 14. Fresh-laptop readiness checklist

- [ ] Native Apple-silicon toolchain and supported macOS are confirmed.
- [ ] Installation sources and reviewed versions are recorded; no unreviewed floating plugin stack.
- [ ] Project source is obtained and its Git boundary is deliberate.
- [ ] uv/Python 3.12, locked dependencies and offline verification work.
- [ ] OpenCode is the selected primary harness; alternatives are not all installed automatically.
- [ ] Two model choices, approved providers and spending/retry limits are established.
- [ ] Project instructions are short and relevant while preserving safety controls.
- [ ] Reviewed skills load on demand; custom skills are actually authored and verified.
- [ ] One documentation route works with version-aware queries.
- [ ] MCP servers are scoped and unrelated integrations stay disconnected.
- [ ] RTK is tested independently; raw errors and exit codes remain reliable.
- [ ] DCP, semantic services, memory and orchestration each have an adoption reason.
- [ ] Secrets are protected; no broker authority reaches the coding environment.
- [ ] Whole-task cost, correctness and human effort are measured before declaring savings.

**Bottom line:** use a small, model-flexible baseline and a complete optional catalog. Add complexity only when its measured benefit exceeds its context, cost and maintenance overhead.
