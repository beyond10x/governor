---
format: aep.planning-md/3
id: review-result:adversary-w16-governor-canon-governor-pass-2
kind: review-result
status: active
title: Wave 2026-10-04-w16 adversary, governor story:canon-governor, pass 2
relations:
- reviews: story:canon-governor
revision: 1
---
## Adversary pass 2 — governor story:canon-governor

Tree: pass-1 tree + its tests. Verdict CONFIRMED; cases 15→21, red 1. New file: `crates/governor/tests/adversary2_governor.rs` (6 cases).

| # | where | verdict | origin | finding |
|---|---|---|---|---|
| 1 | `crates/governor/src/lib.rs:563` | INFEASIBLE | introduced | evidence reached Canon as JSON text read as YAML; U+FFFE/U+FFFF, DEL and C1 controls dropped the record |
| 2 | commission `crates/commission-testkit/src/kits/governor.rs:419` (e61e4f0) | CONFIRMED | pre-existing | the governor kit never reads observations after evidence; a governor recording evidence as observations passes it |
| 3 | `crates/governor/src/lib.rs:258` | INFEASIBLE | introduced | `open` loops forever if a store `insert` fails a write; `CaseStore::insert` has no error channel |
| 4 | `crates/governor/src/lib.rs:306` | CONFIRMED | introduced | `update_revision` to the held revision raised the case revision |

| # | finding | decision |
|---|---|---|
| 1 | lib.rs:563 evidence reaches Canon as JSON text parsed as YAML; U+FFFE/U+FFFF, DEL, C1 controls drop the record | accept, fix: convert the JSON value to Canon's value type directly (`evidence_from_value` or equivalent), no text round trip; adversary2 case `a_record_canon_evaluates_has_its_effect_through_the_governor` goes green |
| 2 | commission-testkit governor kit never reads observations after evidence | accept as a pre-existing Commission gap; adversary2 `observations_are_exactly_what_was_observed_never_evidence` covers it here; noted in the review record, no issue filed |
| 3 | lib.rs:258 `open` loops forever if a store's `insert` fails a write | decline for now: only `MemoryCaseStore` exists; fix: document on `CaseStore::insert` that `false` means the id is held and nothing else; the durable-store story gives the trait an error channel |
| 4 | lib.rs:306 `update_revision` with the current revision raises the case revision | accept, fix: an update to the revision the case already holds changes nothing and returns the current case revision; story wording "a new revision" kept; add a case |
| 5 | fix pass changed the adversary2 fixture `advance` (R{next} -> M{next}) because row 4 made its first move a no-op | accept: assertions unchanged; the fixture relied on the bump row 4 removed |

After the fixes: integration gate on wave/2026-10-04-w16 (71e0b9c): fmt, clippy, tests (22 passed, 0 failed), aep validate, all exit 0.
