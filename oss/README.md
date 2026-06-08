# OSS Specifications

Draft SpecScore specifications for open-source projects.

Each package uses [SpecScore](https://specscore.md) to describe behavior as
reviewable requirements and acceptance criteria.

| Project | Spec package | Source project | Status |
|---|---|---|---|
| jq / MiniJQ | [`jq/`](jq/README.md) | [`itchyny/gojq`](https://github.com/itchyny/gojq), a pure Go implementation of jq, the command-line JSON processor/query language | Draft |

## Inclusion Criteria

A project belongs here when it is useful as a reference specification target:

- it has behavior that can be checked with deterministic inputs and outputs;
- the source project is public and inspectable;
- the specification can cite concrete source material;
- the first slice can stay small enough to review.
