---
format: https://specscore.md/feature-specification
status: Draft
---
# Feature: Go Library API

> [SpecScore.**Studio**](https://specscore.studio): | [Explore](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/go-library-api?op=explore) | [Edit](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/go-library-api?op=edit) | [Ask question](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/go-library-api?op=ask) | [Request change](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/go-library-api?op=request-change) |
**Status:** Draft
**Source Ideas:** —

## Summary

gojq can be embedded as a Go library. This draft specifies the small public API
surface needed to parse a query, compile it, run it against decoded JSON-shaped
Go values, and consume emitted results or errors.

## Problem

The reference package should reflect that `gojq` is not only a command. A Go
implementation target needs behavior that can be checked at the library level,
where command-line serialization and process exit behavior are irrelevant.

## Behavior

### Query Parsing

#### REQ: parse-query

The library MUST expose `Parse(string)` to parse a jq query string into a query
value or return a parse error.

#### REQ: parse-error-position

When query parsing fails, the parse error SHOULD expose enough position
information for callers to display the failing token location.

### Query Execution

#### REQ: run-query

The parsed query MUST expose `Run(input)` and `RunWithContext(context, input)`
execution paths.

#### REQ: compile-query

The library MUST expose `Compile(query, options...)` to produce reusable code
that can run against multiple input values.

#### REQ: json-shaped-input-types

The library input value MUST be JSON-shaped Go data: arrays as `[]any`, objects
as `map[string]any`, and scalar JSON values as their decoded Go equivalents.

### Iteration and Errors

#### REQ: result-iterator

Execution MUST return an iterator whose `Next()` method returns a value and a
boolean indicating whether iteration continues.

#### REQ: iterator-can-emit-errors

The iterator MAY emit errors as values, and callers MUST be able to distinguish
those errors from ordinary JSON results.

### Compiler Options

#### REQ: compiler-options

Compilation MUST support options for module loading, environment loading,
variables, custom functions, custom iterator functions, and input iterators.

## Acceptance Criteria

### AC: parse-and-run-field-query (verifies REQ:parse-query, REQ:run-query)

**Given** query string `.foo` and input `map[string]any{"foo": 128}`
**When** a caller parses the query and runs it
**Then** the iterator emits `128`.

### AC: compile-reuses-query (verifies REQ:compile-query)

**Given** parsed query `.foo`
**When** a caller compiles it once and runs the compiled code against two inputs
**Then** each run evaluates the same query against that run's input.

### AC: custom-input-shape-rejected-or-unspecified (verifies REQ:json-shaped-input-types)

**Given** a custom struct or a map with non-`any` value type
**When** a caller passes it directly as query input
**Then** behavior is outside the supported input contract; callers should
marshal/unmarshal to JSON-shaped values first.

### AC: iterator-error-is-observable (verifies REQ:iterator-can-emit-errors)

**Given** a query that can produce a runtime error
**When** a caller consumes iterator values
**Then** an emitted error can be detected by type-checking the returned value.

### AC: variables-option-binds-run-values (verifies REQ:compiler-options)

**Given** a query that references `$x`
**When** a caller compiles with `WithVariables([]string{"$x"})` and runs with a
value for `$x`
**Then** the query can use the supplied variable value during execution.

## Open Questions

- Should this package include executable Go test snippets, or keep library
  checks as prose until the first automated harness exists?
- Should context cancellation behavior be specified in this feature or in a
  later performance/safety feature?

---
*This document follows the https://specscore.md/feature-specification*
