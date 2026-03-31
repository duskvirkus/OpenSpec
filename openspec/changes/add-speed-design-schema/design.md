## Context

OpenSpec ships with one built-in schema: `spec-driven` (proposal → specs → design → tasks). Schemas live at `schemas/<name>/schema.yaml` in the package and are resolved via a three-tier lookup: project-local → user override → package built-in. Adding a new built-in schema requires only a new directory under `schemas/` — no CLI code changes.

The existing `spec-driven` schema demonstrates the full pattern. The `speed-design` schema is a strict subset: same artifact structure, same `apply` contract, just fewer nodes in the dependency graph.

## Goals / Non-Goals

**Goals:**
- Ship `speed-design` as a built-in schema available to all users without configuration
- Two artifacts only: `design` (root) → `tasks` (requires design)
- Templates and instructions scoped to the leaner context — no mentions of proposal or specs
- Apply contract identical to `spec-driven`: requires `tasks`

**Non-Goals:**
- Modifying any existing schema or CLI code
- Adding a `specs` artifact as optional — keep the schema minimal by design
- Supporting per-project or user-level template overrides (that already works via the resolver, nothing new needed)

## Decisions

**Decision: New directory under `schemas/`, no code changes**
The schema resolver already handles multiple built-in schemas by scanning `schemas/*/schema.yaml`. Adding `schemas/speed-design/schema.yaml` is sufficient. Alternative (adding a flag or variant to `spec-driven`) was rejected — a separate schema is cleaner, independently discoverable, and doesn't complicate the existing schema.

**Decision: `design` has `requires: []` (root artifact)**
Without a proposal, design becomes the entry point. The agent is expected to gather context conversationally or from the codebase before writing the design doc.

**Decision: Tasks template references design only**
The `spec-driven` tasks template instructs the agent to "reference specs for what, design for how." The `speed-design` tasks template will instruct the agent to reference design only, which is the sole source of truth in this schema.

**Decision: Copy and adapt templates from `spec-driven`, don't share them**
Templates are small and schema-specific. Sharing would couple schemas together and complicate future independent evolution. Each schema owns its templates.

## Risks / Trade-offs

- [Risk] Users expect `speed-design` to behave like a strict subset of `spec-driven` → Mitigation: keep artifact IDs (`design`, `tasks`) and apply contract identical so skills written for one work with the other
- [Risk] Template instructions still reference proposal/specs by accident → Mitigation: explicit review of all instruction text before shipping

## Open Questions

*(none — implementation is straightforward)*
