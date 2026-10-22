# Architecture diagrams

These diagrams are Mermaid source; GitHub renders them in place. Each node is labelled **Current**, **Unverified**, **Proposed**, **Deferred** or **Future**. Dashed outlines mark anything that is not current behaviour.

## 1. Current: the manual workflow

**Status: Current.** This is how work moves today, entirely by hand. No OpenFleet software is involved. The person is the integration layer.

```mermaid
sequenceDiagram
  autonumber
  actor Person
  participant PA as Personal assistant
  participant CLI as Local coding CLI
  participant Cloud as Cloud coding sessions
  participant Git as Git host
  Person->>PA: Describe the goal
  PA-->>Person: Draft a plan or prompt
  Person->>CLI: Paste the prompt and run a command
  CLI->>Cloud: Start one or more sessions
  Cloud->>Git: Push a branch or open a pull request
  Cloud-->>Person: Status and summary, read in the cloud tool
  Person->>PA: Paste the results back for the next step
  Note over Person,PA: Each assistant keeps its own memory. Context moves only when the person copies it.
```

## 2. First pilot: dot → controller on a user-owned PC → CloudRoom factory

**Status: Proposed. The control route is unverified. Nothing here has been built or accepted.** The PC is the controller host chosen for this pilot. Users should not need an always-on machine of their own. An event-driven controller hosted in CloudRoom is a proposed, untested alternative (dotted path). dot is an integration target, not a verified integration, and its commands and buttons do not exist yet.

```mermaid
flowchart TB
  USER["User"]
  DOT["dot, personal agent<br/>controller commands and buttons: PROPOSED, not built"]
  subgraph PLAN["Built-in plan, PROPOSED"]
    direction LR
    IDEA["Grill or Wayfinder"] --> PLANDOC["Plan: a version, spec or tickets"]
  end
  PLANOK{"User approves the plan<br/>covers this plan only, not every action"}
  subgraph PC["User-owned PC: chosen for this pilot, not required for everyone"]
    CTRL["Controller, PROPOSED<br/>one owner at a time, explicit handoff"]
    LOG[("Durable request and receipt log, PROPOSED")]
    REG[("Session registry, PROPOSED<br/>roles and observed configuration")]
  end
  ROUTE["Control route to CloudRoom<br/>UNVERIFIED"]
  ALT["Alternative host, PROPOSED, UNTESTED<br/>event-driven controller in CloudRoom automations and subagents<br/>state in GitHub and the issue tracker, no inbound endpoint assumed"]
  subgraph FACTORY["CloudRoom factory: bounded loop per ticket attempt, PROPOSED"]
    direction LR
    BUILD["Build"] --> TEST["Test"] --> REVIEW["Independent review"]
    REVIEW -->|"changes requested, attempts left"| REWORK["Rework"]
    REWORK --> TEST
  end
  GATE{"Policy gate, set by the user<br/>request approval, or proceed only under<br/>applicable prior authorization"}
  ESC["Stop and escalate to the user"]
  UNSUP["Unsupported or uncertain<br/>reported as such, never emulated"]
  MERGE["Merge on GitHub"]
  CLEAN["Automatic destructive workspace cleanup<br/>DEFERRED, owner-approved, integration pending"]

  USER -->|"decides through commands and buttons"| DOT
  DOT --> IDEA
  PLANDOC --> PLANOK
  PLANOK -->|"changes requested"| IDEA
  PLANOK -->|"approved"| CTRL
  PLANOK -.->|"approved, alternative host"| ALT
  ALT -.->|"start sessions"| BUILD
  CTRL --> LOG
  CTRL --> REG
  CTRL -->|"start, reuse, drain or rotate sessions"| ROUTE
  ROUTE --> BUILD
  ROUTE -.->|"route unavailable or outcome unknown"| UNSUP
  REVIEW -->|"passes, reaches a checkpoint"| GATE
  REVIEW -->|"attempt limit reached"| ESC
  GATE -->|"approval requested"| DOT
  GATE -->|"approved, or prior authorization applies"| MERGE
  GATE -->|"changes requested"| REWORK
  ESC --> DOT
  UNSUP --> DOT
  CTRL -.-> CLEAN

  classDef proposed stroke-dasharray: 5 5
  class DOT,IDEA,PLANDOC,PLANOK,CTRL,LOG,REG,ROUTE,ALT,BUILD,TEST,REVIEW,REWORK,GATE,ESC,UNSUP,MERGE,CLEAN proposed
```

Notes:

- **Gates are policy, not fixed steps.** The user decides which checkpoints, merges included, request approval and which may proceed under an applicable prior authorization. Approving the plan does not authorise every action the plan leads to.
- **User control has limits.** No policy can exceed provider permissions, platform restrictions or applicable safety requirements.
- Requests and receipts are logged durably, so an interrupted or duplicated request is reconciled rather than repeated blindly.
- The builder and the independent reviewer are separate sessions with recorded roles.
- Automatic destructive cleanup of workspaces is deferred. Until it exists, cleanup is a manual action.
- The alternative host would rely on CloudRoom's documented [automations](https://www.cloudroom.dev/docs/automations) and [subagents](https://www.cloudroom.dev/docs/subagents). It is untested for this use. It promises no stable inbound endpoint, no always-on sandbox server, and no verified launch while the user's own computer is off.

## 3. Proposed: personal agents, shared memory and coding factories

**Status: Proposed. Integrations are unverified targets. Business multiplayer is Future.** A user can start with one personal agent. No personal-agent interface is assumed to exist: each adapter needs a supported, documented interface that the user is allowed to use.

```mermaid
flowchart TB
  USER["User<br/>sets policy, grants and revokes access, decides when asked"]
  CLI["OpenFleet CLI and agent commands<br/>primary interfaces, PROPOSED"]
  WEB["OpenFleet Web, PROPOSED<br/>live graph and control surface, same backend and operations<br/>not a separate state authority"]
  TRACK[("User's existing issue tracker<br/>GitHub Issues or another")]
  LIMITS["Provider permissions, platform restrictions,<br/>applicable safety requirements"]

  subgraph AGENTS["Personal agents: start with one, add more later"]
    PA1["dot<br/>initial target, UNVERIFIED"]
    PA2["Grok Bot<br/>initial target, UNVERIFIED"]
    PA3["Muse, Duet<br/>planned later, FUTURE"]
  end

  subgraph ADAPT["Adapters, PROPOSED: none verified"]
    AD1["Adapter for dot"]
    AD2["Adapter for Grok Bot"]
    AD3["Later adapters"]
  end

  subgraph FLEET["Coordination layer, PROPOSED"]
    POL["Policy and grants<br/>gates, prior authorizations, revocation"]
    COORD["Coordinator<br/>plans, delegation, handoff, status"]
    BROKER["Memory broker<br/>scoped references, automatic capture"]
    AUDIT[("Provenance and audit log")]
  end

  MEM[("User's private repository<br/>memory captured automatically, ongoing domain onboarding<br/>detailed behaviour: separate design session")]

  subgraph DOWN["Downstream coding factory on the user's own subscriptions and compute, PROPOSED"]
    CTRL["Controller, orchestrator role<br/>user-owned PC, or event-driven CloudRoom host, untested"]
    subgraph RUNLOOP["Bounded loop"]
      direction LR
      B["Build"] --> T["Test"] --> R["Review"]
      R -->|"rework, within attempt limit"| W["Rework"]
      W --> T
    end
    GATE{"Policy gate<br/>request approval, or proceed only under<br/>applicable prior authorization"}
    STOP["Stop and escalate to the user"]
  end

  GIT["GitHub, required at first"]
  FUT["Business multiplayer<br/>FUTURE SCOPE"]

  PA1 --- AD1
  PA2 --- AD2
  PA3 -.- AD3
  AD1 -->|"PACP v0.1 draft"| COORD
  AD2 -->|"PACP v0.1 draft"| COORD
  AD3 -.->|"PACP v0.1 draft"| COORD
  USER --> CLI
  USER --> WEB
  CLI -->|"operations"| COORD
  WEB -->|"same operations"| COORD
  COORD -->|"running, blocked, done, loop and review progress, policy decisions"| WEB
  COORD ---|"tickets"| TRACK
  USER --> POL
  LIMITS -.->|"bound every policy"| POL
  COORD --> POL
  COORD --> BROKER
  BROKER ---|"scoped reads, captured writes"| MEM
  COORD --> AUDIT
  BROKER --> AUDIT
  COORD -->|"approved plan, bounded"| CTRL
  CTRL --> B
  R -->|"passes, reaches a checkpoint"| GATE
  R -->|"limit reached"| STOP
  POL -.->|"gate rules"| GATE
  GATE -->|"approval requested"| COORD
  GATE -->|"approved, or prior authorization applies"| GIT
  STOP --> COORD
  CTRL -->|"status, results, unsupported or uncertain"| COORD
  FUT -.- FLEET

  classDef proposed stroke-dasharray: 5 5
  class CLI,WEB,PA1,PA2,PA3,AD1,AD2,AD3,POL,COORD,BROKER,AUDIT,MEM,CTRL,B,T,R,W,GATE,STOP,FUT proposed
```

OpenFleet Web reads and acts through the same operations as the CLI and agent commands. When implemented, each item it shows would link to the real underlying artifact.

Memory and context windows are deliberately separate. An agent receives scoped references, and only the content its grant allows. What lands in its context window is a transient copy. How capture, consent, conflicts, encryption and retention work is left to a separate memory design session.

## 4. Proposed: private development to public release

**Status: Proposed process.** The private development repository and the public repository share no Git history. Public files are written fresh and pass an allowlisted export review.

```mermaid
flowchart TD
  SRC["Other private sources<br/>read-only reference, nothing copied"]
  DEV["Private development<br/>notes, drafts, experiments"]
  CAND["Candidate public change<br/>written fresh, synthetic examples only"]
  ALLOW{"Allowlist check<br/>only approved file paths"}
  SCAN{"Privacy, secret and private-link scan<br/>plus claim and status check"}
  MAINT{"Maintainer approves exact file list and diff"}
  PUB["Public repository<br/>ordinary commit, no force-push"]
  VERIFY["Read back the remote tree and compare"]
  TAG["Tag and release notes<br/>only for an approved release"]

  SRC -.->|"reference only"| DEV
  DEV --> CAND
  CAND --> ALLOW
  ALLOW -->|"file not on the allowlist"| CAND
  ALLOW --> SCAN
  SCAN -->|"finding"| CAND
  SCAN --> MAINT
  MAINT -->|"changes requested"| CAND
  MAINT -->|"approved"| PUB
  PUB --> VERIFY
  VERIFY -->|"mismatch"| MAINT
  VERIFY --> TAG
```
