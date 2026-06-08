# Open Source Specs

Unofficial SpecScore specifications for established open-source projects.

This repository collects draft specifications for public projects so engineers,
tool builders, and agents can inspect concrete examples of SpecScore applied to
real software. The specs are not upstream-maintainer endorsements unless a
project explicitly says so.

For readers new to jq: jq is a command-line JSON processor and query language
used to filter, transform, and format JSON data.

## Contents

| Path | Project | Status |
|---|---|---|
| [`oss/jq/`](oss/jq/README.md) | MiniJQ subset behavior for jq, the command-line JSON processor/query language, based on the Go implementation [`itchyny/gojq`](https://github.com/itchyny/gojq) | Draft |

## Rules

- Specs must cite the source project and the source material used.
- Specs must not imply upstream endorsement.
- Specs should prefer small, correct behavior slices over broad summaries.
- Acceptance checks should map back to requirement and AC IDs.
- Public claims should wait until the relevant spec is lint-clean and reviewable.

## Layout

```text
oss/
  README.md
  <project>/
    README.md
    spec/
      features/
    eval/
      acceptance-checks.md
      scoring-rubric.md
      fixtures/
```

---
This repository follows the public artifact strategy in
[`specscore/marketing`](https://github.com/specscore/marketing).
