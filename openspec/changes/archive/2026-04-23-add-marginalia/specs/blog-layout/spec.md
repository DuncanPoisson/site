## ADDED Requirements

### Requirement: Blog single page renders marginalia
A blog post's single page SHALL render any front-matter `marginalia` entries according to the `marginalia` capability. A post without marginalia SHALL render exactly as it does today.

#### Scenario: Post with marginalia
- **WHEN** a blog post under `content/blog/*.md` has a non-empty `marginalia` front-matter list
- **THEN** the rendered single page contains the configured notes per the `marginalia` capability requirements (desktop rail and mobile inline)

#### Scenario: Post without marginalia
- **WHEN** a blog post has no `marginalia` front-matter
- **THEN** the rendered single page is byte-identical (modulo whitespace) to the same post built before this change
