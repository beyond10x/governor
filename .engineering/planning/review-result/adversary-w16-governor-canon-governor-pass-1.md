---
format: aep.planning-md/3
id: review-result:adversary-w16-governor-canon-governor-pass-1
kind: review-result
status: active
title: Wave 2026-10-04-w16 adversary, governor story:canon-governor, pass 1
relations:
- reviews: story:canon-governor
revision: 1
---
## Adversary pass 1 — governor story:canon-governor

Tree: 75a94b3 + phase 2. Verdict CONFIRMED (no red case against the code); cases 2→15. New file: `crates/governor/tests/adversary_governor.rs` (13 cases).

Findings (each test gap shown by a mutant that the unit suite let through and the new cases kill):

| # | where | verdict | finding |
|---|---|---|---|
| 1 | `crates/governor/src/lib.rs:365` | CONFIRMED | frontier claim values never compared with Canon (True/False swap survived) |
| 2 | `crates/governor/src/lib.rs:380` | CONFIRMED | obligations never exercised (inversion survived) |
| 3 | `crates/governor/src/lib.rs:588` | CONFIRMED | frontier-id determinism and distinctness unasserted |
| 4 | `crates/governor/src/lib.rs:136` | CONFIRMED | `CaseExists` refusal untested (overwrite survived) |
| 5 | `crates/governor/Cargo.toml:14` | CONFIRMED | Canon `branch = "main"` against AGENTS.md "exact revision" |

| # | finding | decision |
|---|---|---|
| 1 | lib.rs:365 claim values never compared with Canon | accept; adversary_governor.rs kills the swap mutant; no code change |
| 2 | lib.rs:380 obligations never exercised | accept; adversary_governor.rs covers incident.response@1; no code change |
| 3 | lib.rs:588 frontier-id determinism/distinctness unasserted | accept; adversary_governor.rs kills the mutant; no code change |
| 4 | lib.rs:136 CaseExists refusal untested | accept; adversary_governor.rs kills the overwrite mutant; no code change |
| 5 | Cargo.toml:14 Canon `branch = "main"` against AGENTS.md "exact revision" | accept the manifest; change the rule: AGENTS.md now says Cargo.lock pins the exact revision (gates are `--locked`) and the manifest uses ELS's reference so one Canon builds (coordinator edit in the unit tree) |
