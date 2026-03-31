## Why

The default `spec-driven` schema requires four artifacts (proposal → specs → design → tasks), which is thorough but heavyweight for small or well-understood changes. Users who already know what they want to build often find the proposal and specs phases redundant with a good design document. A leaner schema that goes straight to design and tasks reduces friction without losing the structured thinking that makes the workflow valuable.

## What Changes

- Add a new built-in schema `speed-design` with two artifacts: `design → tasks`
- The schema is a first-class alternative to `spec-driven`, shipped with the package
- `design` has no dependencies (root artifact)
- `tasks` requires `design`
- `apply` requires `tasks` (same as `spec-driven`)
- New templates for `design` and `tasks` tailored to the leaner context (no proposal/specs references)

## Capabilities

### New Capabilities

- `speed-design-schema`: A built-in workflow schema that skips proposal and specs phases, going directly from design to tasks. Suitable for small changes, spikes, and situations where requirements are already understood.

### Modified Capabilities

*(none)*

## Impact

- New directory: `schemas/speed-design/` with `schema.yaml` and `templates/`
- No changes to existing `spec-driven` schema or any CLI code
- No breaking changes — purely additive
