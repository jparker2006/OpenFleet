# OpenWiz

> [!WARNING]
> **Design preview 0.0.0. Proposal only.**
> There is no installable implementation here: no package, CLI, web app, service, API, adapter, command, button, endpoint or working release. Everything in this repository is a proposal for review.

OpenWiz proposes a **coordination layer above the personal agents a person already uses**. A user could start with one personal agent and add more later. The agents would share memory kept in the user's own private repository. Cloud coding factories would sit downstream and do bounded software work when asked.

It is not one personal agent that happens to launch coding agents. The aim is for personal agents to work from the same memory and hand work to each other and to coding factories, without the user copying context between them or managing terminals by hand.

OpenWiz would be **CLI-first and agent-first**, with a **polished UI** as an equal goal. Users would drive it from a command line, through commands and buttons in their personal agent, or from the web.

- **The core, the CLI and all backend logic would be written in Rust.**
- **The controller** is the one Rust core that owns authoritative state, permissions, routing and coordination. Each project's **orchestrator** runs that project's factory workflow under the controller. It is not a second authority.
- **OpenWiz Web** would be a Next.js and TypeScript client of that same backend. It would be a live graph and control surface, where every button is one backend operation.
  - It would show what is running, blocked or done, loop and review progress, and policy decisions, linking to the real underlying artifacts.
  - It would keep only presentation state, such as which screen is open, and never authoritative product state.
- **Desktop packaging** is undecided.
- Everything would live in **one monorepo**: a Rust workspace for the core, the CLI and the service adapters, `apps/web` for the Next.js client, and shared contracts, tests and docs. The layout and dependency rules are in [docs/architecture.md](docs/architecture.md#5-proposed-repository-layout-and-dependency-direction).

None of these interfaces exists yet.

![OpenWiz proposed product architecture. Personal agents, including dot and Grok Bot, connect through the draft PACP protocol to the OpenWiz Controller: one CLI-first Rust core that owns state, permissions, routing and coordination. The controller links to shared memory in the user's private GitHub repo and to OpenWiz Web, a Next.js client that keeps presentation state only. Over an unverified route it sends work to per-project factories on CloudRoom. In each one, a project orchestrator works under the controller. It defines proof (claims and evidence) before build, then runs test and review, loops back through rework to test, and ends at a policy gate.](docs/assets/openwiz-architecture.png)

*Product architecture · 0.0.0. Everything shown is proposed. Dashed links are unverified. Editable source: [openwiz-architecture.svg](docs/assets/openwiz-architecture.svg).*

![OpenWiz planned monorepo modules. The thin clients crates/cli and apps/web, which keep presentation state only, call the same operations in crates/core, the controller. The web also uses generated types from contracts/. crates/core holds operations and rules and depends on contracts/. crates/adapters/*, one per external service, implement core interfaces, keep integration bookkeeping only, never own policy, and would talk to external services: CloudRoom, GitHub, personal agents via PACP and the memory repository. No OpenWiz adapter is built yet. tests/ check every surface. Desktop packaging comes later.](docs/assets/openwiz-modules.png)

*Planned monorepo modules · 0.0.0. Nothing is built. Dependencies point inward. Editable source: [openwiz-modules.svg](docs/assets/openwiz-modules.svg).*

Detailed diagrams: [docs/architecture.md](docs/architecture.md). Proposed commands and screens: [docs/interface.md](docs/interface.md).

## First pilot (proposed)

The first pilot is **dot → a controller on a user-owned PC → a CloudRoom coding factory**.

- The PC is the controller host chosen for this pilot. Users should not need an always-on machine of their own.
- **A proposed, untested alternative** is an event-driven controller hosted in CloudRoom, using its [automations](https://www.cloudroom.dev/docs/automations) and [subagents](https://www.cloudroom.dev/docs/subagents).
  - Each run would read its state from GitHub and the issue tracker and write it back.
  - This alternative assumes no stable inbound endpoint and no always-on server.
  - Starting work while the user's own computer is off has not been verified.
- The first personal agents to integrate are **dot** and **Grok Bot**. **Muse** and **Duet** are planned later. These are targets. None is a verified integration.

## What users would bring

- A **GitHub** account. GitHub is required at first.
- Their existing **issue tracker**. GitHub Issues is one option, not the only one intended.
- Their existing subscriptions, personal agents, agent harnesses and compute.

Managed hosting and selling compute are possible paid offerings for later. Neither exists, and neither is a launch requirement.

## What is in this preview

| File | What it is |
| --- | --- |
| [docs/overview.md](docs/overview.md) | The problem, the layers, interfaces, built-in plans, approval policy, terms and principles |
| [docs/interface.md](docs/interface.md) | The proposed command set, decision types and screens shared by the CLI, agent commands and OpenWiz Web |
| [docs/workflow.md](docs/workflow.md) | How the controller and the factory loop work: ownership, ticket states, rework, pause and resume, approval, dependent unlock, a worked example, and proof-driven development |
| [docs/architecture.md](docs/architecture.md) | Mermaid diagrams: today's manual workflow, the first pilot, the broader architecture, the release flow, and the planned repository layout with its dependency direction |
| [docs/pacp/v0.1.md](docs/pacp/v0.1.md) | **PACP v0.1** (Personal Agent Connection Protocol): a draft protocol proposal with synthetic examples and proposed conformance checks |
| [docs/decisions.md](docs/decisions.md) | Decisions made for this preview, and the ones still open |
| [CHANGELOG.md](CHANGELOG.md) | What changed in each version, including revisions of this preview |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to give feedback and contribute |
| [RELEASING.md](RELEASING.md) | Version lines, what a release means here, and how one is reviewed |
| [LICENSE](LICENSE) | Apache License 2.0 |

## Status words

| Label | Meaning |
| --- | --- |
| **Current** | What a person does by hand today. No OpenWiz software is involved. |
| **Unverified** | An integration that might be possible but has not been tested or proven. |
| **Proposed** | Design only. Nothing has been built. |
| **Future** | Planned for later, or deliberately out of scope for now. |

| Part | Status |
| --- | --- |
| Manual workflow: assistant → person → local CLI → cloud coding sessions | Current |
| Controller (one Rust core), CLI, agent commands and buttons | Proposed |
| Command set, decision types and screens ([docs/interface.md](docs/interface.md)) | Proposed |
| Controller and factory workflow ([docs/workflow.md](docs/workflow.md)) | Proposed |
| OpenWiz Web (Next.js client: live graph and control surface) | Proposed |
| Monorepo layout: Rust workspace (core, CLI, adapters), `apps/web`, shared contracts, tests and docs | Proposed |
| Desktop packaging | Undecided |
| 0.0.1 first job: read-only Rust inventory of real CloudRoom sessions ([docs/decisions.md](docs/decisions.md#planned-next-001-not-started)) | Planned, not started |
| Memory captured automatically in the user's private repository | Proposed. Detailed behaviour is left to a separate design session. |
| Built-in plans: Grill or Wayfinder → plan → user approval → launch | Proposed |
| Controller on a user-owned PC for the pilot | Proposed |
| Event-driven controller hosted in CloudRoom | Proposed, untested |
| Control route from the controller to CloudRoom | Unverified |
| OpenWiz adapters for CloudRoom, GitHub, personal agents and the memory repository ([access vs adapters](docs/architecture.md#external-services-access-versus-adapters)) | Proposed. None implemented |
| dot and Grok Bot integrations | Proposed targets, unverified |
| Muse and Duet integrations | Future (planned later) |
| Managed hosting and selling compute | Future (possible paid offerings, not implemented) |
| PACP v0.1 | Proposed (draft) |
| Business multiplayer (shared organisational memory, many owners) | Future |

## Separate things, separate versions

- **OpenWiz** is the product. This preview is version **0.0.0**. It contains documents only.
- **PACP** is a proposed protocol with its own version line, currently **v0.1 (draft)**. It is meant to stand apart from OpenWiz so that other systems could implement it.
- **Adapters** would connect particular personal agents to PACP. None exists. Each would have its own version.

These version numbers move independently, and none implies compatibility with another. A release that contains software states its real dependencies explicitly, for example which PACP version it implements. See [RELEASING.md](RELEASING.md).

## License

[Apache License 2.0](LICENSE).
