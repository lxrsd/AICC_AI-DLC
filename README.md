# AICC AI-DLC

AICC AI-DLC gives engineers one standardized, AI-powered way to plan, design, build, test, and deliver software. It runs on the **official [AWS AI-DLC](https://github.com/awslabs/aidlc-workflows) framework** — AICC does not build its own runtime or installer. You use a terminal only to set a project up; all development then happens inside your preferred AI coding application.

## Quick Start — Three Steps

### Step 1 — Create Your Project Folder

Create a local folder for your application. The name `my-app` and the Desktop location are **examples** — use any project name and location you like (use the same folder for the rest of setup).

macOS (Terminal):

```bash
mkdir -p "$HOME/Desktop/my-app" && cd "$HOME/Desktop/my-app"
```

Windows (PowerShell):

```powershell
$proj = Join-Path ([Environment]::GetFolderPath('Desktop')) 'my-app'; New-Item -ItemType Directory -Force -Path $proj | Out-Null; Set-Location $proj
```

### Step 2 — Install & Configure the AWS AI-DLC Runtime

Install the **official AWS runtime once per computer** (review remote installer scripts before running).

macOS (Terminal):

```bash
curl -fsSL https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.sh | sh
```

Windows (PowerShell, normal non-admin window):

```powershell
irm https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.ps1 | iex
```

Then **configure a harness inside your project folder** — this is required, and the exact command depends on which coding application you use. Run **one** command for your application (these are examples for different supported harnesses, not a list to run in sequence):

| Your coding application | Configure command (run in the project folder) |
| --- | --- |
| Claude Code | `aidlc config --harness claude` |
| Kiro — current (v3) | `aidlc config --harness kiro-ide` |
| Kiro — legacy | `aidlc config --harness kiro` |
| Cursor | `aidlc config --harness cursor` |

AWS also supports additional harnesses (Codex CLI, GitHub Copilot, opencode) — see the [harness guides](docs/guide/harnesses/README.md). You configure only the one harness for the application you use. Runtime install is once per computer; harness configuration is once per project.

### Step 3 — Open Your AI Coding Application & Build

Setup is done — you can close the terminal. Now, inside your coding application:

1. Open your supported AI coding desktop application.
2. Open the project folder you just configured.
3. Invoke the AI-DLC entry point — normally `/aidlc` for the documented desktop integrations (Codex uses `$aidlc`; in Kiro IDE, pick **aidlc** in the chat agent picker first).
4. Select a supported AWS AI-DLC workflow and build through its stages and human approval gates.

All development happens **inside the coding application**, not in Terminal or PowerShell. If `/aidlc` is not recognized, see [Troubleshooting](#troubleshooting). Examples here are illustrative and may differ by operating system, harness, and installed version.

---

## Windows — Getting Started with AICC AI-DLC

Follow these steps to set up your development environment, connect your application to Huron's GitHub organization, and start building with your preferred AI coding tool.

**What you'll do:**

1. **Create your development workspace** — Set up a local Windows folder outside OneDrive and SharePoint.
2. **Install AWS AI-DLC** — Install the framework once on your computer.
3. **Create your GitHub repository** — Create and connect an application repository under HCG-AICC.
4. **Configure AI-DLC** — Select your preferred AI coding harness for the project.
5. **Verify your setup** — Confirm everything is properly configured.
6. **Start building** — Develop your application using the official AWS AI-DLC lifecycle.
7. **Save your work** — Commit and push your application to Huron GitHub.

Use **Windows PowerShell** for every command below. GitHub stores your application source; your desktop AI coding tool works against the local clone on your machine. You install AWS AI-DLC once per computer, but you configure it separately for each new project.

### Step 1 — Create Your Local Development Workspace

Huron-managed Windows desktops are often redirected to OneDrive. Your application development files must stay on the local Windows drive, so this step creates a local `dev` folder and (if your Desktop is redirected) adds a shortcut to it on the visible Desktop. You only do this once.

```powershell
$Dev = Join-Path $HOME 'Desktop\dev'; New-Item -ItemType Directory -Force -Path $Dev | Out-Null; $Desktop = [Environment]::GetFolderPath('Desktop'); if ($Desktop.TrimEnd('\') -ine (Split-Path $Dev -Parent).TrimEnd('\')) { $Shell = New-Object -ComObject WScript.Shell; $Link = $Shell.CreateShortcut((Join-Path $Desktop 'dev.lnk')); $Link.TargetPath = $Dev; $Link.WorkingDirectory = $Dev; $Link.Save() }; Set-Location -LiteralPath $Dev
```

This creates a local `dev` folder, makes it reachable from the visible Desktop, keeps application files out of OneDrive/SharePoint, and opens the folder in your PowerShell session. It works regardless of your Windows username.

### Step 2 — Install AWS AI-DLC

Installation is required **only once per computer**, not for every application. Install the official AWS runtime:

```powershell
irm https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.ps1 | iex
```

Confirm it is available:

```powershell
aidlc --version
```

If a version number is displayed, AI-DLC is installed — skip the install command and continue. On Huron-managed endpoints the installer may be terminated or blocked by corporate certificate-verification policies; if that happens, work with your endpoint/IT support rather than disabling SSL or certificate checks.

### Step 3 — Create Your HCG-AICC GitHub Repository

GitHub is the central source-control platform for Huron AICC applications; you develop against a local clone of your GitHub repository. This step needs Git, the GitHub CLI (`gh`), an authenticated GitHub login, and permission to create repositories in the HCG-AICC organization.

Create a new private repository (using `my-app` as the example name) and clone it into your `dev` folder:

```powershell
gh repo create HCG-AICC/my-app --private --add-readme --clone
```

This creates a private repository in HCG-AICC, initializes it with a README, clones it into your current `dev` folder, and connects the local copy to GitHub.

If the repository already exists, clone it instead:

```powershell
gh repo clone HCG-AICC/my-app
```

Then move into the application folder:

```powershell
cd my-app
```

### Step 4 — Configure AWS AI-DLC

Run this **from inside the application folder** and select your preferred harness from the official interactive setup:

```powershell
aidlc config
```

Choose one of the seven supported coding applications when prompted:

- Claude Code
- Codex CLI
- GitHub Copilot
- Cursor
- Kiro CLI
- Kiro IDE
- opencode

Select the application you actually use; do not simply accept a different preselected default. Remember: AI-DLC is installed once per machine, but configured per project.

### Step 5 — Verify Your Setup

```powershell
aidlc doctor
```

This checks the runtime, framework, and project configuration. Resolve any reported failures before you start developing. (Warnings are advisory; only a `fail` row blocks — see [Verify Your Setup](#verify-your-setup-optional-diagnostics).)

### Step 6 — Start Building

Open your `my-app` project folder in the supported AI coding application you configured, then start the official AWS AI-DLC workflow through that application's documented entry point (for most desktop integrations this is `/aidlc`; Codex uses `$aidlc`). Choose a workflow and build through the lifecycle's stages and human approval gates. Per-tool entry points and prerequisites are in the [harness guides](docs/guide/harnesses/README.md).

### Step 7 — Save Your Work to GitHub

After developing and testing, commit and push your application:

```powershell
git status
```

```powershell
git add .
```

```powershell
git commit -m "feat: implement AI-DLC application"
```

```powershell
git push origin main
```

These assume your current branch is `main` and that HCG-AICC allows direct pushes to it. Follow your organization's branch-protection and review policies where they apply.

---

## Additional Setup & Reference (Optional / When Needed)

Everything below is supporting reference. The three steps above are the full happy path; use these sections only when a situation calls for them.

### Different Harnesses — choose the one matching your coding application

The harness configuration in Step 2 is **required**, but the specific command depends on your tool — there is no single universal command.

- **Claude Code** — `aidlc config --harness claude`. Open your project in the Claude desktop app's **Code** workspace (not ordinary Chat) and type `/aidlc`. Desktop-only engineers do **not** need the standalone `claude` CLI when Desktop Code runs the configured harness. The AI-DLC plan-selection interface has been observed in Claude Desktop Code with AWS AI-DLC 2.11.0.
- **Kiro** — current Kiro (v3) uses `aidlc config --harness kiro-ide`; legacy Kiro uses `aidlc config --harness kiro`. The legacy hooks do not run on Kiro's v3 engine, so the wrong row means `/aidlc` is not recognized. Exact versions, how to check yours, and how to switch an existing project are in the [Kiro guide](docs/guide/harnesses/kiro-cli.md).
- **Cursor** — `aidlc config --harness cursor`, then open the project folder in Cursor and type `/aidlc`. See the [Cursor guide](docs/guide/harnesses/cursor.md).

Not every AI coding tool provides an identical desktop interface or integration surface; rely on what your installed versions actually show. Full per-tool detail: [harness guides](docs/guide/harnesses/README.md).

### Verify Your Setup (optional diagnostics)

Check the machine-level runtime:

```bash
aidlc version
```

Check the project configuration **from the project folder**:

```bash
aidlc doctor
```

`aidlc doctor` reports **Machine**, **Project**, and **Framework integrity** sections and ends with a count like `0 problems, N warnings.` Warnings are advisory, not failures — on a new project, a few (first-run hooks, uncommitted `aidlc/` changes) are expected. A `fail` row is a real problem and prints its own fix. On Windows, if `aidlc` is not recognized, open a new PowerShell window. Git is optional for setup but recommended; initialize it in your project with `git init` when you want version control.

### Advanced Configuration

Running `aidlc config` without `--harness` starts an interactive wizard (Harness → Model provider → Model effort preset → Plugins → MCP servers → settings layer); pressing Enter accepts the shown default at each step, and recommended defaults keep your current model provider, enable MCP servers and plugins, and record a balanced effort preset. Model providers, MCP servers, plugins, and customization are covered in [Getting Started](docs/guide/01-getting-started.md) and [Install and Lifecycle](docs/guide/18-install-and-lifecycle.md). These are optional preferences — the Step 2 harness configuration is the only required configuration.

### Troubleshooting

Run `aidlc doctor` from the project folder first; it names most issues and prints a fix.

| Symptom | What to check |
| --- | --- |
| `aidlc` command not found | Open a new terminal (reopen PowerShell on Windows); apply the PATH instruction the installer printed. |
| `/aidlc` not recognized | Confirm the harness was configured in this folder and the application was reopened; in Kiro IDE pick **aidlc** in the agent picker; Codex uses `$aidlc`. |
| Kiro version / harness mismatch | Check `kiro-cli --version`: current (v3) uses `kiro-ide`, legacy uses `kiro`; the legacy harness does not run on the v3 engine. |
| Project not configured | Run the Step 2 harness command from the project folder, then `aidlc doctor`. |
| Wrong directory | Harness config and `aidlc doctor` act on the current folder — confirm with `pwd` (macOS) or `Get-Location` (Windows). |
| Claude Desktop Code vs. Chat | Use the **Code** workspace, not ordinary Chat; open the configured project folder. |
| Advisory `aidlc doctor` warnings | Warnings are advisory; only a `fail` row blocks. |

More: [Troubleshooting guide](docs/guide/15-troubleshooting.md).

### Collaborative Development

- **Individual AI-DLC** — one engineer builds an application end to end (the three steps above).
- **Collaborative AI-DLC** — multiple engineers work on the **same** application using the official AWS multi-engineer capabilities: shared work context, coordinated ownership of **units of work**, review, and controlled integration where supported. Each engineer needs their own configured environment (runtime + harness) and access to the shared repository. See [Construction and the Swarm](docs/harness-engineering/08-construction-and-swarm.md).

> ⚠️ **Pending validation.** Multi-engineer collaborative acceptance is **pending** until a real multi-engineer test is conducted. Treat the linked official material as the reference until then.

### Additional Official AWS Documentation

- **Install & lifecycle:** [docs/guide/18-install-and-lifecycle.md](docs/guide/18-install-and-lifecycle.md)
- **Getting started:** [docs/guide/01-getting-started.md](docs/guide/01-getting-started.md)
- **Your first workflow:** [docs/guide/02-your-first-workflow.md](docs/guide/02-your-first-workflow.md)
- **Workflow profiles:** [docs/guide/workflow-profiles.md](docs/guide/workflow-profiles.md)
- **Harness guides (per tool):** [docs/guide/harnesses/](docs/guide/harnesses/)
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
Code, Kiro CLI, and Cursor**, but you may use any harness AWS supports.

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
