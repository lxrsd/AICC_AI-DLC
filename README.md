# AICC AI-DLC

**AICC AI-DLC** is an engineering solution built on the official
**AWS AI-DLC (AI-Driven Development Life Cycle)** framework. It gives engineers
**one standardized, AI-powered development solution** that turns an AI coding
assistant into a structured, verifiable, governed software-delivery lifecycle —
and packages that experience for two ways of working at AICC.

> **Based on official AWS work.** The lifecycle engine, agents, harness
> integrations, approval mechanisms, documentation, scripts, and tests in this
> repository come from the official AWS project
> **[awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows)** and
> are used under its **MIT-0** license. We do not recreate functionality AWS
> already maintains. See [AWS upstream attribution & license](#aws-upstream-attribution--license).

This repository is AICC's **development and testing** workspace. We build and
validate here first; **nothing is promoted to an operational environment from
this repository.**

---

## 1. Overview & mission

Our goal is a single, standardized AI-DLC experience for AICC engineers,
delivered in two modes that both run on the **official AWS framework** rather
than any custom-built lifecycle engine:

- **Individual AI-DLC** — one engineer installs AICC AI-DLC on their own
  computer and uses a supported AI development tool to plan, design, develop,
  test, and deliver an application through the complete AWS AI-DLC methodology.
- **Collaborative AI-DLC** — multiple engineers work together on the same
  application using shared context, coordinated work ownership, approval
  workflows, and controlled integration, leveraging AWS's official
  collaborative capabilities.

Both modes use the same official engine, so an engineer who learns one is
immediately productive in the other.

## 2. Individual and Collaborative AI-DLC

### Individual AI-DLC
A self-contained experience for a single engineer and a single machine. The
official framework installs a native `aidlc` command plus the runtime for the
engineer's chosen harness; the engineer configures a project and runs the
governed lifecycle (requirements → design → approval → code → tests → evidence)
locally, with human approval gates enforced at each decision point.

### Collaborative AI-DLC
A multi-engineer experience on a shared application, using AWS's official
collaborative capabilities: shared context, coordinated ownership of work units,
approval workflows, and controlled integration. AICC uses these capabilities as
provided by upstream — **no AICC-custom collaboration service is built in this
repository.**

> These are **target experiences**. AICC has **not yet validated** either mode
> end-to-end on this environment (see [Current development status](#6-current-development-status)).

## 3. How engineers will use the solution

The following are the **intended** end-to-end experiences, described in plain
language. They reflect the official framework's documented behavior; they are
not a claim that AICC has already run them here. For authoritative steps, follow
the official guide — [`docs/guide/01-getting-started.md`](docs/guide/01-getting-started.md)
and [`docs/guide/18-install-and-lifecycle.md`](docs/guide/18-install-and-lifecycle.md).

**Individual workflow**
1. Install AICC AI-DLC on a supported computer.
2. Open or create an application project.
3. Configure a supported AI development tool (harness).
4. Start the official AI-DLC workflow.
5. Complete requirements, design, approvals, implementation, and testing.
6. Review the evidence and deliver the application.

**Collaborative workflow**
1. Install and configure the framework.
2. Join a shared application project.
3. Follow the official collaborative onboarding and coordination process.
4. Work on an assigned or claimed unit of work.
5. Complete the applicable AI-DLC workflow for that unit.
6. Submit the work for review and integration.
7. Contribute to the completed shared application.

## 4. Architecture & official AWS foundation

- The **core lifecycle engine comes from AWS** and lives under
  [`core/`](core/) (agents, scopes, sensors, skills, hooks, knowledge,
  templates, tools).
- AWS provides the **agents, harness integrations, workflows, approval
  mechanisms, and associated functionality**. We preserve them.
- We **preserve the official AWS implementation and full Git history**; the
  engine is **unmodified** in this repository.
- AICC will add **only** necessary onboarding, documentation, configuration, and
  improvements — not a reimplementation of what AWS maintains.
- The repository **tracks official AWS upstream** via a fetch-only `upstream`
  remote so we can pull in AWS changes over time.

### Built-in Multi-Team Construction vs. the Collaborative AI-DLC sample

These are two different things, and we treat them differently:

- **Multi-Team / ensemble Construction (built in).** The official engine already
  ships multi-engineer Construction: work is decomposed into **Units of work**
  and **Bolts**, executed with ensemble topologies (collaborator, pipeline, mob,
  hub-and-spoke), git **worktree** isolation per Unit, and enforced reviewer
  receipts. This capability is present in [`core/`](core/) and documented in
  [`docs/harness-engineering/08-construction-and-swarm.md`](docs/harness-engineering/08-construction-and-swarm.md).
  This is what our **Collaborative AI-DLC** experience is built on.
- **The separate AWS Collaborative AI-DLC sample application.** AWS also
  publishes a *separate sample application* that demonstrates collaboration. That
  sample is **not vendored or integrated** into this repository, and we make **no
  claim** that it has been integrated here. If we reference it later, it will be
  as an external example, kept distinct from the built-in capability above.

## 5. Supported development tools (harnesses)

The official core is harness-neutral and ships integrations for all of the
following (see [`harness/`](harness/)):

| Harness | Directory |
|---------|-----------|
| Claude Code | [`harness/claude`](harness/claude) |
| Kiro CLI | [`harness/kiro`](harness/kiro) |
| Kiro IDE | [`harness/kiro-ide`](harness/kiro-ide) |
| Codex CLI | [`harness/codex`](harness/codex) |
| Cursor | [`harness/cursor`](harness/cursor) |
| opencode | [`harness/opencode`](harness/opencode) |
| GitHub Copilot | [`harness/copilot`](harness/copilot) |

## 6. Current development status

The table distinguishes three things: **working** (verified in this repo),
**upstream-documented** (available in the official framework but not yet
validated by AICC), and **AICC-tested** (validated by us — currently none of the
lifecycle flows).

| Item | Status |
|------|--------|
| Official AWS source — downloaded & preserved | ✅ **Working** — complete, unmodified, vendored in this repo |
| Git history — preserved | ✅ **Working** — full upstream history retained |
| AICC development repository — created & pushed | ✅ **Working** — this repo, tracking AWS upstream |
| Official engine, agents, harnesses, approval mechanisms | 📘 **Upstream-documented** — shipped by AWS; not yet exercised here |
| Individual lifecycle — end-to-end on Mac | 🟡 **Pending AICC validation** |
| Collaborative lifecycle — multi-engineer | 🟡 **Pending AICC validation** |
| Simplified AICC engineer installation | ⚪ **Planned — not implemented** |
| Operational migration | ⚪ **Not started** |

Legend: ✅ working & verified here · 📘 available/ documented upstream, not AICC-tested · 🟡 pending AICC validation · ⚪ planned / not started.

**No lifecycle flow has been AICC-validated yet.** Where a capability is marked
upstream-documented, it means AWS ships and documents it — not that we have run
it on this setup.

## 7. Installation & onboarding guidance

The near-term goal is a **clean, upstream-trackable baseline** that an AICC
engineer can install and run with minimal friction. We intend to:

- Keep this repository a faithful mirror of AWS AI-DLC with full history.
- Validate the individual lifecycle end-to-end on a Mac dev environment.
- Validate the collaborative lifecycle using the official AWS capabilities.
- Only then add AICC-specific onboarding/configuration and plan rollout.

For the **authoritative** install and configuration steps, use the official
instructions — preserved here as
[`README.aws-upstream.md`](README.aws-upstream.md) — and the official guide:

- Getting started — [`docs/guide/01-getting-started.md`](docs/guide/01-getting-started.md)
- Your first workflow — [`docs/guide/02-your-first-workflow.md`](docs/guide/02-your-first-workflow.md)
- Install & lifecycle — [`docs/guide/18-install-and-lifecycle.md`](docs/guide/18-install-and-lifecycle.md)
- Onboarding — [`docs/guide/onboarding.md`](docs/guide/onboarding.md)

> We intentionally do **not** restate CLI commands here; the official README and
> guide are the single source of truth and track the installed version.

## 8. Development & validation roadmap

1. **Baseline (done).** Vendor the official source with full history, configure
   remotes (push to our fork only), and preserve AWS docs/attribution.
2. **Green baseline (next).** Run the upstream test suite ([`tests/`](tests/))
   and CI locally to confirm the untouched source passes on this environment.
3. **Individual validation.** Install the runtime, run one real application
   through the full lifecycle on a Mac, and verify approval gates and evidence.
4. **Collaborative validation.** Exercise the official multi-engineer
   Construction capability with two or more engineers on a shared application.
5. **AICC onboarding layer.** Add only the necessary onboarding, documentation,
   and configuration — no engine changes.
6. **Operational planning.** Only after the above, plan a reviewed migration to
   AICC's operational environment (separate from this repository).

## 9. Definition of success

These are the acceptance criteria. **None is passed yet.**

**Individual AI-DLC is complete when:**
- A new engineer can follow documented installation instructions on a supported machine.
- The official AI-DLC runtime works with the selected harness.
- A real application completes the applicable lifecycle.
- Human approval gates and lifecycle artifacts are verified.
- The engineer can complete the GitHub delivery process.

**Collaborative AI-DLC is complete when:**
- Two or more engineers can participate in the same application project.
- Team members can coordinate and own separate units of work.
- Shared context and dependencies are handled correctly.
- Engineers can complete their work using supported AI tools.
- Changes pass review and controlled integration.
- The team delivers one working application.
- The process is repeatable and documented.

## 10. Repository structure

```
AICC_AI-DLC/
├── core/            # the harness-neutral AI-DLC engine (from AWS, unmodified)
│   ├── agents/      scopes/     sensors/    skills/
│   ├── aidlc-common/ hooks/     knowledge/  memory/
│   ├── templates/   tools/
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

## 11. AWS upstream attribution & license

- **Upstream project:** [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows)
  (official AWS Labs AI-DLC).
- **License:** **MIT-0** (MIT No Attribution) — see [`LICENSE`](LICENSE).
  Copyright Amazon.com, Inc. or its affiliates.
- **Official documentation is preserved in this repository:**
  [`README.aws-upstream.md`](README.aws-upstream.md), [`docs/`](docs/)
  ([guide](docs/guide/), [reference](docs/reference/),
  [harness-engineering](docs/harness-engineering/),
  [roadmap](docs/roadmap.md)), [`AGENTS.md`](AGENTS.md),
  [`CONTRIBUTING.md`](CONTRIBUTING.md), [`DEVELOPERS.md`](DEVELOPERS.md), and
  [`CHANGELOG.md`](CHANGELOG.md).

AICC AI-DLC adds AICC-specific packaging and (later) configuration on top of
this upstream work; it does not claim ownership of the official AWS framework.
