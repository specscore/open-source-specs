---
format: https://specscore.md/feature-specification
status: Draft
---
# Feature: Version Subcommand

> [SpecScore.**Studio**](https://specscore.studio): | [Explore](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/version?op=explore) | [Edit](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/version?op=edit) | [Ask question](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/version?op=ask) | [Request change](https://specscore.studio/app/github.com/specscore/open-source-specs/spec/features/cli/version?op=request-change) |
**Status:** Draft
**Source Ideas:** —

## Summary

`minijq version`, `minijq --version`, and `minijq -v` report implementation
version information. The subcommand is for humans; the flags are for scripts.

This document follows the [SpecScore feature specification](https://specscore.md/feature-specification).

## Problem

The benchmark may produce multiple implementations and revisions. Tests, result
reports, and bug reports need a stable way to identify the build under test.

## Behavior

### Version Surfaces

#### REQ: version-subcommand

`minijq version` MUST print a single human-readable line beginning with
`minijq ` and ending with a newline.

#### REQ: version-flag

`minijq --version` MUST print only the version token on a single line. The
output MUST NOT include extra prose.

#### REQ: short-version-flag

`minijq -v` MUST behave the same as `minijq --version`.

#### REQ: dev-placeholder

When the implementation has no injected release version, the version token MAY
be `dev`.

## Acceptance Criteria

### AC: version-subcommand-prints-human-line

**Given** an installed MiniJQ implementation
**When** the user runs `minijq version`
**Then** stdout starts with `minijq ` and ends with a newline.

### AC: version-flag-prints-token

**Given** an installed MiniJQ implementation
**When** the user runs `minijq --version`
**Then** stdout contains one version token and no extra prose.

### AC: short-version-flag-matches-long

**Given** an installed MiniJQ implementation
**When** the user runs `minijq -v`
**Then** stdout matches `minijq --version`.

## Open Questions

- Should benchmark implementations use a common version format such as
  `<language>-dev`, or is plain `dev` sufficient before releases exist?

---
*This document follows the https://specscore.md/feature-specification*
