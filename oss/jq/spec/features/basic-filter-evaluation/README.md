---
format: https://specscore.md/feature-specification
status: Draft
---
# Feature: Basic Filter Evaluation

> [SpecScore.**Studio**](https://specscore.studio): | [Explore](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/basic-filter-evaluation?op=explore) | [Edit](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/basic-filter-evaluation?op=edit) | [Ask question](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/basic-filter-evaluation?op=ask) | [Request change](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/basic-filter-evaluation?op=request-change) |
**Status:** Draft
**Source Ideas:** —

## Summary

The jq implementation evaluates filter expressions against JSON input and emits
deterministic JSON values. This draft covers a small, source-backed slice from
the `gojq` README examples: identity, object field lookup, array iteration, pipe
composition, and object construction.

## Problem

Without a small executable behavior slice, a reference jq specification either
becomes too broad to review or too vague to test. This feature defines the first
filter operations that acceptance checks can verify exactly.

## Behavior

### Identity and Field Lookup

#### REQ: identity-filter

The implementation MUST return the input value unchanged when the filter is `.`.

#### REQ: object-field-lookup

The implementation MUST return the value of a named object field when the filter
uses dot notation such as `.foo`.

#### REQ: nested-object-field-lookup

The implementation MUST support chained object field lookup such as `.a.b`.

### Arrays and Pipes

#### REQ: array-iterator

The implementation MUST emit one result per array element when the filter uses
the array iterator `.[]`.

#### REQ: pipe-composition

The implementation MUST feed each result from the left side of `|` into the
filter on the right side.

### Object Construction

#### REQ: object-construction

The implementation MUST support object construction where the output object key
and value are produced from filter expressions, such as `{(.id): .["10"].b}`.

## Acceptance Criteria

### AC: identity-preserves-input (verifies REQ:identity-filter)

**Given** JSON input `{"foo":128}`
**When** the filter `.` is evaluated
**Then** the emitted JSON value is `{"foo":128}`.

### AC: dot-field-returns-value (verifies REQ:object-field-lookup)

**Given** JSON input `{"foo":128}`
**When** the filter `.foo` is evaluated
**Then** the emitted JSON value is `128`.

### AC: nested-dot-field-returns-value (verifies REQ:nested-object-field-lookup)

**Given** JSON input `{"a":{"b":42}}`
**When** the filter `.a.b` is evaluated
**Then** the emitted JSON value is `42`.

### AC: array-pipe-emits-each-id (verifies REQ:array-iterator, REQ:pipe-composition)

**Given** JSON input `[{"id":1},{"id":2},{"id":3}]`
**When** the filter `.[] | .id` is evaluated
**Then** the emitted JSON values are `1`, `2`, and `3` in that order.

### AC: computed-object-key-builds-object (verifies REQ:object-construction)

**Given** JSON input `{"id":"sample","10":{"b":42}}`
**When** the filter `{(.id): .["10"].b}` is evaluated
**Then** the emitted JSON value is `{"sample":42}`.

## Open Questions

- Should missing object fields in this initial slice be specified now, or should
  they wait for a broader null/error behavior feature?

---
*This document follows the https://specscore.md/feature-specification*
