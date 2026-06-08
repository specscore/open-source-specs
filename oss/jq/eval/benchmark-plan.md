# MiniJQ Benchmark Plan

This draft benchmark uses a MiniJQ subset as a small but non-trivial target for
SpecScore-based specification and implementation.

The benchmark input is a [SpecScore](https://specscore.md) specification tree
with acceptance checks.

## Decision

Use MiniJQ, not full jq, as the benchmark target.

Use [`itchyny/gojq`](https://github.com/itchyny/gojq) as the primary behavioral
reference. jq documentation and jq/gojq tests can provide secondary context, but
the draft package should not claim that gojq is more correct than jq.

## Rationale

Full jq is too large for the first benchmark. A complete jq specification could
be very long and would distract from the question this artifact is meant to
answer: can a compact, explicit SpecScore spec reduce ambiguity and improve
implementation quality?

MiniJQ should be:

- small enough to specify completely;
- small enough for AI-assisted implementation;
- large enough to require a parser, AST, evaluator, tests, and error handling;
- deterministic enough for public and hidden acceptance tests.

## Draft Shape

| Field | Draft value |
|---|---|
| Tool | MiniJQ |
| Implementation language | Rust |
| Public tests | 30 |
| Hidden tests | 30 |
| Public commands | Around 10 command-level examples |
| Spec target length | Roughly 20-30 pages once complete |

## Benchmark Scenarios

Compare three implementation paths:

| Scenario | Input to implementer |
|---|---|
| Direct reimplementation from source | Source repository material and broad task prompt. |
| Specification-first with Spec Kit | Spec generated and carried through a Spec Kit workflow. |
| Specification-first with SpecScore | This MiniJQ SpecScore tree plus acceptance checks. |

## Metrics

Track:

- specification-generation tokens;
- implementation tokens;
- total tokens;
- public-test correctness;
- hidden-test pass rate;
- number and type of clarifying questions;
- manual repair cycles before passing tests.

## Open Questions

- Is the current MiniJQ feature list final?
- Should hidden tests come from jq tests, gojq tests, or independently authored cases?
- How much jq documentation should be cited directly versus gojq source/tests?
- Should MiniJQ become a standalone benchmark project after this package settles?
