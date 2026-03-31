## Context

OpenSpec ships with one built-in schema: `spec-driven` (proposal → specs → design → tasks). Schemas live at `schemas/<name>/schema.yaml` in the package and are resolved via a three-tier lookup: project-local → user override → package built-in. Adding a new built-in schema requires only a new directory under `schemas/` — no CLI code changes.

The existing `spec-driven` schema defines specs as dependent on proposal. The `speed-design` schema inverts this: design is the root artifact, and specs are derived from the design rather than from a proposal. This preserves testable requirements (specs) while eliminating the proposal as a prerequisite.

## Goals / Non-Goals

**Goals:**
- Ship `speed-design` as a built-in schema available to all users without configuration
- Three artifacts: `design` (root) → `specs` (requires design) → `tasks` (requires specs)
- Templates and instructions scoped to the leaner context — no proposal references
- Apply contract identical to `spec-driven`: requires `tasks`

**Non-Goals:**
- Modifying any existing schema or CLI code
- Making specs optional — they remain a required artifact, just driven by design instead of proposal
- Supporting per-project or user-level template overrides (that already works via the resolver, nothing new needed)

## Decisions

**Decision: New directory under `schemas/`, no code changes**
The schema resolver already handles multiple built-in schemas by scanning `schemas/*/schema.yaml`. Adding `schemas/speed-design/schema.yaml` is sufficient. Alternative (adding a flag or variant to `spec-driven`) was rejected — a separate schema is cleaner, independently discoverable, and doesn't complicate the existing schema.

**Decision: `design` has `requires: []` (root artifact)**
Without a proposal, design becomes the entry point. The agent is expected to gather context conversationally or from the codebase before writing the design doc.

**Decision: `specs` requires `design`, not the other way around**
In `spec-driven`, specs capture WHAT based on a proposal. Here, the design already captures WHAT and HOW — specs formalize the requirements in testable language from the design. This is the key structural difference from `spec-driven`.

**Decision: `tasks` requires `specs` (same dependency as `spec-driven`)**
Tasks are grounded in specs, which remain the testable contract. The chain is: design (intent) → specs (requirements) → tasks (work).

**Decision: Copy and adapt templates from `spec-driven`, don't share them**
Templates are small and schema-specific. Sharing would couple schemas together and complicate future independent evolution. Each schema owns its templates.

## Risks / Trade-offs

- [Risk] Agents may still write specs in the `spec-driven` delta format (ADDED/MODIFIED sections) when using `speed-design` → Mitigation: spec template and instruction explicitly describe full specs, not delta format
- [Risk] Template instructions reference proposal by accident → Mitigation: explicit review of all instruction text before shipping

## Open Questions

*(none — implementation is straightforward)*
