# rshade-theme

Canonical design tokens (fonts + accent color ramp) shared across the rshade
docs ecosystem. One definition, many consumers:

- `rshade.github.io` — the blog (Astro + Tailwind 4)
- `finfocus` — Astro Starlight docs
- `ax-go` — docs site (greenfield)
- `canonical-syndicate` — docs site (greenfield)

The single source of truth is [`tokens.css`](./tokens.css): a small set of
vendor-neutral CSS custom properties. It contains **only** `:root` tokens — no
Tailwind directives, no Starlight `--sl-*` variables, no `@apply`, no `.prose`
rules. Each consumer maps these onto its own system.

## Tokens

| Property               | Value                         | Purpose           |
| ---------------------- | ----------------------------- | ----------------- |
| `--font-sans`          | `"Inter", …, sans-serif`      | Body / UI font    |
| `--font-mono`          | `"JetBrains Mono", …, mono`   | Code font         |
| `--color-accent`       | `oklch(0.65 0.18 250)`        | Base accent       |
| `--color-accent-hover` | `oklch(0.72 0.18 250)`        | Hover accent      |
| `--color-accent-soft`  | `oklch(0.65 0.18 250 / 0.12)` | Soft tinted fill  |

See [`tokens.css`](./tokens.css) for the complete font-family fallback stacks.

## Fonts are not vendored

This repo ships **no font files**. Consumers remain responsible for loading
Inter and JetBrains Mono (for example via [Fontsource][fontsource] or a `<link>`
tag). The tokens only declare the font-family *stacks*; if the webfonts are not
loaded, the stacks fall back to the listed system fonts.

[fontsource]: https://fontsource.org/

## Consumption (git submodule)

`rshade-theme` is consumed as a git submodule — not (yet) an npm package. From a
consumer repo, at the docs project root:

```bash
# add the submodule into a ./theme directory
git submodule add https://github.com/rshade/rshade-theme theme

# pin to a known-good tag so updates are deliberate
cd theme && git checkout v0.1.0 && cd ..
git add theme && git commit -m "chore: vendor rshade-theme tokens at v0.1.0"
```

When cloning a consumer repo that already references the submodule:

```bash
git clone --recurse-submodules <consumer-repo-url>
# or, if already cloned:
git submodule update --init --recursive
```

To bump to a newer tag later:

```bash
cd theme && git fetch --tags && git checkout v0.2.0 && cd ..
git add theme && git commit -m "chore: bump rshade-theme to v0.2.0"
```

### Plain CSS / Astro / Tailwind consumers

Import the tokens once (for example in a global stylesheet) and reference the
custom properties directly:

```css
@import "../../theme/tokens.css";

body {
  font-family: var(--font-sans);
}

a {
  color: var(--color-accent);
}

a:hover {
  color: var(--color-accent-hover);
}
```

### Starlight consumers

Starlight is themed through its own `--sl-*` variables. Add a thin bridge that
imports the tokens and maps them onto Starlight's variables:

```css
/* docs/src/styles/theme-bridge.css */
@import "../../theme/tokens.css";

:root {
  --sl-font: var(--font-sans);
  --sl-font-mono: var(--font-mono);
  --sl-color-accent: var(--color-accent);
  --sl-color-accent-high: var(--color-accent-hover);
}
```

Register the bridge in `astro.config.mjs`:

```js
starlight({
  customCss: ["./src/styles/theme-bridge.css"],
});
```

## Future npm consumption

`package.json` already declares an `exports` map and a `files` allowlist, so the
package can be published and imported without restructuring:

```css
@import "rshade-theme/tokens.css";
```

## Versioning

Releases are tagged (`v0.1.0`, …) so consumers can pin a known-good revision.
Token changes are tracked in [`CHANGELOG.md`](./CHANGELOG.md). Treat any change
to an existing token value as potentially breaking for downstream sites.

## License

Licensed under the Apache License, Version 2.0 — see [`LICENSE`](./LICENSE) and
[`NOTICE`](./NOTICE).
