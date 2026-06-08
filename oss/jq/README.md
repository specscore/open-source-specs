# MiniJQ Spec Package

Unofficial SpecScore specifications for a MiniJQ subset, based on
[`itchyny/gojq`](https://github.com/itchyny/gojq), a pure Go implementation of
jq.

This package is a draft reference artifact. It is not an official jq or gojq
specification, and it does not imply endorsement by jq or gojq maintainers.

## Decision Direction

The target is a deliberately small jq subset, not the full jq language.

Rationale:

- full jq is too large for a first SpecScore benchmark;
- a MiniJQ subset can be specified completely enough for implementation;
- the subset is still large enough to require a parser, AST, evaluator, tests,
  and error handling;
- `gojq` is a practical behavioral reference because it is a substantial,
  compatibility-oriented Go reimplementation with public source and tests.

The current working benchmark target is:

| Field | Draft value |
|---|---|
| Tool | MiniJQ |
| Implementation target | Rust |
| Public tests | 30 |
| Hidden tests | 30 |
| Behavioral reference | `itchyny/gojq`, with jq docs/tests as secondary context |

## Source Material

Primary source:

- [`itchyny/gojq`](https://github.com/itchyny/gojq)
- [`README.md`](https://github.com/itchyny/gojq/blob/main/README.md)
- [`cli/`](https://github.com/itchyny/gojq/tree/main/cli)
- [`query.go`](https://github.com/itchyny/gojq/blob/main/query.go)
- [`compiler.go`](https://github.com/itchyny/gojq/blob/main/compiler.go)
- [`option.go`](https://github.com/itchyny/gojq/blob/main/option.go)

## Spec Contents

| Feature | What it covers | Status |
|---|---|---|
| [`basic-filter-evaluation`](spec/features/basic-filter-evaluation/README.md) | Identity, field lookup, array indexing, array iteration, pipes, object construction, and the first MiniJQ builtins. | Draft |
| [`input-and-output-modes`](spec/features/input-and-output-modes/README.md) | JSON input, null input, compact output, raw output, and invalid JSON handling. | Draft |
| [`go-library-api`](spec/features/go-library-api/README.md) | Embedding gojq with `Parse`, `Compile`, `Run`, compiler options, and iterator errors. | Draft |

## Candidate MiniJQ Subset

The draft subset currently includes:

- `.foo`
- `.bar.baz`
- `.[]`
- `.[0]`
- `length`
- `keys`
- `has()`
- `map()`
- `select()`
- `type`

The exact final list is still open. The current rule is to include only behavior
that can be backed by fixtures and checked deterministically.

## Evaluation

- [`eval/acceptance-checks.md`](eval/acceptance-checks.md) maps checks to AC IDs.
- [`eval/benchmark-plan.md`](eval/benchmark-plan.md) records the MiniJQ
  benchmark shape.
- [`eval/scoring-rubric.md`](eval/scoring-rubric.md) describes how to judge a
  jq-like implementation against this package.
- [`eval/fixtures/`](eval/fixtures/) contains JSON inputs and expected outputs.

## Scope Boundary

This draft starts with behavior that is easy to inspect from `gojq` examples and
source. It intentionally does not attempt full jq language coverage.

Deferred:

- complete jq grammar coverage;
- complete builtin-function coverage;
- streaming mode;
- YAML input/output;
- color and terminal behavior;
- exact diagnostic wording across platforms.
