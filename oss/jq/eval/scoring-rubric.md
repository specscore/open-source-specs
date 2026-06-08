# jq Scoring Rubric

This rubric is for evaluating a jq-like implementation against the draft
SpecScore package. It is not a benchmark leaderboard.

## Pass Levels

| Level | Meaning |
|---|---|
| Pass | The implementation satisfies every listed acceptance check for the slice. |
| Partial | The implementation satisfies some checks, with failures documented by AC ID. |
| Not evaluated | The implementation was not run against the slice. |
| Out of scope | The check targets behavior outside the implementation's stated scope. |

## Rules

- Compare exact JSON values, not pretty-print whitespace, unless the AC is about output mode.
- For multi-output filters, preserve result order.
- For invalid JSON, do not require exact diagnostic wording in this draft; require failure and an invalid-JSON classification.
- For library checks, record the Go version and `gojq` commit if executable tests are added.
- Do not report aggregate scores until the command and expected output for every AC are reproducible.
