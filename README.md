# AICC AI-DLC

**AICC AI-DLC** is an AICC-tailored distribution of the official
**AWS AI-DLC (AI-Driven Development Life Cycle)** framework. It turns AI coding
assistants into a structured, verifiable, governed software-delivery lifecycle,
and packages that experience for two ways of working at AICC:

1. **Individual AI-DLC** — a single engineer installs the framework on their own
   machine and builds applications using the complete official AWS AI-DLC
   lifecycle.
2. **Collaborative AI-DLC** — multiple engineers collaborate on the same
   application using the official AWS collaborative capabilities.

This repository is AICC's **development and testing** workspace for that
distribution. It is a clean fork of the official AWS source with full upstream
history, kept synchronizable with AWS. **We develop and validate here first;
nothing is promoted to an operational environment from this repository.**

> **Based on official AWS work.** The engine, agents, harness integrations,
> documentation, scripts, and tests in this repository come from the official
> AWS project **[awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows)**
> and are used under its **MIT-0** license. See
> [Upstream attribution & license](#upstream-attribution--license).

---

## Status at a glance

| Capability | Status |
|------------|--------|
| Official AWS AI-DLC engine, agents, harnesses, docs, scripts, tests | ✅ **Present & unmodified** (vendored from upstream with full history) |
| Individual AI-DLC (install + run the lifecycle on one machine) | 🟡 **Supported by upstream — pending local validation on this setup** |
| Collaborative AI-DLC (multi-engineer, official AWS collaboration) | 🟡 **Supported by upstream — pending validation** |
| AICC-specific customizations to the engine | ⚪ **Not started** (intentionally — the engine is unchanged for now) |
| Operational / production rollout | ⚪ **Planned — not begun** |

Legend: ✅ verified in this repo · 🟡 available upstream, not yet validated by us · ⚪ planned / not started.

**What is genuinely working right now:** the repository contains the complete,
unmodified official AWS AI-DLC source and history, with Git remotes configured
so we can track AWS upstream and push only to our own fork. **What still requires
testing:** actually installing and running both the individual and collaborative
lifecycles end-to-end on this environment (see
[Testing the two workflows](#testing-the-two-workflows)). We make no claim that
either workflow has been validated here yet.

## What AICC AI-DLC is (and isn't)

- **It is** a faithful distribution of AWS AI-DLC, plus AICC-oriented onboarding
  and (later) AICC-specific configuration, validated in a personal repo before
  any operational use.
- **It is not** a rewrite of the AI-DLC engine, and it does **not** yet add
  custom collaboration logic. The official AWS collaborative capabilities are
  used as-is; building anything custom on top is out of scope until the
  baseline is validated.

## Individual AI-DLC

One engineer, one machine, the full lifecycle. The official framework installs a
native `aidlc` command and the runtime for the engineer's chosen harness; the
engineer configures a project and runs the governed lifecycle (requirements →
design → approval → code → tests → evidence) locally.

Setup and usage are documented in the official guide (preserved in this repo):
see [`README.aws-upstream.md`](README.aws-upstream.md) and
[`docs/guide/`](docs/guide/).

> Status: supported by the upstream framework; **not yet validated on this
> setup.** Treat the steps as the official instructions pending our own run.

## Collaborative AI-DLC

Multiple engineers working on the same application using the **official AWS
collaborative capabilities** of AI-DLC. We will exercise these as provided by
upstream — no AICC-custom collaboration is implemented in this repository.

> Status: supported by the upstream framework; **not yet validated on this
> setup.**

## Supported development tools (harnesses)

The official core is harness-neutral and ships integrations for all of these
(see [`harness/`](harness/)):

| Harness | Directory |
|---------|-----------|
| Claude Code | `harness/claude` |
| Kiro CLI | `harness/kiro` |
| Kiro IDE | `harness/kiro-ide` |
| Codex CLI | `harness/codex` |
| Cursor | `harness/cursor` |
| opencode | `harness/opencode` |
| GitHub Copilot | `harness/copilot` |

## Installation & onboarding goals

The near-term goal is a **clean, upstream-trackable baseline** that an AICC
engineer can install and run with minimal friction. Concretely:

- Keep this repository a faithful mirror of AWS AI-DLC with full history.
- Validate the individual lifecycle end-to-end on a Mac dev environment.
- Validate the collaborative lifecycle using the official AWS capabilities.
- Only then layer AICC-specific onboarding/configuration, and only then plan an
  operational rollout.

For the authoritative install and configuration steps, use the official
instructions in [`README.aws-upstream.md`](README.aws-upstream.md).

## Repository structure (from upstream)

```
AICC_AI-DLC/
├── core/            # the harness-neutral AI-DLC engine
│   ├── agents/      scopes/     sensors/    skills/
│   ├── aidlc-common/ hooks/     knowledge/  memory/
│   ├── templates/   tools/
├── harness/         # per-tool integrations (claude, kiro, kiro-ide, codex, cursor, opencode, copilot)
├── plugins/         # optional plugins (e.g. test-pro)
├── docs/            # official documentation (guide, reference, harness-engineering, roadmap)
├── scripts/         # build / lint / release / tooling scripts
├── tests/           # the official test suite
├── .github/         # upstream CI workflows
├── README.aws-upstream.md   # the ORIGINAL official AWS README (preserved verbatim)
└── README.md        # this AICC overview
```

## Current development & testing status

- **Environment:** local Mac development repository only.
- **Source integrity:** complete official AWS source vendored with full commit
  history; the AI-DLC engine is **unmodified**.
- **Remotes:** `origin` → our fork; `upstream` → AWS (fetch-only; pushing to
  upstream is disabled).
- **Validation:** both the individual and collaborative lifecycles **still
  require testing** on this setup before any claim of working functionality.

## Planned operational rollout

After both workflows are validated in this personal repository, the solution
will be migrated to AICC's operational environment in a separate, reviewed step.
**No deployment or operational migration happens from this repository.**

## Testing the two workflows

Recommended next steps (not yet executed here):

1. **Individual:** install `aidlc` per the official README, run
   `aidlc config --harness <tool>` and `aidlc doctor` in a scratch project, then
   drive one lifecycle to completion and confirm the governance gates and
   evidence output.
2. **Collaborative:** follow the official collaborative capability docs with two
   engineers (or two clones) against a shared application, and confirm the
   collaboration behaves as upstream specifies.
3. Run the upstream test suite (`tests/`) and CI locally to establish a green
   baseline before any AICC changes.

## Upstream attribution & license

- **Upstream project:** [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows)
  (official AWS Labs AI-DLC).
- **License:** **MIT-0** (MIT No Attribution) — see [`LICENSE`](LICENSE).
  Copyright Amazon.com, Inc. or its affiliates.
- The official AWS README and documentation are preserved in this repository:
  [`README.aws-upstream.md`](README.aws-upstream.md), [`docs/`](docs/),
  [`AGENTS.md`](AGENTS.md), [`CONTRIBUTING.md`](CONTRIBUTING.md),
  [`DEVELOPERS.md`](DEVELOPERS.md), and [`CHANGELOG.md`](CHANGELOG.md).

AICC AI-DLC adds AICC-specific packaging and (later) configuration on top of
this upstream work; it does not claim ownership of the official AWS framework.
