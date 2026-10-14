# AICC AI-DLC

AICC AI-DLC gives engineers one standardized, AI-powered way to plan, design, build, test, and deliver software. It runs on the **official [AWS AI-DLC](https://github.com/awslabs/aidlc-workflows) framework** — AICC does not build its own runtime or installer; you use the official AWS runtime and its supported harnesses exactly as provided. Pick your AI coding tool (Claude Code, Kiro CLI, or Cursor) and follow the Quick Start below.

## Quick Start — Install and Run

Three things, in order:

1. **Install the official AWS AI-DLC runtime** — once per computer.
2. **Install your AI coding tool** (Claude Code, Kiro CLI, or Cursor) — separately, from its vendor.
3. **Configure the runtime for your project** — once per application project, then start AI-DLC inside your tool.

You do **not** need to clone this repository to use the runtime. Full step-by-step setup is below: [macOS](#macos--step-by-step-installation) · [Windows](#windows--step-by-step-installation).

**Install the runtime (official AWS installers — review remote scripts before running):**

macOS / Linux / WSL (Terminal):

```bash
curl -fsSL https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.sh | sh
```

Windows (PowerShell):

```powershell
irm https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.ps1 | iex
```

**Verify the runtime** (open a new terminal first if `aidlc` is not found):

```bash
aidlc version
```

## macOS — Step-by-Step Installation

### Step 1 — Prerequisites

You need a supported Mac with Terminal, Git, one supported AI coding tool (Claude Code, Kiro CLI, or Cursor), and whatever model-provider access that tool requires. **The AI coding tool is installed separately from AWS AI-DLC.**

Check Git is available (Terminal):

```bash
git --version
```

If Git is missing, macOS prompts to install the Command Line Tools; accept it, then re-run the check.

### Step 2 — Install the official AWS AI-DLC runtime

This downloads the **official AWS runtime** (not an AICC-built installer). Review the script first if your policy requires it.

```bash
curl -fsSL https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.sh | sh
```

If the installer prints a PATH instruction, follow it, then open a new Terminal window before continuing.

### Step 3 — Confirm the runtime installation

This checks the **machine-level** runtime only (not any project).

```bash
aidlc version
```

### Step 4 — Create the application project

Create and enter a sample project directory. You may choose another location — just use the same directory for every step that follows.

Create the directory:

```bash
mkdir -p "$HOME/Desktop/aicc-aidlc-test"
```

Enter it:

```bash
cd "$HOME/Desktop/aicc-aidlc-test"
```

Initialize Git:

```bash
git init
```

Confirm you are in the right place before configuring:

```bash
pwd
```

### Step 5 — Configure your harness (inside the project)

Run the configure command **from the project directory** you just entered. Pick the one block for your tool. (Do not open the AI tool yet.)

Claude Code:

```bash
aidlc config --harness claude
```

Kiro CLI v3 (version 2.24.1 or later — check with `kiro-cli --version`):

```bash
aidlc config --harness kiro-ide
```

Kiro CLI legacy (version 2.6 up to before 2.24.1):

```bash
aidlc config --harness kiro
```

Cursor:

```bash
aidlc config --harness cursor
```

### Step 6 — Interactive setup prompts

If you run `aidlc config` without a flag it starts an interactive wizard. In plain language:

- **Recommended defaults vs. customize.** The first prompt offers `1. Yes, use recommended defaults`, `2. No, customize step by step`, or `3. Exit`. Pressing **Enter** accepts the default (option 1). Defaults record MCP servers on, all installed plugins, your current model provider preserved, and the balanced effort preset.
- **Customize** walks six steps — Harness, Model provider, Model effort preset, Plugins, MCP servers, and where to record the preset. Enter accepts the shown `[default]` at each step. **Type the menu number** shown; do not type a free-form description.
- **MCP servers.** This controls whether a `.mcp.json` with the shipped servers is written. For a simple local test with no MCP integrations, choose **off** in the customize path; if you need MCP integrations, choose **on** per your project's needs.
- **Model provider.** Keep your current provider (default) unless you specifically use Amazon Bedrock, in which case choose it and enter your region/profile. Kiro provides model access itself, so Kiro rows handle the model differently (a session-model choice rather than a provider).
- **Status line (Claude Code).** You may be asked whether to show AI-DLC's progress in the status line; either answer is fine and is stored in your own gitignored settings.

To resume an interrupted setup, simply run the configure command again from the project directory:

```bash
aidlc config
```

### Step 7 — Verify the project configuration

Run this **from the application project root** (the same directory you configured). It reports project configuration, not just the machine.

```bash
aidlc doctor
```

Read the result by section:

- **Machine** — runtime installed and healthy.
- **Project (.<harness>)** — the project is configured for the correct harness, and required hooks/files are present.
- **Framework integrity** — the framework files are intact.

`aidlc doctor` ends with a count like `0 problems, N warnings.` **Warnings are advisory, not failures.** On a brand-new project a few are expected (for example, "hooks have not run in this project yet" clears after your first chat; "uncommitted changes under `aidlc/`" clears after you commit). A **`fail`** row is a real problem and prints its own fix. Machine-level success alone does **not** mean your project is configured — rely on the Project section.

### Step 8 — Open your AI coding tool and activate AI-DLC

Open the tool **in the configured project**, then invoke AI-DLC. See [Choose Your AI Development Tool](#choose-your-ai-development-tool) for the per-tool launch and invoke details, and [Confirm AI-DLC is active](#confirm-ai-dlc-is-active-read-this-first) before you build.

## Windows — Step-by-Step Installation

Use **PowerShell** for every block in this section (not Git Bash or CMD).

### Step 1 — Prerequisites

You need Windows with PowerShell, Git, one supported AI coding tool, and its required model-provider access. The AI coding tool is installed separately from AWS AI-DLC.

```powershell
git --version
```

### Step 2 — Install the official AWS AI-DLC runtime

Run in a **normal (non-administrator)** PowerShell window. This installs the **official AWS runtime** and registers its bin directory on your User PATH.

```powershell
irm https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.ps1 | iex
```

Do not disable TLS or certificate validation. If PATH was just changed, **close and reopen PowerShell** before the next step.

### Step 3 — Confirm the runtime installation

```powershell
aidlc version
```

If `aidlc` is not recognized, open a new PowerShell window and try again.

### Step 4 — Create the application project

Resolve your real Desktop first (Windows may redirect Desktop to OneDrive), then create and enter the project under it.

Resolve the Desktop path and store it:

```powershell
$Desktop = [Environment]::GetFolderPath('Desktop')
```

Create the project directory:

```powershell
New-Item -ItemType Directory -Force -Path (Join-Path $Desktop 'aicc-aidlc-test') | Out-Null
```

Enter it:

```powershell
Set-Location (Join-Path $Desktop 'aicc-aidlc-test')
```

Initialize Git:

```powershell
git init
```

Confirm the current directory:

```powershell
Get-Location
```

### Step 5 — Configure your harness (inside the project)

Run the one block for your tool **from the project directory**. Do not open the AI tool yet.

Claude Code:

```powershell
aidlc config --harness claude
```

Kiro CLI v3 (2.24.1 or later — check with `kiro-cli --version`):

```powershell
aidlc config --harness kiro-ide
```

Kiro CLI legacy (2.6 up to before 2.24.1):

```powershell
aidlc config --harness kiro
```

Cursor:

```powershell
aidlc config --harness cursor
```

### Step 6 — Interactive setup prompts

The prompts are identical to macOS — see [Step 6 above](#step-6--interactive-setup-prompts). Resume an interrupted run with:

```powershell
aidlc config
```

### Step 7 — Verify the project configuration

Run from the application project root:

```powershell
aidlc doctor
```

Interpret the sections exactly as in the [macOS Step 7](#step-7--verify-the-project-configuration): Machine, Project, and Framework integrity, with advisory warnings distinct from `fail` rows.

### Step 8 — Open your AI coding tool and activate AI-DLC

Open your tool in the configured project and invoke AI-DLC — see [Choose Your AI Development Tool](#choose-your-ai-development-tool) and [Confirm AI-DLC is active](#confirm-ai-dlc-is-active-read-this-first).

## Choose Your AI Development Tool

Configure the harness from your project root (above), then open the tool and invoke AI-DLC. Each tool differs slightly.

### Claude Code (CLI)

Check the CLI is available (Terminal / PowerShell):

```bash
claude --version
```

Configure the harness in your project (if not already done):

```bash
aidlc config --harness claude
```

Open Claude Code in the project:

```bash
claude
```

Inside Claude Code, invoke AI-DLC by typing `/aidlc` followed by what you want to build. Success looks like AI-DLC responding with a workflow or plan-selection prompt — not ordinary code generation.

### Claude Code (Desktop — Code workspace)

Claude's desktop app can run AI-DLC through its **Code** workspace (not ordinary Chat):

1. Open the Claude desktop application.
2. Select the **Code** workspace (not Chat).
3. Open your local application project directory (the one you configured with `aidlc config --harness claude`).
4. Start a Code session in that project.
5. Type `/aidlc` with your request.
6. Confirm the AI-DLC workflow/plan-selection interface appears.

The AI-DLC plan-selection interface has been observed in Claude Desktop Code with AWS AI-DLC 2.11.0. Desktop-only users do not need the standalone `claude` CLI if their Desktop Code integration works — but note that `aidlc doctor` may still warn that the standalone CLI is missing even when Desktop Code works correctly. Desktop support is not guaranteed across every version or OS; rely on what your installed versions actually show.

### Kiro CLI

Check your Kiro version first — the correct harness depends on it:

```bash
kiro-cli --version
```

- **Kiro CLI v3 (2.24.1 or later):** use `aidlc config --harness kiro-ide` (this pins Kiro's v3 engine). Then open with `kiro-cli` and type `/aidlc`.
- **Kiro CLI legacy (2.6 up to before 2.24.1):** use `aidlc config --harness kiro`. Then open with `kiro-cli chat` and type `/aidlc`.

Do not use the legacy `kiro` harness with a v3 Kiro CLI: on the v3 engine the legacy hooks do not run, so `/aidlc` will not be recognized. If an existing `kiro` project needs to move to v3, run `aidlc config --harness kiro-ide` to switch it in place (see the [Kiro CLI guide](docs/guide/harnesses/kiro-cli.md)).

### Cursor

Install Cursor from its vendor first. Configure the harness in your project:

```bash
aidlc config --harness cursor
```

Open the configured project in Cursor (or run `agent`), then type `/aidlc` with your request and confirm the AI-DLC workflow responds.

## Start Your First Application

Keep your **application code in your own project repository** — not inside this framework repo. With the runtime installed, your project created, and your harness configured, open your tool in the project and invoke AI-DLC.

### Confirm AI-DLC is active (read this first)

Before building, confirm the official AI-DLC workflow actually started:

- The project was configured for the correct harness (`aidlc config --harness <tool>`).
- `aidlc doctor` reports **no blocking (`fail`) project errors**.
- Your coding tool recognizes `/aidlc` (Codex uses `$aidlc`; in Kiro IDE, pick **aidlc** in the agent picker first).
- You see an official AI-DLC **workflow or plan-selection** response.

If `/aidlc` is not recognized, go back to the configuration steps and your harness guide — do **not** continue in ordinary coding mode. Ordinary application generation or a single generic approval question is **not** proof that AI-DLC executed.

### Example walkthrough

Invoke AI-DLC with a request such as:

```text
/aidlc Build a simple Task Manager web application where users can add, edit, complete, and delete tasks. Save tasks locally in the browser.
```

What typically happens next:

1. You open the correctly configured project and invoke `/aidlc`.
2. AI-DLC presents its supported workflow / plan-selection options.
3. You choose a workflow that fits the work.
4. AI-DLC gathers the information it needs.
5. You review the requirements and the proposed design.
6. You respond to the applicable human approval gate(s).
7. AI-DLC continues with implementation and validation.
8. You review the finished application and the generated lifecycle artifacts (written under your project at `aidlc/spaces/<space>/intents/<YYMMDD>-<label>/`).

A plan-selection interface observed in Claude Desktop Code (AWS AI-DLC 2.11.0) offered options such as **Compose**, **Classic**, **MVP**, and **Feature**. These are examples only — the exact options, number of approval gates, and lifecycle stages vary by request, workflow, harness, and runtime version. Treat this as an illustrative example, not a guaranteed sequence. The full walkthrough is in [Your First Workflow](docs/guide/02-your-first-workflow.md).

## Collaborative AI-DLC — Working with a Team

- **Individual AI-DLC** — one engineer builds an application end to end on their own machine (everything above).
- **Collaborative AI-DLC** — multiple engineers work on the **same** application using the official AWS multi-engineer capabilities: shared work context, coordinated ownership of **units of work**, review, and controlled integration where supported.

Each engineer needs their own configured local environment (runtime + harness) and access to the shared application repository. Collaborative development is built on the framework's official multi-team / ensemble Construction — see [Construction and the Swarm](docs/harness-engineering/08-construction-and-swarm.md).

> ⚠️ **Pending validation.** Multi-engineer collaborative acceptance is **pending** until a real multi-engineer test is conducted. Until then, treat the linked official material as the reference.

## Troubleshooting and Support

Run `aidlc doctor` from the project root first — it names most issues and prints a fix.

| Symptom | What to check |
| --- | --- |
| `aidlc` command not found | Open a new terminal; on Windows reopen PowerShell. If still missing, apply the PATH instruction the installer printed. |
| Project not configured | Run `aidlc config --harness <tool>` from the application project root, then `aidlc doctor`. |
| Missing required hooks | Re-run `aidlc config` for your harness; confirm the Project section of `aidlc doctor` is healthy. |
| Project not trusted (Kiro) | On first run in a folder, choose **Trust Folder & Continue** only for a project you own or have reviewed. |
| Running from the wrong directory | `aidlc config` and `aidlc doctor` act on the current directory — confirm with `pwd` (macOS) or `Get-Location` (Windows). |
| `/aidlc` not recognized | Confirm the harness was configured and the tool was restarted; in Kiro IDE pick **aidlc** in the agent picker; Codex uses `$aidlc`. |
| Kiro version / harness mismatch | Check `kiro-cli --version`: v3 (≥ 2.24.1) uses `kiro-ide`; legacy (≥ 2.6) uses `kiro`. The legacy harness does not run on the v3 engine. |
| AI tool CLI unavailable | Install the tool from its vendor; it is separate from AI-DLC. |
| Claude Desktop Code vs. Chat | Use the **Code** workspace, not ordinary Chat; open the configured project directory. |
| Interrupted interactive setup | Re-run `aidlc config` from the project directory to resume. |
| Advisory warnings from `aidlc doctor` | Warnings are advisory; only a `fail` row blocks. New-project warnings about first-run hooks and uncommitted `aidlc/` changes are expected. |
| Windows PATH not updated | Close and reopen PowerShell after install; see the install guide's Windows PowerShell section. |
| Existing project configuration | Re-running `aidlc config` refreshes a configured project rather than starting over. |

For diagnostics and recovery, see [Troubleshooting](docs/guide/15-troubleshooting.md).

## Additional Documentation

- **Install & lifecycle (macOS / Linux / Windows):** [docs/guide/18-install-and-lifecycle.md](docs/guide/18-install-and-lifecycle.md)
- **Getting started:** [docs/guide/01-getting-started.md](docs/guide/01-getting-started.md)
- **Your first workflow:** [docs/guide/02-your-first-workflow.md](docs/guide/02-your-first-workflow.md)
- **Workflow profiles (Classic, Express, Feature, MVP, and more):** [docs/guide/workflow-profiles.md](docs/guide/workflow-profiles.md)
- **Harness guides (per tool):** [docs/guide/harnesses/](docs/guide/harnesses/) — [Kiro CLI](docs/guide/harnesses/kiro-cli.md), [Kiro IDE / v3](docs/guide/harnesses/kiro-ide.md), [Cursor](docs/guide/harnesses/cursor.md)
- **Collaborative / multi-team Construction:** [docs/harness-engineering/08-construction-and-swarm.md](docs/harness-engineering/08-construction-and-swarm.md)
- **Preserved official AWS README:** [README.aws-upstream.md](README.aws-upstream.md)
- **Full official docs:** [docs/](docs/) ([guide](docs/guide/), [reference](docs/reference/), [roadmap](docs/roadmap.md))

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
