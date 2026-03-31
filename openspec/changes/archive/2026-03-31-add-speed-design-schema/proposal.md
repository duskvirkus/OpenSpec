## Why

The default `spec-driven` schema requires four artifacts (proposal → specs → design → tasks), which is thorough but heavyweight for small or well-understood changes. Users who already know what they want to build often find the proposal phase redundant — a design document can serve as the entry point, with specs flowing from it rather than from a proposal. A leaner schema that starts at design, derives specs from it, then produces tasks reduces friction without losing structured thinking or testable requirements.

## What Changes

- Add a new built-in schema `speed-design` with three artifacts: `design → specs → tasks`
- The schema is a first-class alternative to `spec-driven`, shipped with the package
- `design` has no dependencies (root artifact) — the agent gathers context conversationally or from the codebase
- `specs` requires `design` — requirements are derived from the design, not a proposal
- `tasks` requires `specs` — implementation tasks are grounded in the specs
- `apply` requires `tasks` (same as `spec-driven`)
- New templates for `design`, `specs`, and `tasks` tailored to the leaner context (no proposal references)

## Capabilities

### New Capabilities

- `speed-design-schema`: A built-in workflow schema that skips the proposal phase, starting from a design document and deriving specs and tasks from it. Suitable for changes where intent is already clear and the design doc can serve as the authoritative starting point.

### Modified Capabilities

*(none)*

## Impact

- New directory: `schemas/speed-design/` with `schema.yaml` and `templates/`
- No changes to existing `spec-driven` schema or any CLI code
- No breaking changes — purely additive
