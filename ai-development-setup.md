---
title: General AI Development Setup - Fresh Mac
created: 2026-09-24
updated: 2026-09-24
researched: 2026-09-24
status: setup-guide
target: Fresh Apple-silicon Mac
tags:
  - ai/tooling
  - ai/context-engineering
  - development/setup
  - research
---

# General AI development setup

## 1. Scope and recommendation

A reusable setup for **any AI-assisted software project** on a fresh Apple-silicon Mac: applications, agents, data pipelines, research, or developer tooling. Assume no installed development tools, model accounts, credentials, skills, MCP connections, checkout, or memory service.

This guide is independent of any application repository. There are no required sibling files, external local folders, pre-existing notes, application CLI commands, fixed Python versions, or domain-specific services. Start with the [navigation guide](README.md).

Goals:

1. Reduce unnecessary tokens and total cost per correctly completed task.
2. Improve engineering efficiency and verification quality.
3. Switch models/providers without rebuilding the workflow.

**Recommended baseline:** iTerm2 + OpenCode + two model choices + the project's supported runtimes and verification tools + targeted retrieval/LSP + a few on-demand skills + one documentation route. Measure this baseline, then evaluate RTK independently.

OpenCode is already a **harness**: the application connecting a model to tools, permissions, context management, and the edit/test loop. An additional orchestration framework is not required.

This is a catalog, **not an instruction to install everything**. No installation, account connection, or paid benchmark is implied by following its links.

### Adoption tiers

| Tier | Meaning |
|---|---|
| Baseline | Establish before normal implementation |
| Trial | Compare independently with the baseline; retain only if useful |
| Optional | Add when a capability is needed |
| Alternative | Choose instead of another option or compare in isolation |
| Resource/custom | Documentation, service setup, or content to author—not an assumed downloadable tool |

## 2. Installation policy and Apple-silicon prerequisites

- Use native macOS **arm64** tools. Homebrew's supported Apple-silicon prefix is `/opt/homebrew`.
- Choose a macOS release supported by the selected tools. Add Rosetta only for a documented dependency that needs it.
- Prefer reviewed stable releases at least seven days old; record exact versions, plugin revisions, and image digests where practical.
- Upstream `@latest`, prerelease, global-install, and remote-shell examples are not a reproducible install manifest. Review the selected release and installer.
- Choose one installation owner per runtime/tool. Avoid competing Homebrew, npm, uv, mise, and standalone installations of the same executable.
- Select application runtime versions from its manifests and support policy. Keep coding-tool runtimes isolated; a tool's Python requirement does not change an application's requirement.
- Preserve each project's lockfile and dependency-release controls. Do not bypass them to make an install succeed.
- Authentication, subscriptions, API spending, shell integration, and permission grants are separate decisions. Keep secrets out of prompts, visible command arguments, notes, source, and skill files.
- Development agents should not receive production write authority, payment access, deployment credentials, or sensitive data by default.

## 3. Fresh Mac foundations

Installation links lead to official guidance, project sources, or package-maintainer documentation. Verification describes future checks, not completed installation evidence.

| Tool | Tier | Installation link | Setup and verification |
|---|---|---|---|
| Apple Command Line Tools | Baseline | [Requirements](https://docs.brew.sh/Installation#macos-requirements), [Apple downloads](https://developer.apple.com/download/all/) | Use the supported CLT flow; verify `xcode-select -p`. Full Xcode is not required solely for these CLI tools. |
| Homebrew | Baseline | [Installation](https://docs.brew.sh/Installation) | Native arm64 installation and documented shell setup; verify `brew --prefix` is `/opt/homebrew`. |
| iTerm2 | Recommended terminal | [Official downloads](https://iterm2.com/downloads.html), [features](https://iterm2.com/features.html) | Install a reviewed stable build. Verify splits, search, key bindings, font rendering, and your coding-agent TUI. |
| Ghostty | Terminal alternative | [Official binary installation](https://ghostty.org/docs/install/binary) | Signed/notarized macOS download; community Homebrew cask also repackages it. Verify the same agent/TUI workflow. |
| Git | Baseline | [Homebrew package](https://formulae.brew.sh/formula/git) | Use Apple's supplied Git or a reviewed Homebrew release; verify `git --version`. User owns identity/authentication decisions. |
| GitHub CLI (`gh`) | If using GitHub | [Installation](https://cli.github.com/) | Homebrew package `gh`; authenticate separately, then verify account/repository scope. |
| uv | For Python projects/tools | [Installation](https://docs.astral.sh/uv/getting-started/installation/) | Manage project environments and isolated Python tools; verify `uv --version`. Do not modify system Python. |
| Python | When required | [uv Python management](https://docs.astral.sh/uv/guides/install-python/) | Select the version supported by the target project or isolated tool. Do not impose one version on all AI projects. |
| Node.js LTS + npm | For selected JS tools/projects | [Official downloads](https://nodejs.org/en/download) | Choose one arm64 installation route and a supported version; verify `node --version` and `npm --version`. |
| ripgrep (`rg`) | Baseline | [Homebrew package](https://formulae.brew.sh/formula/ripgrep) | Fast bounded text searches; verify version and a fixture search. |
| fd | Baseline utility | [Installation](https://github.com/sharkdp/fd#installation) | Fast filename discovery; verify version and scoped lookup. |
| jq | Baseline | [Download/install](https://jqlang.org/download/) | Filter JSON before it enters model context; verify version and a small transformation. |
| VS Code | Optional visual editor | [macOS installation](https://code.visualstudio.com/docs/setup/mac) | arm64/Universal build; configure language support for the actual project. No extra AI subscription is required for ordinary editing. |
| Obsidian | Optional notes workspace | [Mac download](https://obsidian.md/download?os=mac) | Create a vault only if wanted; keep project-scoped notes. This guide does not depend on a vault. |

### Terminal choice

**Choose iTerm2 by default** for mature split panes, profiles, global search, a hotkey window, and optional shell integration. Separate panes for the agent, tests, and application logs make a development session easier to inspect. [Official feature reference](https://iterm2.com/features.html).

**Choose Ghostty instead** if you prefer its native, GPU-accelerated terminal approach and macOS/Linux availability. Use its [official download instructions](https://ghostty.org/docs/install/binary); do not disable Gatekeeper or other security checks to install it. Neither terminal is universally better for every keyboard/TUI workflow.

Choose one as your daily terminal; keeping macOS Terminal as a fallback is fine. Review shell-integration changes before enabling them. Avoid persistent paste/session logging of secrets and automatic AI transmission of terminal content. Test multiline paste, interrupts, colors, Unicode, scrolling, and agent shortcuts.

A terminal improves ergonomics; it does **not** directly reduce model tokens or replace RTK, caching, permissions, or an execution sandbox.

## 4. Coding harnesses and orchestration

| Tool | Tier | Installation/setup | Role and adoption check |
|---|---|---|---|
| OpenCode | Primary baseline | [Download](https://opencode.ai/download), [models](https://opencode.ai/docs/models/) | Multiple providers, skills, MCP, and LSP. Verify version, bounded read-only work, permissions, and a disposable edit/test task. |
| Pi | Alternative | [Quickstart](https://pi.dev/docs/latest/quickstart) | Minimal extensible harness. Package `@earendil-works/pi-coding-agent`; official npm route uses `--ignore-scripts`. Compare the same model/task against OpenCode. |
| Aider | Alternative | [Install](https://aider.chat/docs/install.html), [repository maps](https://aider.chat/docs/repomap.html) | Explicit file selection and bounded edits. Isolate `aider-chat` with uv. Review auto-commit behavior; architect/editor mode adds model requests. |
| Devin CLI | Alternative | [Quickstart](https://docs.devin.ai/cli) | Local coding workflow and model choices; check account/plan eligibility. Install only if selected. |
| Superpowers | Selected skills first | [OpenCode integration](https://github.com/obra/superpowers/blob/main/docs/README.opencode.md) | Full plugin adds a collection and bootstrap context. Evaluate its workflow overhead separately from a few reviewed skills. |
| GSD Core | Optional phase management | [Install guide](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/install-on-your-runtime.md) | Package `@opengsd/gsd-core`; use runtime-aware transformations rather than raw files for another agent. |
| Oh My OpenCode / Oh My OpenAgent | Optional heavy orchestration | [Current installation guide](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/guide/installation.md) | Upstream uses Oh My OpenAgent. Review bundled agents, hooks, MCPs, and provider settings; not a default cost-saving layer. |
| Bun | Optional prerequisite | [Installation](https://bun.com/docs/installation) | Add only when the chosen supported tool/installer needs it. |
| Autoresearch / Ralph-style loops | Optional bounded experiments | [Shopify account](https://shopify.engineering/autoresearch), [Pi extension](https://github.com/davebcn87/pi-autoresearch) | Require a measurable objective, iteration/spend limits, isolated state, and correctness checks. Never begin with an unbounded loop. |

Choose **one primary harness**. Model switching does not require installing several agents. Pi does not include built-in MCP, plan mode, or permission popups; matching those features requires extensions/external controls.

For parallel work, use separate worktrees and explicit ownership. Multiple agents should not write to the same checkout. Parallelism may reduce elapsed time while increasing aggregate tokens.

## 5. Token and context controls

| Tool/practice | Tier | Setup resource | Use and verification |
|---|---|---|---|
| Native compaction/pruning | Baseline | [OpenCode docs](https://opencode.ai/docs/), [v2 compaction](https://opencode.ai/v2/docs/compaction) | Start with the selected major's native behavior. Compaction is lossy; preserve active evidence. |
| Prompt caching | Baseline practice | [Caching guidance](https://openrouter.ai/docs/guides/best-practices/prompt-caching) | Stable prompt/tool prefixes can lower billed input cost; inspect provider cache reporting. |
| RTK | Trial | [Install package](https://formulae.brew.sh/formula/rtk), [integration/analytics](https://github.com/rtk-ai/rtk/) | Filter supported verbose commands. Preview installation, prevent double rewriting, preserve exit codes and raw diagnostics. |
| DCP | Later trial | [Official README](https://github.com/Opencode-DCP/opencode-dynamic-context-pruning/blob/master/README.md) | Long-session context management; pin a host-compatible release and compare cache effects, cost, and correctness. |
| Repomix | Occasional handoff | [Install](https://repomix.com/guide/installation), [token budgets](https://repomix.com/guide/command-line-options) | Pack an allowlisted subset; keep secret checks on and inspect before sharing. Current docs require Node.js 22+. |
| QMD | Optional local docs search | [Install/setup](https://github.com/tobi/qmd) | Package `@tobilu/qmd`; begin with one project collection and bounded CLI results. Model downloads/indexing need deliberate setup. |
| Small project instructions | Baseline practice | [Agent Skills format](https://agentskills.io/specification) | Exact commands, unusual constraints, and links—not a repository encyclopedia. |
| Session/model handoff | Baseline practice | Custom skill in section 7 | Short factual checkpoint instead of replaying a whole conversation. |

Search before reading; fetch relevant sections and symbols. Keep datasets, databases, environments, caches, generated archives, and full logs out of automatic context. Exclusions are not security boundaries.

Keep unrelated domain instructions out of startup context without removing safety rules. Prefer test/lint feedback to speculative reviews. Bound retries, subagents, output, and research scope. Cryptic output that loses diagnostic evidence is not a useful optimization.

### Version compatibility

Choose a reviewed release, then use [OpenCode v1](https://opencode.ai/docs/) or [v2](https://opencode.ai/v2/docs/) documentation as appropriate. Permissions, MCP fields, plugin keys, and compaction schemas differ. Superpowers documents v1 `plugin` versus v2 `plugins`; do not treat these as a universal template.

RTK installers can configure additional assistants or global hooks. Preview the selected installer and use one integration. DCP must also match the selected host major. History rewriting can invalidate cached prefixes, so fewer raw tokens can still mean a higher bill.

## 6. Model access, routing and local inference

### Model roles

| Role | Work | Selection |
|---|---|---|
| Fast | Narrow edits, fixtures, simple tests, documentation | Economical mini/Flash/Haiku-class or open-model candidate |
| Build | Normal multi-file implementation | Best measured quality/cost on representative tasks |
| Review | Architecture, difficult debugging, data/privacy/API invariants | Strong reasoning model; can initially share Build's model |

Start with **two models**. Current OpenAI/Anthropic/Google and GLM/Kimi/DeepSeek families are candidates, not permanent winners independent of price and task.

| Option | Tier | Setup/resource | Requirements and verification |
|---|---|---|---|
| Direct providers | Baseline option | [OpenCode providers](https://opencode.ai/docs/providers/) | Approved account, supported auth, privacy policy, and budget. No gateway is required for model switching. |
| OpenRouter | Optional convenience | [Quickstart](https://openrouter.ai/docs/quickstart), [routing](https://openrouter.ai/docs/guides/routing/provider-selection.md) | One account for many models; allowlist providers/fallbacks and inspect actual charges/serving endpoint. |
| Models.dev | Resource | [Catalog/API](https://github.com/anomalyco/models.dev) | Check exact model IDs, tool support, limits, releases, and prices against provider documentation. |
| LiteLLM | Optional later gateway | [Docker setup](https://docs.litellm.ai/docs/proxy/docker_quick_start), [budgets](https://docs.litellm.ai/docs/proxy/users) | Isolated service, reviewed pinned release/image, required storage, and tested budget enforcement. |
| Ollama | Optional local inference | [macOS installation](https://docs.ollama.com/macos) | Select model after assessing RAM, context, throughput, and tool quality. Keep endpoint local; approve large downloads separately. |

Record exact model IDs and reasoning settings. Switch at task boundaries or after a handoff; provider/model changes can lose warm caches and alter tool semantics. OpenAI-compatible APIs do not guarantee identical streaming, structured outputs, reasoning controls, or tool calls.

OpenCode v1's `small_model` is for auxiliary tasks such as titles, not automatic routing of all easy coding work. API access and consumer subscriptions are different products; use officially supported authentication.

Local inference has hardware/energy/maintenance costs and may not be fast or capable enough. For gateways, distinguish warnings, quotas, and hard spend controls. OpenRouter fallback must not silently broaden approved data recipients.

LiteLLM's [March 2026 advisory](https://docs.litellm.ai/blog/security-update-march-2026) identifies compromised releases 1.82.7/1.82.8; exclude them and review later releases normally. Its database-backed budget controls do not enforce caps in a DB-less deployment.

## 7. Skills: sources, installation and custom requirements

Skills are reusable workflows, not models. The [Agent Skills specification](https://agentskills.io/specification) loads metadata first, instructions on invocation, and references when needed. Large unused catalogs still consume context.

### Installable/adaptable skills

| Skill | Source/setup | Trigger and expected result |
|---|---|---|
| `test-driven-development` | [Superpowers](https://github.com/obra/superpowers/tree/main/skills/test-driven-development) | Failing regression, minimal fix, passing checks and negative cases. Do not inherit automatic commits or destructive resets. |
| `systematic-debugging` | [Superpowers](https://github.com/obra/superpowers/tree/main/skills/systematic-debugging) | Reproduce, trace data flow, test a hypothesis, verify the root-cause fix. |
| `verification-before-completion` | [Superpowers](https://github.com/obra/superpowers/tree/main/skills/verification-before-completion) | Fresh commands/results and explicit limits before a completion claim. |
| `differential-review` | [Trail of Bits](https://github.com/trailofbits/skills/tree/main/plugins/differential-review) | Evidence-backed review of sensitive diffs, trust boundaries and missing tests; not a whole-repo audit every turn. |
| `property-based-testing` | [Trail of Bits](https://github.com/trailofbits/skills/tree/main/plugins/property-based-testing) | Appropriate invariants, properties, and counterexamples. The testing library is a separate dependency choice. |
| Documentation lookup | [Context7 CLI/skills](https://context7.com/docs/clients/cli) | Bounded, version-specific answer with source. |
| Browser workflow | [Playwright CLI/skills](https://github.com/microsoft/playwright-cli/blob/main/README.md) | Targeted browser assertions and bounded snapshots when browser behavior is in scope. |

Resources: [Superpowers installation](https://github.com/obra/superpowers/blob/main/docs/README.opencode.md), [Trail of Bits collection](https://github.com/trailofbits/skills), [Skills.sh discovery/CLI](https://www.skills.sh/docs), and [Agent Skills format](https://agentskills.io/specification). Marketplace popularity is not a security approval.

Full Superpowers registers its collection and injects bootstrap instructions. Adapting a few portable skills is a different setup, not an assumed selective-plugin switch.

### Custom skills to author for each project

These are **specifications, not existing downloadable packages**. Use the Agent Skills format and the target project's real contracts/commands.

| Skill | Trigger | Required behavior/output |
|---|---|---|
| `project-verify` | Before review/completion | Run the project's documented tests, lint, typecheck/build as applicable; report commands, outcomes, negatives and unverified scope. |
| `data-evaluation-review` | Data pipeline, model, prompt, or retrieval changes | Check provenance, train/evaluation separation, representative test cases, leakage, reproducibility, privacy, and metric limitations. Time-based data needs availability/maturity checks. |
| `side-effect-safety-review` | Stateful API, workflow, database, or deployment changes | Check authorization, idempotency, retries, transactions, stale inputs, partial failure, rollback, and forbidden effects. |
| `session-handoff` | Task/model/session switch | Concise goal, constraints, decisions, files, verified checks, unresolved questions, and next action; no secrets/full logs. |

### Storage and acceptance

Keep one reviewed source of skill content and configure each harness's supported discovery paths. Newly authored Devin/shared content can live in `.devin/skills/<name>/SKILL.md`; global Devin configuration belongs under `~/.config/devin/`. OpenCode does not automatically discover `.devin/skills`, so configure an explicit supported path and verify it. Portable Markdown does not make hooks, tool names, or permissions portable.

Avoid redundant compatibility configs. Audit installer write scope, scripts, network access, licenses, and revisions. Verify relevant tasks load a skill and unrelated tasks do not. Skills must not silently authorize commits, pushes, destructive actions, subagents, or paid calls.

## 8. MCP and CLI integration catalog

[MCP](https://modelcontextprotocol.io/docs/getting-started/intro) standardizes tool/data connections; it is not inherently a token-saving feature. Schema, discovery, and result costs depend on the client.

| Integration | Tier | Setup link | Scope and acceptance |
|---|---|---|---|
| Context7 CLI + skill | Recommended docs route | [CLI setup](https://context7.com/docs/clients/cli) | Package `ctx7`, Node prerequisite; inspect generated paths/rules and verify a version-aware query. |
| Context7 MCP | Alternative docs route | [Client setup](https://context7.com/docs/resources/all-clients) | Approved remote/auth connection; verify discovery and a useful query. Do not enable both routes by default. |
| GitHub MCP | Optional, CLI first | [Official server](https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md) | Selected read-only tools and repository-scoped authorization. Prefer `gh` when sufficient. |
| Atlassian Rovo MCP | Optional | [Official setup](https://support.atlassian.com/atlassian-rovo-mcp-server/docs/getting-started-with-the-atlassian-remote-mcp-server/) | Authorized org/account access; enable for Jira/Confluence tasks, not every session. |
| Serena | Optional semantic trial | [Installation](https://oraios.github.io/serena/02-usage/010_installation.html) | Current docs isolate `serena-agent` with Python 3.13. Use a supported language backend and validate symbols/references before edits. |
| Playwright CLI + skills | Optional browser work | [Installation](https://github.com/microsoft/playwright-cli/blob/main/README.md) | Package `@playwright/cli`; satisfy browser-runtime requirements and use a dedicated profile/local fixture. |
| Playwright MCP | Stateful-browser alternative | [Official server](https://github.com/microsoft/playwright-mcp) | Choose when persistent inspection outweighs extra context; avoid personal browser sessions. |
| Web search, e.g. Exa | Optional | [Exa setup](https://exa.ai/docs/reference/exa-mcp) | One provider if native research is insufficient; approve cost/privacy and expose only needed search/fetch tools. |
| QMD/Obsidian | Optional | [QMD](https://github.com/tobi/qmd), [Obsidian](https://obsidian.md/download?os=mac) | Selected project collections; local CLI/file access first. No whole-vault access is required. |
| Mem0 | Optional memory foundation | [Open-source quickstart](https://docs.mem0.ai/open-source/python-quickstart) | Isolated environment, project namespaces, extraction/embedding budget, bounded retrieval. |
| Chroma | Optional memory storage | [Getting started](https://docs.trychroma.com/docs/overview/getting-started) | Only if chosen by the memory design; not mandatory for every Mem0 deployment. |
| Cross-agent memory MCP | Custom, deferred | [MCP docs](https://modelcontextprotocol.io/docs/getting-started/intro) | Separately build or verify a server; test provenance, namespaces, stale-memory handling and limits. No generic installer is asserted. |
| Production-write integrations | Excluded from baseline | No setup planned | Do not grant deployment, payment, destructive database, or other real-world authority by default. |

Enable servers by task/agent, not globally. Native lazy discovery/code execution may help where supported; verify actual behavior. Use the selected host's schema rather than copying another client's JSON.

Prefer approved OAuth or protected credential injection. Treat websites, issues, docs and memory as untrusted content, not authority to run commands. Permissions and ignore files are not OS isolation; shell/plugins can retain host filesystem/network access.

## 9. Verification, reproducibility and supporting tools

Choose verification tools for the project's language and workflow; this list does not make every AI project a Python project.

| Tool | Tier | Installation/setup | Use and check |
|---|---|---|---|
| pytest | For Python tests | [Getting started](https://docs.pytest.org/en/stable/getting-started.html) | Locked project environment; positive and negative regression cases. |
| Ruff | For Python lint/format | [Installation](https://docs.astral.sh/ruff/installation/) | Use the project version, not an unrelated global release as verification evidence. |
| Language server / Pyright | Appropriate language support | [Pyright](https://github.com/microsoft/pyright/blob/main/docs/installation.md), [OpenCode LSP](https://opencode.ai/docs/lsp/) | Pyright is for Python; use a supported server for other languages. Check actual interpreter/import/project paths. |
| Hypothesis | Optional Python property tests | [Installation/tutorial](https://hypothesis.readthedocs.io/en/latest/tutorial/introduction.html) | Reviewed dev dependency when properties warrant it; skills do not install it implicitly. |
| pre-commit | Optional deterministic checks | [Setup](https://pre-commit.com/index.html) | Pin/review hooks and activate deliberately; never bypass existing controls. |
| pip-audit | Optional Python dependency audit | [Official package docs](https://pypi.org/project/pip-audit/) | Audit the intended dependency set; report rather than silently upgrade or weaken release controls. |
| GitHub Actions / CI | Recommended release gate | [Quickstart](https://docs.github.com/en/actions/get-started/quickstart) | Hosted service, not a laptop package; mirror project checks and pin reviewed actions. |
| just | Optional task aliases | [Installation](https://just.systems/man/en/installation.html) | Thin aliases around authoritative commands, not duplicate build logic. |
| mise | Optional version manager | [Getting started](https://mise.jdx.dev/getting-started.html) | Alternative runtime ownership; review executable config before trust. |
| direnv | Optional environment loading | [Installation](https://direnv.net/docs/installation.html) | Shell hook and `.envrc` approval are explicit; do not inject secrets into every process. |
| Docker Desktop | Optional isolated execution | [Mac install](https://docs.docker.com/desktop/setup/install/mac-install) | License/resources, arm64 images and restricted mounts; installing Docker alone does not sandbox a host agent. |
| OpenCode usage reporting | Baseline built-in capability | [CLI docs](https://opencode.ai/docs/cli/) | Match selected version; compare estimates with provider billing. |
| RTK gain | Trial telemetry | [Analytics](https://github.com/rtk-ai/rtk/) | Measures command-output compression, not the whole invoice. |

For JavaScript/TypeScript, Rust, Go, or other projects, use their declared test/lint/typecheck/build tools and lockfiles. Do not install a second toolchain just to match the examples below.

## 10. Later application tooling—not coding prerequisites

| Tool | Setup link | Adoption condition |
|---|---|---|
| Promptfoo | [Getting started](https://www.promptfoo.dev/docs/getting-started/) | Application prompt/provider regression testing; approve paid eval budgets. |
| Langfuse | [Docker setup](https://langfuse.com/self-hosting/deployment/docker-compose), [cost tracking](https://langfuse.com/docs/observability/features/token-and-cost-tracking) | Instrument application traces with redaction. It does not automatically capture every coding-agent call. |
| LiteLLM | [Gateway setup](https://docs.litellm.ai/docs/proxy/docker_quick_start) | Centralized routing/accounting justifies a service; see budget/security caveats above. |
| LangGraph | [Installation](https://docs.langchain.com/oss/python/langgraph/install) | The application needs stateful graph orchestration/checkpoints; evaluate before adding a framework. |
| MLflow | [Tracking quickstart](https://mlflow.org/docs/latest/ml/getting-started/quickstart/) | Model/experiment tracking is needed; integrate with the project's manifests and artifact policy. |

Dataframe/ML libraries, databases, vector stores, queues, GPUs and cloud services depend on the actual application. None is mandatory merely because a project uses AI. Install application dependencies in its own reviewed, locked environment.

## 11. Setup order from an empty laptop

### Stage 0 — Select versions, accounts and boundaries

Choose one primary harness, compatible stable releases, approved providers, privacy requirements and an owner-approved budget. Define accessible folders and side effects. Keep production credentials absent. Record exact resolved versions rather than silently inheriting another machine's setup.

**Exit check:** optional tools stay optional; account/authentication/spending decisions are explicit.

### Stage 1 — Bootstrap the Mac

Install Apple CLT, native Homebrew, **iTerm2 or Ghostty**, Git, and retrieval utilities. Add Python/uv and/or Node according to the selected tools and target projects. Add a visual editor/notes app only if desired. Verify arm64 binaries, PATH, terminal shortcuts, multiline paste and interrupts.

**Exit check:** each command resolves to the intended installation owner; versions are recorded.

### Stage 2 — Obtain and verify a target project

Obtain an approved checkout or create a project deliberately. Inspect its README, runtime manifests, lockfiles and CI configuration. Use its documented dependency groups and verification commands from its root; do not assume a directory name or copy application-specific commands from another project.

For a **uv-based Python project that already declares pytest and Ruff**, this may resemble:

```bash
uv sync --locked
uv run --no-sync pytest -q
uv run --no-sync ruff check .
```

Select any required development groups/extras according to that project's configuration. For an **npm project with a package-lock and declared test/lint scripts**, it may resemble:

```bash
npm ci
npm test
npm run lint
```

These are conditional examples, not universal commands. Use the actual package manager, runtime version, typecheck/build steps and isolation requirements. Dependency installation may download packages; ordinary unit tests should prefer synthetic fixtures/mocked external services.

**Exit check:** reproducible dependency install and relevant project checks pass; skipped or unavailable checks are reported.

### Stage 3 — Establish plain OpenCode

Install the chosen release, authenticate approved providers and configure Fast and Build/Review model choices. Start with native context management and bounded retries/output/spend. Add short project instructions with real commands, unusual constraints, links, and approval boundaries. Test read-only work, a disposable edit/test task and a model switch with a handoff.

**Exit check:** useful tool use/model switching works without an orchestration plugin.

### Stage 4 — Add relevant knowledge tools

Enable language support for the project. Add selected reviewed skills and author its custom specifications. Choose Context7 CLI+skill or MCP. Verify on-demand loading and a version-aware docs query. Keep unrelated servers disconnected.

**Exit check:** relevant evidence is retrieved without repeatedly reading the whole project; startup/tool overhead is measured.

### Stage 5 — Compare additions independently

Measure baseline OpenCode, then RTK alone with diagnostic/exit-code checks. Try DCP only for a measured long-context problem and inspect cache effects. Compare another harness separately with equivalent tasks/model settings. Do not run competing agents against the same writable checkout.

**Exit check:** keep an add-on only when the intended metric improves without weakening correctness or safeguards.

### Stage 6 — Add capabilities when needed

Add semantic navigation, browsers, external MCPs, documentation search or memory for concrete tasks. Add restricted Docker/VM execution before unattended/untrusted work. Select orchestration only for a coordination problem; do not stack every framework. Add gateways, evals, tracing and application frameworks only when their distinct role is justified.

## 12. Measuring token usage, cost and efficiency

| Metric | Purpose |
|---|---|
| Correctly accepted tasks | Tests and human review define success |
| Cost across all attempts / accepted tasks | Includes failures, retries and subagents |
| Fresh input, cached input/writes, output/reasoning | Separates differently priced token categories |
| Elapsed time and human correction | Measures practical efficiency |
| Repeated reads, tool calls and working context | Exposes retrieval/plugin overhead |
| Missing evidence or regressions | Detects lossy compression and premature success claims |

For an OpenCode version exposing the documented command, from the intended project:

```bash
opencode stats --days 7 --models --project ""
```

Verify selected-major syntax. Compare estimates with provider billing: request quotas, token counts and charges are different. `rtk gain` is not an invoice.

Use 5–10 representative tasks: a fixture/test change, a bug with a failing regression, a multi-file change, a data/privacy/API-invariant review, and a version-specific library question. Repeat important comparisons from equivalent isolated starting states. Fix model/reasoning/task/acceptance criteria and record cold/warm-cache conditions, versions and every attempt. A small pilot is directional, not a universal savings guarantee.

## 13. What research and builder feedback support

### Harness choice matters

[Databricks' July 2026 benchmark](https://www.databricks.com/blog/benchmarking-coding-agents-databricks-multi-million-line-codebase) used real engineering tasks and held-out tests. Some same-model/same-effort harness comparisons differed by over 2× in cost; Pi sent about 3× less context per turn in its analysis. This does not establish a universal cheapest harness.

**Decision:** OpenCode is the balanced baseline; Pi is a controlled comparison, not a guaranteed winner.

### Compression is not billing savings

[RTK's README](https://github.com/rtk-ai/rtk/) distinguishes Bash-output compression from total bills and estimates tokens using bytes divided by four. [JetBrains' 425-trial study](https://blog.jetbrains.com/ai/2026/07/rtk-claude-code-token-savings/) reported a 7.6% median low-effort cost increase and approximately unchanged high-effort cost, without significant quality differences, for its Claude Code/Sonnet 5/RTK 0.43.0 setup. Much built-in file/search output bypassed RTK.

The study is independent of RTK but from another developer-tool vendor; it is not a benchmark of every newer release or OpenCode. **Decision:** trial RTK without promising 60–90% whole-session savings.

### Instructions and tools also cost context

[ETH Zurich/LogicStar's AGENTS.md evaluation](https://arxiv.org/abs/2602.11988) found no general success-rate improvement and over 20% higher average inference cost in its settings. Keep essential commands and safety boundaries; avoid irrelevant boilerplate.

[Anthropic's context-engineering guidance](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) and [MCP/code execution](https://www.anthropic.com/engineering/code-execution-with-mcp) explain high-signal context and schema/result overhead. [Microsoft's Playwright CLI guidance](https://github.com/microsoft/playwright-cli/blob/main/README.md) favors CLI+skills for many coding workflows while retaining MCP for richer interactions. [DCP](https://github.com/Opencode-DCP/opencode-dynamic-context-pruning/blob/master/README.md) acknowledges cache invalidation from pruning.

**Decision:** choose the smallest useful interface and measure actual cost.

### Builder feedback is a signal, not proof

[Pi's Hacker News discussion](https://news.ycombinator.com/item?id=47143754) describes customization benefits and integration/sandboxing tradeoffs. [Shopify's autoresearch account](https://shopify.engineering/autoresearch) shows useful optimization alongside unacceptable shortcuts requiring review. [OpenCode issue 9858](https://github.com/anomalyco/opencode/issues/9858) and [issue 15660](https://github.com/anomalyco/opencode/issues/15660) record version-specific context/overhead concerns, not universal current defects.

**Decision:** use anecdotes to choose experiments, and verified outcomes/billing to choose the stack.

## 14. Fresh-laptop readiness checklist

- [ ] Supported macOS and native Apple-silicon tools are confirmed.
- [ ] iTerm2 or Ghostty works with the selected coding-agent TUI and shell.
- [ ] Reviewed installation sources and versions are recorded.
- [ ] Target project source and repository boundary are deliberate.
- [ ] Its supported runtimes, locked dependencies and verification commands work.
- [ ] One primary harness and two approved model choices are selected.
- [ ] Provider budgets, privacy requirements, retry limits and credentials are controlled.
- [ ] Project instructions are short, relevant and preserve safety boundaries.
- [ ] Reviewed skills load on demand; custom project skills are authored and verified.
- [ ] One version-aware documentation route works.
- [ ] MCP servers are scoped and unrelated integrations remain disconnected.
- [ ] RTK is independently tested with reliable raw errors and exit codes.
- [ ] DCP, semantic services, memory and orchestration have measured adoption reasons.
- [ ] No production side-effect authority is exposed by default.
- [ ] Cost, correctness and human effort are measured before declaring savings.

**Bottom line:** a portable, model-flexible baseline with project-specific runtime and verification choices. Add complexity only when it justifies its context, cost and maintenance overhead.
