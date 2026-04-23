## ADDED Requirements

### Requirement: Journal entry single page renders marginalia
A journal entry's single page SHALL render any front-matter `marginalia` entries according to the `marginalia` capability. An entry without marginalia SHALL render exactly as it does today.

#### Scenario: Entry with marginalia
- **WHEN** a journal entry under `content/journal/<project>/*.md` has a non-empty `marginalia` front-matter list
- **THEN** the rendered single page contains the configured notes per the `marginalia` capability requirements (desktop rail and mobile inline)

#### Scenario: Entry without marginalia
- **WHEN** a journal entry has no `marginalia` front-matter
- **THEN** the rendered single page is byte-identical (modulo whitespace) to the same entry built before this change

#### Scenario: Project landing page is out of scope
- **WHEN** a project landing page (`content/journal/<project>/_index.md`) declares a `marginalia` front-matter list
- **THEN** the rendered landing page contains no marginalia output (v1 limits scope to entry pages, not project landings)
