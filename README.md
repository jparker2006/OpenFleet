# OpenFleet

> [!WARNING]
> **Design preview 0.0.0. Proposal only.**
> There is no installable implementation here: no package, CLI, web app, service, API, adapter, command, button, endpoint or working release. Everything in this repository is a proposal for review.

OpenFleet proposes a **coordination layer above the personal agents a person already uses**. A user could start with one personal agent and add more later. The agents would share memory kept in the user's own private repository. Cloud coding factories would sit downstream and do bounded software work when asked.

It is not one personal agent that happens to launch coding agents. The aim is for personal agents to work from the same memory and hand work to each other and to coding factories, without the user copying context between them or managing terminals by hand.

OpenFleet would be **CLI-first and agent-first**: users would drive it from a command line or through commands and buttons in their personal agent. **OpenFleet Web** would be a live graph and control surface over the same backend and operations. It would show what is running, blocked or done, loop and review progress, and policy decisions, linking to the real underlying artifacts. It would not be a separate source of state. None of these interfaces exists yet.

## First pilot (proposed)

The first pilot is **dot → a controller on a user-owned PC → a CloudRoom coding factory**.

- The PC is the controller host chosen for this pilot. Users should not need an always-on machine of their own.
- **A proposed, untested alternative** is an event-driven controller hosted in CloudRoom, using its [automations](https://www.cloudroom.dev/docs/automations) and [subagents](https://www.cloudroom.dev/docs/subagents). Each run would read its state from GitHub and the issue tracker and write it back. This alternative assumes no stable inbound endpoint and no always-on server. Starting work while the user's own computer is off has not been verified.
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
| [docs/architecture.md](docs/architecture.md) | Four Mermaid diagrams: today's manual workflow, the first pilot, the broader architecture, and the release flow |
| [docs/pacp/v0.1.md](docs/pacp/v0.1.md) | **PACP v0.1** (Personal Agent Connection Protocol): a draft protocol proposal with synthetic examples and proposed conformance checks |
| [docs/decisions.md](docs/decisions.md) | Decisions made for this preview, and the ones still open |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to give feedback and contribute |
| [RELEASING.md](RELEASING.md) | What a release means here and how one is reviewed |
| [LICENSE](LICENSE) | Apache License 2.0 |

## Status words

| Label | Meaning |
| --- | --- |
| **Current** | What a person does by hand today. No OpenFleet software is involved. |
| **Unverified** | An integration that might be possible but has not been tested or proven. |
| **Proposed** | Design only. Nothing has been built. |
| **Future** | Planned for later, or deliberately out of scope for now. |

| Part | Status |
| --- | --- |
| Manual workflow: assistant → person → local CLI → cloud coding sessions | Current |
| Coordination layer, CLI, agent commands and buttons | Proposed |
| OpenFleet Web (live graph and control surface) | Proposed |
| Memory captured automatically in the user's private repository | Proposed. Detailed behaviour is left to a separate design session. |
| Built-in plans: Grill or Wayfinder → plan → user approval → launch | Proposed |
| Controller on a user-owned PC for the pilot | Proposed |
| Event-driven controller hosted in CloudRoom | Proposed, untested |
| Control route from the controller to CloudRoom | Unverified |
| dot and Grok Bot integrations | Proposed targets, unverified |
| Muse and Duet integrations | Future (planned later) |
| Managed hosting and selling compute | Future (possible paid offerings, not implemented) |
| PACP v0.1 | Proposed (draft) |
| Business multiplayer (shared organisational memory, many owners) | Future |

## Separate things, separate versions

- **OpenFleet** is the product proposal. This preview is version **0.0.0**.
- **PACP** is a proposed protocol with its own version line, currently **v0.1 (draft)**. It is meant to stand apart from OpenFleet so that other systems could implement it.
- **Adapters** would connect particular personal agents to PACP. None exists.

## License

[Apache License 2.0](LICENSE).
