## 1. Schema directory and YAML

- [ ] 1.1 Create `schemas/speed-design/` directory
- [ ] 1.2 Write `schemas/speed-design/schema.yaml` with `design` and `tasks` artifacts, correct `requires` wiring, and `apply` config
- [ ] 1.3 Verify `openspec schemas` lists `speed-design` with correct description

## 2. Templates

- [ ] 2.1 Write `schemas/speed-design/templates/design.md` — adapted from `spec-driven`, no proposal/specs references
- [ ] 2.2 Write `schemas/speed-design/templates/tasks.md` — identical checkbox format to `spec-driven`, references design only
- [ ] 2.3 Write `schemas/speed-design/templates/spec.md` placeholder (required by resolver even if unused — verify if needed, skip if not)

## 3. Verification

- [ ] 3.1 Create a test change with `--schema speed-design` and confirm status shows only `design` and `tasks`
- [ ] 3.2 Confirm `design` is `ready` on a fresh change and `tasks` is `blocked` on `design`
- [ ] 3.3 Confirm `tasks` becomes `ready` after creating `design.md`
- [ ] 3.4 Confirm `openspec instructions apply` works once `tasks.md` exists
- [ ] 3.5 Delete the test change after verification
