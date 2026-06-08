---
format: https://specscore.md/feature-specification
status: Draft
---
# Feature: Eval Subcommand

> [SpecScore.**Studio**](https://specscore.studio): | [Explore](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/eval?op=explore) | [Edit](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/eval?op=edit) | [Ask question](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/eval?op=ask) | [Request change](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/eval?op=request-change) |
**Status:** Draft
**Source Ideas:** —

## Summary

`minijq eval` evaluates one MiniJQ query against JSON input and writes the query
results. It is the main command used by acceptance tests.

This document follows the [SpecScore feature specification](https://specscore.md/feature-specification).

## Problem

The evaluator needs a stable command wrapper so implementations in different
languages can be compared with the same fixture commands.

## Behavior

### Synopsis

```text
minijq eval [flags] <query> [file...]
```

### Query and Input

#### REQ: query-argument

`eval` MUST require exactly one query argument unless `--help` is requested.
Missing or extra query arguments MUST exit `2`.

#### REQ: stdin-default

When no file path is supplied and null-input mode is not enabled, `eval` MUST
read JSON input from stdin.

#### REQ: file-inputs

When one or more file paths are supplied, `eval` MUST read JSON input from those
files in argument order. A missing file MUST exit `3`.

#### REQ: null-input-flag

`eval` MUST accept `-n` and `--null-input`. In this mode the query runs once
with `null` as input and does not require stdin or file input.

### Output Modes

#### REQ: compact-output-flag

`eval` MUST accept `-c` and `--compact-output` and emit JSON without
pretty-print indentation.

#### REQ: raw-output-flag

`eval` MUST accept `-r` and `--raw-output`. When an emitted result is a string,
raw-output mode writes the string contents without JSON quotes.

### Errors

#### REQ: invalid-query-error

When the query is outside the documented MiniJQ subset or cannot be parsed,
`eval` MUST exit `2` and write a useful query error to stderr.

#### REQ: invalid-json-error

When JSON input cannot be parsed, `eval` MUST exit `2` and write a useful JSON
error to stderr.

## Acceptance Criteria

### AC: eval-field-from-stdin

**Given** stdin JSON `{"foo":128}`
**When** the user runs `minijq eval '.foo'`
**Then** stdout contains `128`.

### AC: eval-compact-output

**Given** input file `fixtures/nested-object.json`
**When** the user runs `minijq eval -c '.' fixtures/nested-object.json`
**Then** stdout contains `{"a":{"b":42}}`.

### AC: eval-null-input

**Given** no JSON input
**When** the user runs `minijq eval -n 'type'`
**Then** stdout contains `"null"`.

### AC: eval-invalid-json-fails

**Given** input file `fixtures/invalid-object.json`
**When** the user runs `minijq eval '.' fixtures/invalid-object.json`
**Then** the command exits `2` and writes a JSON error to stderr.

## Open Questions

- Should multiple input files be part of the first benchmark harness, or is one
  file plus stdin enough for the initial public tests?

---
*This document follows the https://specscore.md/feature-specification*
