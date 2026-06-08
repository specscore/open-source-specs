# MiniJQ Spec Package

Unofficial SpecScore specifications for a MiniJQ subset, based on
[`itchyny/gojq`](https://github.com/itchyny/gojq), a pure Go implementation of
jq.

jq is a command-line JSON processor and query language used to filter,
transform, and format JSON data.

This package is a draft reference artifact. It is not an official jq or gojq
specification, and it does not imply endorsement by jq or gojq maintainers.

The behavior docs use [SpecScore feature specifications](https://specscore.md/feature-specification)
to keep requirements and acceptance checks traceable.

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
  jq-like implementation against these specifications.
- [`eval/fixtures/`](eval/fixtures/) contains JSON inputs and expected outputs.

## Implementation Prompts

These prompts are intended for comparable MiniJQ reimplementation runs across
languages. Each prompt asks for both an importable library/module and a CLI, with
roughly parallel structure: lexer, parser, AST, evaluator, CLI adapter, tests,
and fixtures. They intentionally leave implementation details open enough for
the target language to feel natural, while keeping the project shape similar
enough to compare behavior, test coverage, and implementation effort. Each
prompt lists the hard acceptance-check target, the desired coverage target, and
the source-use constraints.

Implementation runs must be spec-only. The implementer MUST NOT inspect jq,
gojq, jqlang documentation, jq/gojq tests, existing jq implementations, or any
other repository to infer behavior. The only behavioral sources are the
specifications in `spec/` and the acceptance checks in `eval/`. External
references are allowed only for the implementation language, runtime, JSON
parser, CLI parser, test framework, and other imported libraries needed to build
the solution. The jq test suite may be used only after implementation as a
filtered CLI validation harness, not as an implementation guide.

The CLI must follow the [MiniJQ CLI feature specification](spec/features/cli/README.md)
and its `eval`, `check`, and `version` subcommand specs.

These prompts ([Go](#go), [Rust](#rust), [Python](#python), [TypeScript](#typescript),
[C#](#c)) are intended for comparable MiniJQ reimplementation runs.

### Go

```text
Implement MiniJQ in Go from the MiniJQ specifications in this directory. The
specifications are written in the SpecScore.md format.

Requirements:
- Build both an importable library and a CLI.
- Use Go 1.22+, `encoding/json` for JSON, `github.com/spf13/cobra` for CLI
  argument parsing, and the standard `testing` package.
- Use only the specifications in `spec/` and the acceptance checks in `eval/` as
  behavioral sources.
- Do not inspect jq/gojq source, docs, tests, existing implementations, or other
  repositories to infer semantics.
- External references are allowed only for Go, `encoding/json`, `cobra`,
  `testing`, and other imported implementation libraries.
- Pass 100% of the documented acceptance checks; this is a hard requirement.
- Aim for 100% code test coverage as well; this is strongly desired, but not a
  blocker if all acceptance checks pass and coverage gaps are documented.
- After implementation, test the CLI against the existing jq test suite,
  filtered to MiniJQ-specified behavior; skip and document out-of-scope jq
  tests.
- Implement the `minijq eval`, `minijq check`, and `minijq version` subcommands
  specified in `spec/features/cli/`.

Suggested structure:
- `pkg/minijq/` for lexer, parser, AST, evaluator, public API, and errors.
- `cmd/minijq/` for the CLI.
- `testdata/` or `tests/fixtures/` for JSON fixtures and expected outputs.

The library should expose a small API such as `Parse(query string)` and
`Evaluate(query, input)` or equivalent. Cover the documented MiniJQ subset only:
field lookup, nested lookup, array iteration, array index, pipe composition,
object construction, `length`, `keys`, `has()`, `map()`, `select()`, and `type`.
```

### Rust

```text
Implement MiniJQ in Rust from the MiniJQ specifications in this directory. The
specifications are written in the SpecScore.md format.

Requirements:
- Build both an importable crate and a CLI.
- Use stable Rust with edition 2021, `serde_json` for JSON, `clap` for CLI
  argument parsing, and Rust's standard test framework. `assert_cmd` and
  `predicates` may be used for CLI acceptance tests.
- Use only the specifications in `spec/` and the acceptance checks in `eval/` as
  behavioral sources.
- Do not inspect jq/gojq source, docs, tests, existing implementations, or other
  repositories to infer semantics.
- External references are allowed only for Rust, Cargo, `serde_json`, `clap`,
  test libraries, and other imported implementation libraries.
- Pass 100% of the documented acceptance checks; this is a hard requirement.
- Aim for 100% code test coverage as well; this is strongly desired, but not a
  blocker if all acceptance checks pass and coverage gaps are documented.
- After implementation, test the CLI against the existing jq test suite,
  filtered to MiniJQ-specified behavior; skip and document out-of-scope jq
  tests.
- Implement the `minijq eval`, `minijq check`, and `minijq version` subcommands
  specified in `spec/features/cli/`.

Suggested structure:
- `src/lib.rs` plus modules for lexer, parser, AST, evaluator, public API, and
  errors.
- `src/bin/minijq.rs` for the CLI entry point.
- `tests/` and `tests/fixtures/` for acceptance tests and JSON fixtures.

The crate should expose a small API such as `parse(query: &str)` and
`evaluate(query, input)` or equivalent. Cover the documented MiniJQ subset only:
field lookup, nested lookup, array iteration, array index, pipe composition,
object construction, `length`, `keys`, `has()`, `map()`, `select()`, and `type`.
```

### Python

```text
Implement MiniJQ in Python from the MiniJQ specifications in this directory. The
specifications are written in the SpecScore.md format.

Requirements:
- Build both an importable package and a CLI.
- Use Python 3.12+, the standard `json` module, `argparse` for CLI argument
  parsing, and `pytest` for tests.
- Use only the specifications in `spec/` and the acceptance checks in `eval/` as
  behavioral sources.
- Do not inspect jq/gojq source, docs, tests, existing implementations, or other
  repositories to infer semantics.
- External references are allowed only for Python, `json`, `argparse`, `pytest`,
  and other imported implementation libraries.
- Pass 100% of the documented acceptance checks; this is a hard requirement.
- Aim for 100% code test coverage as well; this is strongly desired, but not a
  blocker if all acceptance checks pass and coverage gaps are documented.
- After implementation, test the CLI against the existing jq test suite,
  filtered to MiniJQ-specified behavior; skip and document out-of-scope jq
  tests.
- Implement the `minijq eval`, `minijq check`, and `minijq version` subcommands
  specified in `spec/features/cli/`.

Suggested structure:
- `src/minijq/` for lexer, parser, AST, evaluator, public API, and errors.
- `src/minijq/cli.py` for the CLI entry point.
- `tests/` and `tests/fixtures/` for acceptance tests and JSON fixtures.

The package should expose a small API such as `parse(query)` and
`evaluate(query, value)` or equivalent. Cover the documented MiniJQ subset only:
field lookup, nested lookup, array iteration, array index, pipe composition,
object construction, `length`, `keys`, `has()`, `map()`, `select()`, and `type`.
```

### TypeScript

```text
Implement MiniJQ in TypeScript from the MiniJQ specifications in this directory.
The specifications are written in the SpecScore.md format.

Requirements:
- Build both an importable npm package and a CLI.
- Use Node.js 20+, TypeScript, `commander` for CLI argument parsing, Node's
  built-in filesystem/stdin APIs, and `vitest` for tests.
- Use only the specifications in `spec/` and the acceptance checks in `eval/` as
  behavioral sources.
- Do not inspect jq/gojq source, docs, tests, existing implementations, or other
  repositories to infer semantics.
- External references are allowed only for Node.js, TypeScript, `commander`,
  `vitest`, and other imported implementation libraries.
- Pass 100% of the documented acceptance checks; this is a hard requirement.
- Aim for 100% code test coverage as well; this is strongly desired, but not a
  blocker if all acceptance checks pass and coverage gaps are documented.
- After implementation, test the CLI against the existing jq test suite,
  filtered to MiniJQ-specified behavior; skip and document out-of-scope jq
  tests.
- Implement the `minijq eval`, `minijq check`, and `minijq version` subcommands
  specified in `spec/features/cli/`.

Suggested structure:
- `src/` for lexer, parser, AST, evaluator, public API, and errors.
- `src/cli.ts` for the CLI entry point.
- `tests/` and `tests/fixtures/` for acceptance tests and JSON fixtures.

The package should export a small API such as `parse(query)` and
`evaluate(query, value)` or equivalent. Cover the documented MiniJQ subset only:
field lookup, nested lookup, array iteration, array index, pipe composition,
object construction, `length`, `keys`, `has()`, `map()`, `select()`, and `type`.
```

### C#

```text
Implement MiniJQ in C# from the MiniJQ specifications in this directory. The
specifications are written in the SpecScore.md format.

Requirements:
- Build both an importable .NET library and a CLI.
- Use .NET 8+, `System.Text.Json` for JSON, `System.CommandLine` for CLI
  argument parsing, and xUnit for tests.
- Use only the specifications in `spec/` and the acceptance checks in `eval/` as
  behavioral sources.
- Do not inspect jq/gojq source, docs, tests, existing implementations, or other
  repositories to infer semantics.
- External references are allowed only for .NET, C#, `System.Text.Json`,
  `System.CommandLine`, xUnit, and other imported implementation libraries.
- Pass 100% of the documented acceptance checks; this is a hard requirement.
- Aim for 100% code test coverage as well; this is strongly desired, but not a
  blocker if all acceptance checks pass and coverage gaps are documented.
- After implementation, test the CLI against the existing jq test suite,
  filtered to MiniJQ-specified behavior; skip and document out-of-scope jq
  tests.
- Implement the `minijq eval`, `minijq check`, and `minijq version` subcommands
  specified in `spec/features/cli/`.

Suggested structure:
- `src/MiniJq/` for lexer, parser, AST, evaluator, public API, and errors.
- `src/MiniJq.Cli/` for the CLI entry point.
- `tests/MiniJq.Tests/` for acceptance tests and JSON fixtures.

The library should expose a small API such as `Parse(string query)` and
`Evaluate(query, input)` or equivalent. Cover the documented MiniJQ subset only:
field lookup, nested lookup, array iteration, array index, pipe composition,
object construction, `length`, `keys`, `has()`, `map()`, `select()`, and `type`.
```

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
