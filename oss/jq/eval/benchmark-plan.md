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

## Testing Strategy

The first correctness gate is this repository's documented acceptance checks.
Every implementation run must pass those checks before it is considered
complete.

After the acceptance checks pass, each generated CLI should also be tested
against the existing jq test suite as a compatibility signal. The harness should
run only tests whose filters, inputs, and expected outputs are covered by the
MiniJQ specifications in this package. Tests for full jq behavior that is not in
scope must be skipped with a documented reason rather than treated as new
requirements.

The jq test suite is validation material, not a behavioral source for
implementation. Implementers should not inspect jq tests to infer semantics
before or during implementation; use the suite after implementation to identify
compatibility gaps and to inform future spec work.

## Open Questions

- Is the current MiniJQ feature list final?
- Should hidden tests come from jq tests, gojq tests, or independently authored cases?
- How much jq documentation should be cited directly versus gojq source/tests?
- Should MiniJQ become a standalone benchmark project after this package settles?
