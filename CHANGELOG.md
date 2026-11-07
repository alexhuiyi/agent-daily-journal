# Changelog

All notable changes to this skill. Semver: the skill contract (SKILL.md) and the scripts move
together.

## [0.1.0] — 2026-10-26

First packaged release, extracted from a working personal setup and generalised.

### Added

- `scripts/journal_collect.py` — config-file driven collector (`--config`, `--only`, `--json`,
  `--state-db`, `--workspace`, `--day-start-hour`, `--max-windows`), with English defaults and a
  built-in Chinese label set (`"language": "zh"`). Environment variables still win over the config.
- `scripts/journal_lint.py` — coverage linter: distinctive-token matching (strong evidence from
  headings and key-point lines, weak from outcome prose), optional local embedding fallback for
  paraphrases, marker check, event-scaled character ceilings, advisory imagery / wrap-up / reuse
  warnings (`--strict` promotes them).
- `tests/` — 28 stdlib unittest cases plus synthetic fixtures: a four-session digest, an entry naming
  all four events, and an entry that narrates only one. The fixtures use invented tasks only, so the
  published repo carries no user data.
- `assets/style.example.md`, `assets/style.zh.example.md` — style templates using fill-in slots and
  a banned-tone list, with **no copyable example sentence**.
- `references/deployment.md` — cron wiring via `cronjob_manage`, first-run checklist, troubleshooting.
- `examples/cron-prompt.md` — the ten-line writer contract.

### Changed from the private setup

- Prompt: dropped "one sentence about what was done" (measured 0 of 4 events named) in favour of a
  per-`###` coverage rule, a feeling clause, and an event-scaled ceiling.
- Style: the single authority on voice; the cron prompt no longer restates tone, length or
  forbidden items, and no longer carries an example entry.
- Length: 300 characters for 1–2 events, 420 for 3–5, no floor. Real entries used ~60 characters of
  a 300 ceiling, so length was never the binding constraint — structure was.

### Known limits

- Coverage is lexical first: a paraphrase with zero token overlap and no local embedding model will
  show up as a false MISS. Keep Ollama up, or lower `--threshold`.
- `invented_time` warnings cannot be verified against the digest clock; they stay advisory even
  under `--strict`.
- Collector reads Hermes' own `state.db` schema. A schema change upstream breaks `fetch_sessions()`
  loudly rather than silently — which is the failure mode we want.
