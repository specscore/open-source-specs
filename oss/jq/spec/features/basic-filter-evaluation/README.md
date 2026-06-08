---
format: https://specscore.md/feature-specification
status: Draft
---
# Feature: Basic Filter Evaluation

> [SpecScore.**Studio**](https://specscore.studio): | [Explore](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/basic-filter-evaluation?op=explore) | [Edit](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/basic-filter-evaluation?op=edit) | [Ask question](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/basic-filter-evaluation?op=ask) | [Request change](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/basic-filter-evaluation?op=request-change) |
**Status:** Draft
**Source Ideas:** —

## Summary

The MiniJQ subset evaluates filter expressions against JSON input and emits
deterministic JSON values. This draft covers identity, object field lookup,
array indexing and iteration, pipe composition, object construction, and a small
set of builtins useful for a reimplementation benchmark.

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

#### REQ: array-index

The implementation MUST return the array element at a zero-based numeric index
when the filter uses array index notation such as `.[0]`.

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

### Builtins

#### REQ: length-builtin

The implementation MUST support `length` for arrays and objects.

#### REQ: keys-builtin

The implementation MUST support `keys` for objects and emit the object key list
as a JSON array.

#### REQ: has-function

The implementation MUST support `has(key)` for objects.

#### REQ: map-function

The implementation MUST support `map(filter)` for arrays by applying the filter
to each element and returning an array of results.

#### REQ: select-function

The implementation MUST support `select(predicate)` by emitting the input value
only when the predicate is truthy.

#### REQ: type-builtin

The implementation MUST support `type` and return the jq type name for the input
value as a string.

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

### AC: array-index-returns-first-element (verifies REQ:array-index)

**Given** JSON input `[10,20,30]`
**When** the filter `.[0]` is evaluated
**Then** the emitted JSON value is `10`.

### AC: computed-object-key-builds-object (verifies REQ:object-construction)

**Given** JSON input `{"id":"sample","10":{"b":42}}`
**When** the filter `{(.id): .["10"].b}` is evaluated
**Then** the emitted JSON value is `{"sample":42}`.

### AC: length-counts-array-elements (verifies REQ:length-builtin)

**Given** JSON input `[{"id":1},{"id":2},{"id":3}]`
**When** the filter `length` is evaluated
**Then** the emitted JSON value is `3`.

### AC: keys-lists-object-keys (verifies REQ:keys-builtin)

**Given** JSON input `{"foo":128}`
**When** the filter `keys` is evaluated
**Then** the emitted JSON value is `["foo"]`.

### AC: has-reports-object-key-presence (verifies REQ:has-function)

**Given** JSON input `{"foo":128}`
**When** the filter `has("foo")` is evaluated
**Then** the emitted JSON value is `true`.

### AC: map-applies-filter-to-array (verifies REQ:map-function)

**Given** JSON input `[{"id":1},{"id":2},{"id":3}]`
**When** the filter `map(.id)` is evaluated
**Then** the emitted JSON value is `[1,2,3]`.

### AC: select-emits-matching-value (verifies REQ:select-function)

**Given** JSON input `[{"id":1},{"id":2},{"id":3}]`
**When** the filter `.[] | select(.id == 2) | .id` is evaluated
**Then** the emitted JSON value is `2`.

### AC: type-reports-object (verifies REQ:type-builtin)

**Given** JSON input `{"foo":128}`
**When** the filter `type` is evaluated
**Then** the emitted JSON value is `"object"`.

## Open Questions

- Should missing object fields in this initial slice be specified now, or should
  they wait for a broader null/error behavior feature?
- Is the current MiniJQ subset final, or should some builtins move to a later
  feature once the first benchmark harness exists?

---
*This document follows the https://specscore.md/feature-specification*
