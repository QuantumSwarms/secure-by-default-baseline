# STACK PACKAGES — blessed best-in-class dependencies

Companion to `STACK_LAW.md`. These are the **blessed** libraries by domain. Reach
for these first; don't hand-roll what a best-in-class package already does well.
Each entry is tagged by its place in the stack law:

- **apex-Python** — runs at the Python apex (own infra / control plane).
- **edge-JS** — runs at the JavaScript edge leaf (Cloudflare Workers / browser).
- **output** — emitted artifact, not a runtime dependency.

> The rule is still **Python on top.** A blessed edge-JS package is a subordinate
> leaf; deterministic logic that *can* live at the apex (e.g. color math) should.

## Color

| Package | Tier | Why blessed |
|---|---|---|
| `coloraide` | apex-Python | Full CSS Color 4/5, OKLCH/OKLAB, gamut mapping, interpolation. Server-side color math belongs at the apex. |
| `culori` | edge-JS | Small, fast color manipulation/interpolation for the browser/worker. |
| `colorjs.io` | edge-JS | Spec-accurate (by the CSS color spec authors) when correctness > size. |
| CSS `oklch()` | output | Native perceptual color — no library needed when just emitting styles. |

## Animation (inherently browser → edge leaf)

| Package | Tier | Why blessed |
|---|---|---|
| `GSAP` | edge-JS | Best-in-class timeline engine; precise scrubbing/sequencing. Use for timeline/video work. Free since the Webflow acquisition. |
| `Motion` (motion.dev) | edge-JS | Best for declarative/interactive UI motion (formerly Framer Motion). Use for interactive prototypes. |
| `anime.js` / `Popmotion` | edge-JS | Lightweight options for simple tweens / lower-level control. |

## Iconography & dataviz

| Package | Tier | Why blessed |
|---|---|---|
| `lucide` | edge-JS / output | Best-in-class open icon set; consistent, tree-shakeable. |
| `visx` / `D3` | edge-JS | Composable, powerful dataviz primitives. |
| `Vega-Lite` | output | Declarative charts when a spec-driven chart is enough. |

## Typography

| Choice | Tier | Why blessed |
|---|---|---|
| Inter / Instrument Serif / JetBrains Mono (Google Fonts) | output | EpochCore house pairing — sans / editorial-serif / mono. |

## Seal / crypto (apex)

| Package | Tier | Why blessed |
|---|---|---|
| `tooling/epoch_native` (this repo, when Python) | apex-Python | Deterministic seal hashes (djb2 + FNV-1a) with optional C++ muscle + pure-Python fallback. The reference apex-API pattern. |
| Ed25519 / ML-DSA-65 / ML-KEM-768 | apex-Python | The canonical EpochCore PQC signing/KEM stack (RAS 40668c787c463ca5 / 1210 Hz anchor). |

## How to extend

Adding a new domain? Pick ONE best-in-class package, tag its tier, give a
one-line "why blessed," and prefer the apex-Python option when the logic is
deterministic. Keep hand-rolled code only where no blessed package fits.
