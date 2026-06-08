---
format: https://specscore.md/feature-specification
status: Draft
---
# Feature: Input and Output Modes

> [SpecScore.**Studio**](https://specscore.studio): | [Explore](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/input-and-output-modes?op=explore) | [Edit](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/input-and-output-modes?op=edit) | [Ask question](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/input-and-output-modes?op=ask) | [Request change](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/input-and-output-modes?op=request-change) |
**Status:** Draft
**Source Ideas:** —

## Summary

The gojq command reads JSON input, evaluates a query, and writes one output per
emitted result. This draft covers standard JSON input, null input mode, compact
JSON output, raw string output, and invalid JSON reporting.

This document follows the [SpecScore feature specification](https://specscore.md/feature-specification).

## Problem

Filter semantics are not enough to specify a jq-compatible command. Agents and
humans need to know how input reaches the query and how emitted values are
serialized.

## Behavior

### Input

#### REQ: stdin-json-input

The command MUST read JSON values from standard input when input files are not
specified.

#### REQ: null-input-mode

When invoked with null input mode (`-n` / `--null-input`), the command MUST run
the query with `null` as the input value instead of requiring standard input.

#### REQ: invalid-json-error

When standard input is not valid JSON, the command MUST fail with an invalid
JSON error instead of evaluating the query against invented input.

### Output

#### REQ: one-output-per-result

The command MUST write each emitted query result as a separate output value.

#### REQ: compact-output

When invoked with compact output mode (`-c` / `--compact-output`), the command
MUST serialize JSON output without pretty-print indentation.

#### REQ: raw-string-output

When invoked with raw output mode (`-r` / `--raw-output`) and a result is a
string, the command MUST write the string contents without JSON string quotes.

## Acceptance Criteria

### AC: stdin-json-field-query (verifies REQ:stdin-json-input)

**Given** standard input `{"foo":128}`
**When** the command runs filter `.foo`
**Then** the output value is `128`.

### AC: null-input-computes-value (verifies REQ:null-input-mode)

**Given** no JSON input
**When** the command runs `-n 'type'`
**Then** the output value is `"null"`.

### AC: invalid-json-fails (verifies REQ:invalid-json-error)

**Given** standard input `{"foo": { bar: [] } }`
**When** the command runs filter `.`
**Then** the command exits with failure and reports invalid JSON.

### AC: array-iterator-writes-multiple-values (verifies REQ:one-output-per-result)

**Given** standard input `[{"id":1},{"id":2},{"id":3}]`
**When** the command runs filter `.[] | .id`
**Then** the command writes three output values: `1`, `2`, and `3`.

### AC: compact-output-removes-pretty-whitespace (verifies REQ:compact-output)

**Given** standard input `{"a":{"b":42}}`
**When** the command runs `-c '.'`
**Then** the output is a single compact JSON line `{"a":{"b":42}}`.

### AC: raw-output-removes-string-quotes (verifies REQ:raw-string-output)

**Given** standard input `{"name":"sample"}`
**When** the command runs `-r '.name'`
**Then** the output line is `sample`.

## Open Questions

- Should this package specify file input and query-file flags now, or wait until
  a separate CLI flags feature?
- Which exit-code values should be locked in this draft, if any?

---
*This document follows the https://specscore.md/feature-specification*
