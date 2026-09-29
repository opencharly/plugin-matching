# plugin-matching

The `matching` check verb for [opencharly/charly](https://github.com/opencharly/charly) —
pure in-process value matching with no target probe.

The verb coerces `plugin_input.matching` to a string and asserts every
`plugin_input.contains` goss-style matcher against it via the shared
`sdk.MatchAll` / `sdk.MatchValueString` helpers. It is a STATELESS provider, so
it serves itself over the `pb Invoke` envelope in BOTH placements (compiled-in OR
out-of-process) — the matcher analogue of `candy/plugin-example`'s `exampleprobe`.

## What it provides

| Capability | Surface |
|---|---|
| `verb:matching` | the declarative `matching:` check step any candy or box can bake into its plan |

## The verb

An authored `matching: <value>` step (scalar sugar) or
`matching: {matching: …, contains: …}` (map form). The `matching`-exclusive
fields live in the plugin's own `#MatchingInput` (`schema/matching.cue`).

| Field | Meaning |
|---|---|
| `matching` | the value to match (the verb discriminator) |
| `contains` | a goss-style matcher list asserted against the value |

```yaml
- check: the value carries the expected substrings
  id: matching-ok
  matching:
      matching: charly-candy-factory
      contains:
          - charly-candy-factory
          - contains: charly
  context: [runtime]
```

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-matching/candy/plugin-matching:<tag>'
```

## Layout

- `candy/plugin-matching/` — the plugin module: `plugin.go`,
  `schema/matching.cue` (the self-contained `#MatchingInput`),
  `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `candy/plugin-matching/charly.yml` — the `plugin-matching:` candy entity.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-check:check` — the check verb catalog. This candy
  carries no `skill:` entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
