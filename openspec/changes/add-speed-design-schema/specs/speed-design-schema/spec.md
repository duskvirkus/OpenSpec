## ADDED Requirements

### Requirement: speed-design schema is available as a built-in schema
The system SHALL include a `speed-design` schema alongside `spec-driven` in the package's built-in schemas directory.

#### Scenario: User lists schemas and sees speed-design
- **WHEN** the user runs `openspec schemas`
- **THEN** `speed-design` appears in the output with its description

#### Scenario: User creates a change with the speed-design schema
- **WHEN** the user runs `openspec new change <name> --schema speed-design`
- **THEN** the change is created with schema recorded as `speed-design` in `.openspec.yaml`

### Requirement: speed-design schema defines design and tasks artifacts only
The `speed-design` schema SHALL define exactly two artifacts: `design` and `tasks`. It SHALL NOT define `proposal` or `specs` artifacts.

#### Scenario: Status shows only design and tasks
- **WHEN** the user runs `openspec status --change <name> --json` on a speed-design change
- **THEN** the `artifacts` array contains exactly `design` and `tasks`

#### Scenario: design is the root artifact
- **WHEN** the user runs `openspec status --change <name> --json` on a new speed-design change
- **THEN** `design` has `status: "ready"` and no `missingDeps`

#### Scenario: tasks is blocked until design is complete
- **WHEN** design has not yet been created
- **THEN** `tasks` has `status: "blocked"` with `missingDeps: ["design"]`

#### Scenario: tasks becomes ready after design is created
- **WHEN** `design.md` exists in the change directory
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
The `speed-design` schema SHALL include `design.md` and `tasks.md` templates appropriate for changes where requirements are already understood, with no references to proposal or specs artifacts.

#### Scenario: Design template contains no proposal reference
- **WHEN** the agent fetches instructions for the `design` artifact
- **THEN** the returned `template` field does not reference proposal.md

#### Scenario: Tasks template is identical in format to spec-driven
- **WHEN** the agent fetches instructions for the `tasks` artifact on a speed-design change
- **THEN** the template uses `- [ ] N.M Task description` checkbox format
