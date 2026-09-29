# AGENTS.md — plugin-matching

Standalone plugin repo for the stateless `matching` check verb
(`verb:matching`). The plugin is a Go module at `candy/plugin-matching/` (module
path `github.com/opencharly/plugin-matching/candy/plugin-matching`); the root
`charly.yml` only declares `discover: candy` so the repo is a project and its
candy is scanned.

Canonical files:

- `candy/plugin-matching/charly.yml` — the `plugin-matching:` candy entity
  (`plugin:` block, `plan:` check).
- `candy/plugin-matching/` — the Go source: `plugin.go`,
  `schema/matching.cue` (the self-contained `#MatchingInput`),
  `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, the per-plugin CUE-schema contract,
  placement. Load before touching the provider or schema.
- `/charly-check:check` — the declarative check-step surface the `matching:`
  verb is authored through (the check verb catalog).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-matching/` — compile the plugin module.
- `go test ./...` in `candy/plugin-matching/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- The changed path is exercised by any check bed composing the `matching:` verb.

## Modify this repo

- Edit the `plugin-matching:` candy entity, the Go source, and
  `schema/matching.cue` **together** — the schema is the single source for the
  verb's `params/` struct.
- Keep the matcher evaluation on the SHARED `sdk.MatchAll` /
  `sdk.MatchValueString` helpers — never re-implement matching (R3).

## Landing

- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Load
  `/charly-internals:git-workflow` before any git/PR action; history lives in
  `CHANGELOG/`. Do not restate its rules here.
