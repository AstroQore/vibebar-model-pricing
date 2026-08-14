# VibeBar Model Pricing

Community-maintained pricing supplements for
[Vibe Bar](https://github.com/AstroQore/vibe-bar).

This repository is intentionally small. Vibe Bar checks its primary public
catalogs first, then uses `pricing.json` to fill models those catalogs omit.
Local user overrides in Vibe Bar always take precedence.

## Data format

- Prices are USD per one million tokens.
- `inherits` aliases a model to another resolved model. The embedded `pricing`
  object is an offline fallback and a reviewable snapshot.
- `threshold` replaces the base rates when a request reaches the stated input
  context size.
- `fast` contains explicit priority-service rates when they differ.

Validate changes against `schema.json` before merging. Every pricing change
should include a public source or an explanation of the alias relationship.

## Current supplement

- `gpt-daybreak-blue-latest` inherits the complete `gpt-5.6-sol` rate card.
- `grok-composer-2.5-fast` uses Cursor's published Fast rate of $3/M
  input and $15/M output. Cursor does not publish a separate cache-read rate
  on the cited model page, so this supplement does not invent one.

## License

MIT
