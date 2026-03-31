## 1. Schema directory and YAML

- [x] 1.1 Create `schemas/speed-design/` directory
- [x] 1.2 Write `schemas/speed-design/schema.yaml` with `design`, `specs`, and `tasks` artifacts — `design` as root, `specs` requires `design`, `tasks` requires `specs`, `apply` requires `tasks`
- [x] 1.3 Verify `openspec schemas` lists `speed-design` with correct description

## 2. Templates

- [x] 2.1 Write `schemas/speed-design/templates/design.md` — adapted from `spec-driven`, no proposal references, framed as the entry point for the change
- [x] 2.2 Write `schemas/speed-design/templates/spec.md` — full requirement specs (not delta format), instruction references design as source of truth
- [x] 2.3 Write `schemas/speed-design/templates/tasks.md` — identical checkbox format to `spec-driven`, references specs and design

## 3. Verification

- [x] 3.1 Create a test change with `--schema speed-design` and confirm status shows `design`, `specs`, and `tasks`
- [x] 3.2 Confirm `design` is `ready` on a fresh change, `specs` and `tasks` are `blocked`
- [x] 3.3 Confirm `specs` becomes `ready` after creating `design.md`
- [x] 3.4 Confirm `tasks` becomes `ready` after creating a spec file
- [x] 3.5 Confirm `openspec instructions apply` works once `tasks.md` exists
- [x] 3.6 Delete the test change after verification
