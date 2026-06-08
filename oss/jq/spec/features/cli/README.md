---
format: https://specscore.md/feature-specification
status: Draft
---
# Feature: CLI

> [SpecScore.**Studio**](https://specscore.studio): | [Explore](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli?op=explore) | [Edit](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli?op=edit) | [Ask question](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli?op=ask) | [Request change](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli?op=request-change) |
**Status:** Draft
**Source Ideas:** —

## Summary

The MiniJQ CLI exposes the importable evaluator through a small command surface:
`eval` for running a query, `check` for validating query syntax, and `version`
for reporting build identity.

This document follows the [SpecScore feature specification](https://specscore.md/feature-specification).

## Problem

The benchmark compares multiple language implementations. Without a shared CLI
shape, a passing implementation can still be difficult to run, test, or compare
across languages.

## Contents

| Directory | Description |
|---|---|
| [eval/](eval/README.md) | Evaluate a MiniJQ query against JSON input. |
| [check/](check/README.md) | Parse and validate a MiniJQ query without evaluating input. |
| [version/](version/README.md) | Report implementation version/build identity. |

### eval

Runs a MiniJQ query against JSON read from stdin or file paths. It owns the
query/input/output behavior described by [input-and-output-modes](../input-and-output-modes/README.md).

### check

Parses a query and reports whether it is valid for the documented MiniJQ subset.
It does not require JSON input.

### version

Prints implementation version information for humans and a terse version flag
surface for scripts.

## Behavior

### Subcommand Structure

#### REQ: explicit-subcommands

The CLI MUST expose explicit `eval`, `check`, and `version` subcommands. A bare
`minijq` invocation MUST show help or usage information; it MUST NOT run an
implicit query.

#### REQ: help-flag

The CLI MUST accept `-h` and `--help` at the root and subcommand level. Help
output MUST exit `0`.

### Output and Errors

#### REQ: stdout-data-only

Successful command results MUST be written to stdout. Error explanations MUST be
written to stderr so stdout remains safe for pipelines and tests.

#### REQ: stable-exit-codes

The CLI MUST use stable exit-code meanings:

| Exit code | Meaning |
|---|---|
| `0` | Success |
| `2` | Invalid arguments, invalid query, or invalid JSON input |
| `3` | Input file not found |
| `10` | Unexpected runtime error |

### Project Shape

#### REQ: importable-library-backed

The CLI MUST call the same importable library/module API that tests and users
can import directly. CLI-only evaluator logic is not allowed.

## Acceptance Criteria

### AC: root-help-does-not-evaluate

**Given** no subcommand
**When** the user runs `minijq`
**Then** the command prints usage and does not attempt to parse stdin as JSON.

### AC: eval-subcommand-exists

**Given** JSON input and a documented MiniJQ query
**When** the user runs `minijq eval <query>`
**Then** the query is evaluated according to the [eval](eval/README.md)
subcommand contract.

### AC: check-subcommand-exists

**Given** a documented MiniJQ query
**When** the user runs `minijq check <query>`
**Then** the command validates the query according to the
[check](check/README.md) subcommand contract.

### AC: version-subcommand-exists

**Given** an installed MiniJQ implementation
**When** the user runs `minijq version`
**Then** the command reports version information according to the
[version](version/README.md) subcommand contract.

## Open Questions

- Should `minijq <query>` be accepted as an alias for `minijq eval <query>` in a
  later compatibility-focused phase, or should the benchmark keep explicit
  subcommands only?

---
*This document follows the https://specscore.md/feature-specification*
