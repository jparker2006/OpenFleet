# Decisions

## Decided for this preview (2026-10-22)

These are design decisions. None of them is a built feature or a verified integration.

| Decision | Note |
| --- | --- |
| License: Apache License 2.0 | See [LICENSE](../LICENSE) |
| OpenFleet is CLI-first and agent-first | Commands and buttons in the personal agent; none exists yet |
| OpenFleet Web is a live graph and control surface over the same backend and operations | It is not a separate state authority |
| A user can start with one personal agent and add more later | |
| First pilot: dot → a controller on a user-owned PC → a CloudRoom coding factory | The PC is chosen for this pilot. Users should not need an always-on machine of their own. |
| Event-driven controller hosting in CloudRoom is a proposed alternative | Untested. No stable inbound endpoint, always-on server or verified launch with the user's computer off is assumed. |
| The first personal agents to integrate are dot and Grok Bot. Muse and Duet are planned later. | Targets, not verified integrations |
| GitHub is required at first | GitHub Issues is not the only intended issue tracker |
| Users bring their existing issue trackers, subscriptions, agents, harnesses and compute | |
| Memory is to be captured automatically into the user's private repository, with ongoing domain onboarding | The detailed behaviour is open (below) |
| The built-in plans run: Grill or Wayfinder → a plan (a version, spec or tickets) → the user approves the plan → the factory launches | Plan approval covers that plan only, not every action |
| Merges and other checkpoints are user-configurable policy | Each gate either requests approval or proceeds only under an applicable prior authorization |
| User control is bounded by provider permissions, platform restrictions and applicable safety requirements | |
| 0.0.0 stays proposal-only, with no installable implementation | |

## Open

| # | Decision | Status | What depends on it |
| --- | --- | --- | --- |
| 1 | **Managed hosting and selling compute** | Possible paid offerings. Not implemented, and not launch requirements. | Business model; support burden |
| 2 | **Memory behaviour**: consent, conflict handling, encryption and retention | Left to a separate memory design session | Memory parts of PACP; the privacy model |
| 3 | **Controller hosting** beyond the pilot | Open. Event-driven CloudRoom hosting is a proposed, untested alternative. | Availability; trust boundary; cost |
| 4 | **Which issue trackers** besides GitHub Issues | Open | Tracker adapters |
| 5 | **Which controller commands and buttons**, and how each agent would show them | Open. Depends on each agent's supported interface. | Integration targets |
| 6 | **PACP transport binding**: HTTP plus JSON, a profile of an existing protocol, or several | Open | Implementations; conformance tests |
| 7 | **Identity and authentication** for agents and adapters | Open | Grants; revocation; how far an adapter-only identity can be trusted |
| 8 | **When Muse and Duet are added** | Planned later | Adapters |
| 9 | **Where PACP lives**: in this repository or its own | Open | Governance; independent versioning |
| 10 | **Names**: whether "OpenFleet" and "PACP" are free to use | Not checked | Publishing beyond a preview |
| 11 | **Business multiplayer** | Future scope | Not before the single-user design is proven |
