# VibeBar Model Pricing

Community-maintained pricing supplements for
[Vibe Bar](https://github.com/AstroQore/vibe-bar).

This repository is intentionally small. Vibe Bar checks its primary public
catalogs first, then uses `pricing.json` to fill models those catalogs omit.
A few entries are marked as corrections and outrank the public catalogs (see
below). Local user overrides in Vibe Bar always take precedence.

## Priority

Vibe Bar merges its sources from lowest to highest priority:

1. bundled offline table
2. gap-filling entries from this repository (no `override`, or
   `"override": false`)
3. Portkey Models
4. models.dev
5. LiteLLM
6. correction entries from this repository (`"override": true`)
7. local overrides the user sets in Vibe Bar's Settings

A gap filler only prices a model no public catalog lists. A correction
replaces whatever the public catalogs say for that provider and model: its
resolved rate card (the `inherits` target, or its own `pricing` block when
the target cannot be resolved) is applied as a whole, so a field it leaves out
is not filled in from the public entry being corrected. `inherits` on either
kind of entry resolves against the merged public catalogs.

Use `override` sparingly, only when a public catalog carries a wrong price for
a model and the entry's `source` explains why. Vibe Bar builds released before
the key existed ignore it and treat the entry as an ordinary gap filler.

## Data format

- Prices are USD per one million tokens.
- `inherits` aliases a model to another resolved model. The embedded `pricing`
  object is an offline fallback and a reviewable snapshot.
- `threshold` replaces the base rates when a request reaches the stated input
  context size.
- `fast` contains explicit priority-service rates when they differ.
- `override: true` turns an entry into a correction (see Priority).

Validate changes against `schema.json` before merging. Every pricing change
should include a public source or an explanation of the alias relationship.

## Current supplement

- `gpt-daybreak-blue-latest` inherits the complete `gpt-5.6-sol` rate card.
- `grok-composer-2.5-fast` uses Cursor's published Fast rate of $3/M
  input and $15/M output. Cursor does not publish a separate cache-read rate
  on the cited model page, so this supplement does not invent one.

## Current corrections

- `codex-auto-review` inherits the complete `gpt-5.6-luna` rate card
  ($0.20/M input, $1.20/M output, $0.02/M cache read, $0.25/M cache write,
  long-context and fast tiers included). OpenAI announced on 2026-07-30 that
  Codex Auto-review runs on GPT-5.6 Luna. Portkey lists `codex-auto-review`
  on its own at $2.50/M input and $15/M output with no cache-read rate, which
  bills every cached token at the full input rate, so this entry is marked
  `"override": true` to take precedence over it.

## License

MIT
