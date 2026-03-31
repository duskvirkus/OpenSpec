## ADDED Requirements

### Requirement: speed-design schema is available as a built-in schema
The system SHALL include a `speed-design` schema alongside `spec-driven` in the package's built-in schemas directory.

#### Scenario: User lists schemas and sees speed-design
- **WHEN** the user runs `openspec schemas`
- **THEN** `speed-design` appears in the output with its description

#### Scenario: User creates a change with the speed-design schema
- **WHEN** the user runs `openspec new change <name> --schema speed-design`
- **THEN** the change is created with schema recorded as `speed-design` in `.openspec.yaml`

### Requirement: speed-design schema defines design, specs, and tasks artifacts
The `speed-design` schema SHALL define exactly three artifacts: `design`, `specs`, and `tasks`. It SHALL NOT define a `proposal` artifact.

#### Scenario: Status shows only design, specs, and tasks
- **WHEN** the user runs `openspec status --change <name> --json` on a speed-design change
- **THEN** the `artifacts` array contains exactly `design`, `specs`, and `tasks`

#### Scenario: design is the root artifact
- **WHEN** the user runs `openspec status --change <name> --json` on a new speed-design change
- **THEN** `design` has `status: "ready"` and no `missingDeps`

#### Scenario: specs is blocked until design is complete
- **WHEN** `design.md` has not yet been created
- **THEN** `specs` has `status: "blocked"` with `missingDeps: ["design"]`

#### Scenario: specs becomes ready after design is created
- **WHEN** `design.md` exists in the change directory
- **THEN** `specs` has `status: "ready"`

#### Scenario: tasks is blocked until specs are complete
- **WHEN** specs have not yet been created
- **THEN** `tasks` has `status: "blocked"` with `missingDeps: ["specs"]`

#### Scenario: tasks becomes ready after specs are created
- **WHEN** at least one spec file exists under the change directory
- **THEN** `tasks` has `status: "ready"`

### Requirement: speed-design schema apply phase requires tasks
The `speed-design` schema's `apply` configuration SHALL require `tasks` before implementation begins, matching the `spec-driven` schema's apply contract.

#### Scenario: Apply is blocked without tasks
- **WHEN** `tasks.md` does not exist
- **THEN** `openspec instructions apply` reports the change is blocked on `tasks`

#### Scenario: Apply proceeds when tasks exists
- **WHEN** `tasks.md` exists
- **THEN** `openspec instructions apply` returns apply instructions with task list

### Requirement: speed-design schema ships with tailored templates
The `speed-design` schema SHALL include `design.md`, `spec.md`, and `tasks.md` templates appropriate for changes where design drives requirements, with no references to a proposal artifact.

#### Scenario: Design template contains no proposal reference
- **WHEN** the agent fetches instructions for the `design` artifact
- **THEN** the returned `template` and `instruction` fields do not reference proposal.md

#### Scenario: Spec template describes full specs, not delta format
- **WHEN** the agent fetches instructions for the `specs` artifact
- **THEN** the `instruction` field describes writing full requirement specs (not ADDED/MODIFIED/REMOVED delta format)
- **THEN** the `instruction` field references the design document as the source of truth

#### Scenario: Tasks template is identical in format to spec-driven
- **WHEN** the agent fetches instructions for the `tasks` artifact on a speed-design change
- **THEN** the template uses `- [ ] N.M Task description` checkbox format
