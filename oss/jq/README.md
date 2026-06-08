# jq Spec Package

Unofficial SpecScore specifications for jq behavior, based on
[`itchyny/gojq`](https://github.com/itchyny/gojq), a pure Go implementation of
jq.

This package is a draft reference artifact. It is not an official jq or gojq
specification, and it does not imply endorsement by jq or gojq maintainers.

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
| [`basic-filter-evaluation`](spec/features/basic-filter-evaluation/README.md) | Identity, field lookup, array iteration, pipe composition, and object construction. | Draft |
| [`input-and-output-modes`](spec/features/input-and-output-modes/README.md) | JSON input, null input, compact output, raw output, and invalid JSON handling. | Draft |
| [`go-library-api`](spec/features/go-library-api/README.md) | Embedding gojq with `Parse`, `Compile`, `Run`, compiler options, and iterator errors. | Draft |

## Evaluation

- [`eval/acceptance-checks.md`](eval/acceptance-checks.md) maps checks to AC IDs.
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
