---
format: https://specscore.md/feature-specification
status: Draft
---
# Feature: Check Subcommand

> [SpecScore.**Studio**](https://specscore.studio): | [Explore](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/check?op=explore) | [Edit](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/check?op=edit) | [Ask question](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/check?op=ask) | [Request change](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/check?op=request-change) |
**Status:** Draft
**Source Ideas:** —

## Summary

`minijq check` validates whether a query is accepted by the documented MiniJQ
subset without reading JSON input or evaluating results.

This document follows the [SpecScore feature specification](https://specscore.md/feature-specification).

## Problem

Tests and tooling need a way to exercise parser behavior independently from JSON
input parsing and evaluator behavior.

## Behavior

### Synopsis

```text
minijq check <query>
```

### Query Validation

#### REQ: check-query-argument

`check` MUST require exactly one query argument unless `--help` is requested.
Missing or extra query arguments MUST exit `2`.

#### REQ: valid-query-exits-zero

When the query is valid for the documented MiniJQ subset, `check` MUST exit `0`.

#### REQ: invalid-query-exits-two

When the query is invalid or outside the documented MiniJQ subset, `check` MUST
exit `2` and write a useful query error to stderr.

#### REQ: no-input-read

`check` MUST NOT read stdin or require input files.

## Acceptance Criteria

### AC: check-valid-query

**Given** query `.foo`
**When** the user runs `minijq check '.foo'`
**Then** the command exits `0`.

### AC: check-invalid-query

**Given** query `.foo & .bar`
**When** the user runs `minijq check '.foo & .bar'`
**Then** the command exits `2` and writes a query error to stderr.

### AC: check-does-not-read-json

**Given** no JSON input
**When** the user runs `minijq check '.[] | .id'`
**Then** the command exits based only on query validity.

## Open Questions

- Should `check` print a success message, or should successful validation remain
  silent for shell scripting?

---
*This document follows the https://specscore.md/feature-specification*
