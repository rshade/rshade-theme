# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Fixed

- README Starlight bridge now maps the accent ramp under both `:root` and
  `:root[data-theme="light"]`. The previous snippet themed only `:root`, so
  Starlight's higher-specificity `:root[data-theme="light"]` default won in
  light mode — dark mode showed the brand accent while light mode stayed
  Starlight purple.
- README Starlight bridge now maps `--sl-color-accent-low` to
  `--color-accent-soft` (low-emphasis accent tint, e.g. active sidebar item),
  which was previously left at the Starlight default.
- README now warns that the relative `@import "../../theme/tokens.css"` path
  must be adjusted to the consumer's file depth, since a missing CSS `@import`
  fails silently with no build error.

## [0.1.0] - 2026-06-18

### Added

- `tokens.css` — canonical, vendor-neutral design tokens (`--font-sans`,
  `--font-mono`, `--color-accent`, `--color-accent-hover`, `--color-accent-soft`).
- `package.json` with an `exports` map and `files` allowlist exposing
  `tokens.css` for future npm-based consumption.
- `README.md` documenting git-submodule consumption, the Starlight `--sl-*`
  bridge, and the assumption that consumers load Inter / JetBrains Mono.
- `LICENSE` (Apache License 2.0) and `NOTICE`.

[Unreleased]: https://github.com/rshade/rshade-theme/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/rshade/rshade-theme/releases/tag/v0.1.0
