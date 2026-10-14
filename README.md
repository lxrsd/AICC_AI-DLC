# AICC AI-DLC

AICC AI-DLC gives engineers one standardized, AI-powered way to plan, design, build, test, and deliver software. It runs on the **official [AWS AI-DLC](https://github.com/awslabs/aidlc-workflows) framework** — AICC does not build its own runtime or installer. Use your preferred supported AI coding tool (Claude Code, Kiro CLI, or Cursor) and follow the Quick Start below.

## Quick Start — Install & Run

You install the **official AWS AI-DLC runtime once per computer**, then configure your chosen tool **per application project**. You do **not** need to clone this repository to use the runtime.

> The commands below are the **official AWS installers** (not AICC-built). Please review any remote install script before running it.

**macOS / Linux / WSL** — [detailed macOS guide](docs/guide/18-install-and-lifecycle.md):

```bash
curl -fsSL https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.sh | sh
```

**Windows (PowerShell)** — run in a normal (non-admin) PowerShell window:

```powershell
irm https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.ps1 | iex
```

You must also install your chosen AI coding tool separately — see [Choose your AI development tool](#choose-your-ai-development-tool).

**Verify the installation** (open a new terminal first if `aidlc` is not found):

```bash
aidlc version   # prints the installed runtime version
aidlc doctor    # checks runtime health and reports fixes
```

If `aidlc` is not found, your PATH needs the install directory — follow the instruction the installer printed, then re-open your terminal.

## Choose Your AI Development Tool

Install your preferred tool from its vendor (separately from AI-DLC), then run its configure command **from your application project's root**. Commands are the official AWS ones.

| Tool | Configure (in your project) | Open | Invoke |
| --- | --- | --- | --- |
| **Claude Code** | `aidlc config --harness claude` | `claude` | `/aidlc` |
| **Kiro CLI** (>= 2.24.1, v3) | `aidlc config --harness kiro-ide` | `kiro-cli` | `/aidlc` |
| **Kiro CLI** (>= 2.6, legacy) | `aidlc config --harness kiro` | `kiro-cli chat` | `/aidlc` |
| **Cursor** | `aidlc config --harness cursor` | Open Cursor, or run `agent` | `/aidlc` |

> **Kiro note (version matters):** current Kiro CLI (v3, **>= 2.24.1**) uses the `kiro-ide` harness; older Kiro CLI (**>= 2.6**, legacy) uses the `kiro` harness. Check `kiro-cli --version` and pick the matching row.

After configuring, verify with `aidlc doctor`. Full per-tool setup (providers, trust prompts, prerequisites): [harness guides](docs/guide/harnesses/README.md) — [Claude Code](docs/guide/01-getting-started.md) · [Kiro CLI](docs/guide/harnesses/kiro-cli.md) · [Kiro IDE / v3](docs/guide/harnesses/kiro-ide.md) · [Cursor](docs/guide/harnesses/cursor.md).

## Start Your First Application

Keep your **application code in your own repository** — not inside this AICC framework repo.

```bash
mkdir -p ~/Projects/expense-management-demo
cd ~/Projects/expense-management-demo
git init
```

Configure your tool in that folder (example: Claude Code), then open it and start AI-DLC:

```bash
aidlc config --harness claude   # once per project
claude                          # open your tool in the project
```

Inside the tool, invoke AI-DLC with a description of what you want to build:

```text
/aidlc Build an expense management application with receipt capture and approval workflows
```

(Codex uses `$aidlc`; in Kiro IDE, pick **aidlc** in the chat agent picker first.)

**What happens next:** AI-DLC selects a workflow from your request, asks for the decisions it needs, and **stops at human approval gates** before moving forward through requirements → design → approval → code → tests → evidence. Artifacts are written under your project at `aidlc/spaces/<space>/intents/<YYMMDD>-<label>/`. Full walkthrough: [Your First Workflow](docs/guide/02-your-first-workflow.md).

## Collaborative AI-DLC — Working with a Team

- **Individual AI-DLC** — one engineer builds an application end to end on their own machine (everything above).
- **Collaborative AI-DLC** — multiple engineers work on the **same** application using the official AWS multi-engineer capabilities: shared context, coordinated ownership of **units of work**, approval workflows, and controlled integration.

Collaborative development is built on the framework's official multi-team/ensemble Construction — see [Construction and the Swarm](docs/harness-engineering/08-construction-and-swarm.md).

> ⚠️ **Pending validation.** AICC has **not yet validated** the collaborative workflow end-to-end on this environment. Treat the linked material as the official reference until our validation is complete (see [Current development status](#current-development-status)).

## Documentation

- **Install & lifecycle (macOS / Linux / Windows):** [docs/guide/18-install-and-lifecycle.md](docs/guide/18-install-and-lifecycle.md)
- **Getting started:** [docs/guide/01-getting-started.md](docs/guide/01-getting-started.md)
- **Your first workflow:** [docs/guide/02-your-first-workflow.md](docs/guide/02-your-first-workflow.md)
- **Harness guides (per tool):** [docs/guide/harnesses/](docs/guide/harnesses/) — [Kiro CLI](docs/guide/harnesses/kiro-cli.md), [Kiro IDE / v3](docs/guide/harnesses/kiro-ide.md), [Cursor](docs/guide/harnesses/cursor.md)
- **Collaborative / multi-team Construction:** [docs/harness-engineering/08-construction-and-swarm.md](docs/harness-engineering/08-construction-and-swarm.md)
- **Preserved official AWS README:** [README.aws-upstream.md](README.aws-upstream.md)
- **Full official docs:** [docs/](docs/) ([guide](docs/guide/), [reference](docs/reference/), [roadmap](docs/roadmap.md))

> AICC-specific step-by-step macOS, Windows, and collaborative onboarding guides are **planned**; until published, use the official guides linked above.

---

## About AICC AI-DLC

### Mission & architecture

AICC AI-DLC adopts the **official AWS AI-DLC framework directly** to give AICC
engineers one standardized AI-powered development solution, in two modes:

- **Individual AI-DLC** — a single engineer builds an application through the
  complete AWS AI-DLC lifecycle on their own machine.
- **Collaborative AI-DLC** — multiple engineers collaborate on the same
  application using AWS's official collaborative capabilities.

The core lifecycle engine, agents, harness integrations, and approval
mechanisms come from AWS and live under [`core/`](core/). We **preserve the
official AWS implementation and full Git history** (the engine is unmodified)
and track AWS upstream. AICC adds only onboarding, documentation, configuration,
and improvements — never a reimplementation of what AWS maintains.

**Built-in Multi-Team Construction vs. the AWS Collaborative sample.** The engine
already ships multi-engineer Construction (Units of work, Bolts, ensemble
topologies, per-Unit worktree isolation, enforced reviewer receipts) — this is
what our Collaborative mode uses. AWS's *separate* Collaborative AI-DLC **sample
application** is **not vendored or integrated** here, and we make no claim that
it is.

### Supported harnesses

The official core is harness-neutral and ships integrations for all of these
(see [`harness/`](harness/)): Claude Code, Kiro CLI, Kiro IDE, Codex CLI,
Cursor, opencode, GitHub Copilot. AICC's onboarding examples focus on **Claude
Code, Kiro CLI, and Cursor**.

### Current development status

| Item | Status |
|------|--------|
| Official AWS source — downloaded & preserved | ✅ **Working** — complete, unmodified, vendored |
| Git history — preserved | ✅ **Working** — full upstream history retained |
| AICC development repository — created & pushed | ✅ **Working** — this repo, tracking AWS upstream |
| Official engine, agents, harnesses, approval mechanisms | 📘 **Upstream-documented** — shipped by AWS; not yet exercised here |
| Individual lifecycle — end-to-end on Mac | 🟡 **Pending AICC validation** |
| Collaborative lifecycle — multi-engineer | 🟡 **Pending AICC validation** |
| Simplified AICC engineer installation | ⚪ **Planned — not implemented** |
| Operational migration | ⚪ **Not started** |

Legend: ✅ working & verified here · 📘 available/documented upstream, not AICC-tested · 🟡 pending AICC validation · ⚪ planned / not started.

### Roadmap & acceptance criteria

**Roadmap:** baseline (done) → green test baseline → individual validation →
collaborative validation → AICC onboarding layer → operational planning.

**Individual AI-DLC is complete when:** a new engineer can follow the documented
install steps on a supported machine; the official runtime works with the
selected harness; a real application completes the applicable lifecycle; human
approval gates and lifecycle artifacts are verified; and the engineer can
complete the GitHub delivery process.

**Collaborative AI-DLC is complete when:** two or more engineers participate in
the same project; team members coordinate and own separate units of work; shared
context and dependencies are handled correctly; engineers complete their work
using supported AI tools; changes pass review and controlled integration; the
team delivers one working application; and the process is repeatable and
documented.

*These criteria are not yet passed.*

### Repository structure

```
AICC_AI-DLC/
├── core/            # the harness-neutral AI-DLC engine (from AWS, unmodified)
├── harness/         # per-tool integrations: claude, kiro, kiro-ide, codex, cursor, opencode, copilot
├── plugins/         # optional plugins (e.g. test-pro)
├── docs/            # official documentation (guide, reference, harness-engineering, roadmap)
├── scripts/         # build / lint / release / tooling scripts
├── tests/           # the official test suite
├── .github/         # upstream CI workflows
├── LICENSE          # MIT-0 (Amazon.com, Inc. or its affiliates)
├── README.aws-upstream.md   # the ORIGINAL official AWS README (preserved verbatim)
└── README.md        # this AICC overview
```

### Official AWS attribution & license

- **Upstream project:** [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) (official AWS Labs AI-DLC).
- **License:** **MIT-0** (MIT No Attribution) — see [`LICENSE`](LICENSE). Copyright Amazon.com, Inc. or its affiliates.
- Official documentation is preserved here: [`README.aws-upstream.md`](README.aws-upstream.md), [`docs/`](docs/), [`AGENTS.md`](AGENTS.md), [`CONTRIBUTING.md`](CONTRIBUTING.md), [`DEVELOPERS.md`](DEVELOPERS.md), [`CHANGELOG.md`](CHANGELOG.md).

AICC AI-DLC adds AICC-specific packaging and (later) configuration on top of this upstream work; it does not claim ownership of the official AWS framework.
