# Governor

> **Archived. This code lives in [Loom](https://github.com/beyond10x/loom)** (crate `loom-governor`), per
> Atlas ADR 0090 (2026-10-05): Loom is the one runtime repository. Open issues and changes
> there. This repository is read-only and kept for its history.

The governor owns a case's truth. Commission asks it for a case's current revision, the frontier of
actions issued for that revision, and whether the case is complete; evidence reaches it through
Commission's evidence port. This governor answers by evaluating the case's protocol with
[Canon](https://github.com/beyond10x/canon) — the protocols come from
[ELS](https://github.com/beyond10x/els) — so Commission stays domain-neutral and never sees Canon.

**Status: nothing is built yet.** The crate is empty; the plan is in the AEP store under
`.engineering/`. Its first user is the vertical slice in
[Intake](https://github.com/beyond10x/intake).

## Build

```console
task check
```

Requires Rust 1.98 or newer and [go-task](https://taskfile.dev).

## Licence

Apache-2.0.
