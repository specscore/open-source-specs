# jq Acceptance Checks

These checks map concrete examples to the draft SpecScore AC IDs under
[`../spec/features/`](../spec/features/).

The AC IDs come from [SpecScore feature specifications](https://specscore.md/feature-specification).

The command examples assume a `gojq` binary built from
[`itchyny/gojq`](https://github.com/itchyny/gojq).

## Basic Filter Evaluation

| AC | Command | Expected stdout |
|---|---|---|
| `basic-filter-evaluation/AC:identity-preserves-input` | `gojq -c '.' fixtures/basic-object.json` | `{"foo":128}` |
| `basic-filter-evaluation/AC:dot-field-returns-value` | `gojq '.foo' fixtures/basic-object.json` | `128` |
| `basic-filter-evaluation/AC:nested-dot-field-returns-value` | `gojq '.a.b' fixtures/nested-object.json` | `42` |
| `basic-filter-evaluation/AC:array-pipe-emits-each-id` | `gojq '.[] | .id' fixtures/array-of-ids.json` | `1`, `2`, `3` on separate output lines |
| `basic-filter-evaluation/AC:array-index-returns-first-element` | `gojq '.[0]' fixtures/array-values.json` | `10` |
| `basic-filter-evaluation/AC:computed-object-key-builds-object` | `gojq -c '{(.id): .["10"].b}' fixtures/computed-key.json` | `{"sample":42}` |
| `basic-filter-evaluation/AC:length-counts-array-elements` | `gojq 'length' fixtures/array-of-ids.json` | `3` |
| `basic-filter-evaluation/AC:keys-lists-object-keys` | `gojq -c 'keys' fixtures/basic-object.json` | `["foo"]` |
| `basic-filter-evaluation/AC:has-reports-object-key-presence` | `gojq 'has("foo")' fixtures/basic-object.json` | `true` |
| `basic-filter-evaluation/AC:map-applies-filter-to-array` | `gojq -c 'map(.id)' fixtures/array-of-ids.json` | `[1,2,3]` |
| `basic-filter-evaluation/AC:select-emits-matching-value` | `gojq '.[] | select(.id == 2) | .id' fixtures/array-of-ids.json` | `2` |
| `basic-filter-evaluation/AC:type-reports-object` | `gojq -r 'type' fixtures/basic-object.json` | `object` |

## Input and Output Modes

| AC | Command | Expected result |
|---|---|---|
| `input-and-output-modes/AC:stdin-json-field-query` | `gojq '.foo' fixtures/basic-object.json` | stdout `128` |
| `input-and-output-modes/AC:null-input-computes-value` | `gojq -n '1 + 2'` | stdout `3` |
| `input-and-output-modes/AC:invalid-json-fails` | `gojq '.' fixtures/invalid-object.json` | non-zero exit; stderr reports invalid JSON |
| `input-and-output-modes/AC:array-iterator-writes-multiple-values` | `gojq '.[] | .id' fixtures/array-of-ids.json` | stdout lines `1`, `2`, `3` |
| `input-and-output-modes/AC:compact-output-removes-pretty-whitespace` | `gojq -c '.' fixtures/nested-object.json` | stdout `{"a":{"b":42}}` |
| `input-and-output-modes/AC:raw-output-removes-string-quotes` | `gojq -r '.name' fixtures/name-object.json` | stdout `sample` |

## Go Library API

Library checks are prose-only in this draft. They should become Go test fixtures
once the repository has an evaluation harness.

| AC | Check |
|---|---|
| `go-library-api/AC:parse-and-run-field-query` | Parse `.foo`, run against `map[string]any{"foo": 128}`, assert iterator emits `128`. |
| `go-library-api/AC:compile-reuses-query` | Compile `.foo` once, run against two maps, assert each result follows its input. |
| `go-library-api/AC:custom-input-shape-rejected-or-unspecified` | Document that callers should marshal custom structs to JSON-shaped `any` values first. |
| `go-library-api/AC:iterator-error-is-observable` | Run an error-producing query and assert an emitted iterator value satisfies `error`. |
| `go-library-api/AC:variables-option-binds-run-values` | Compile with `WithVariables([]string{"$x"})`, run with a variable value, assert `$x` is available. |
