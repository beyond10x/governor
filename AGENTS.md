# AGENTS.md — governor

What the governor is and how to build it is in [README.md](README.md); this file is what an agent
changing it must know. The cross-repository architecture is Atlas ADRs 0066–0075, 0077 and 0079,
and the Atlas ADR that places the governor here.

## Serves

- **O2 — decisions as data, with evidence.** A case's frontier and completion are what its protocol
  decides over attributable evidence, nothing else.

## Boundary

- The governor implements Commission's `Governor` port (`current_revision`, `frontier`,
  `completion`) and `EvidencePort`, and evaluates the case's protocol with Canon. It owns the case
  snapshot, the artifact revisions and the evidence submitted for a case.
- Commission defines the ports and stays domain-neutral: it never depends on Canon. Canon owns
  protocol semantics; ELS owns the protocols. The governor re-implements neither.
- The governor decides; it never executes actions, never supplies authority and never turns an
  executor's output into evidence (Atlas ADR 0074).

## Rules

- Anything that runs is Rust; command lines use clap derive.
- Canon is used through its library, pinned to an exact revision; the governor adds no clock,
  network or model call to an evaluation.
- No `/home/<name>/` path literals anywhere: common Gates personal-paths has no allowance.

## ESS

The governor opts out of an ESS domain of its own: its nouns are Commission's (`CaseId`,
`Frontier`, `Evidence`, `CompletionDetermination`, generated from Commission's ESS specification)
and Canon's (case snapshot, decision; Canon opts out in favour of its own conformance). A noun the
governor introduces gets an `ess/` domain here before a story is written around it.

## Work

- Planned in the AEP store under `.engineering/`, written only through `aep plan artifact`. Body
  drafts go in `.engineering/drafts/` (ignored).
- Build with `CARGO_TARGET_DIR=$HOME/.cache/b10x-target/governor` (the Taskfile sets it); give each
  worktree its own `CARGO_TARGET_DIR` before trusting a gate run there.
- Every commit and push is `b10x-bot[bot]`'s through `b10x-gates bot`; every GitHub write goes
  through `b10x-gates api`.
- Use a managed worktree (`worktree create --repo governor --purpose …`) for changes.
