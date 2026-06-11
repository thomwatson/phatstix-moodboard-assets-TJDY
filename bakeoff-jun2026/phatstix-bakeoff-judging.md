# Phatstix Image Bake-off — Judging Results (11 Jun 2026)

7 models x 5 tests, 2 vision judges per image (guardian + director), deterministic scoring. Composite = equal-weight mean of the four must-beat axes: **product fidelity, style matching, realism, iteration control**. Ad-usability tracked separately (excluded from composite). Baseline to beat: **nano-banana-2** — the engine in the current in-house tool, which previously lost to KIVE.

I personally re-inspected the real packshot (`packshot-thebalm-2026.jpg`) and the T2 + T5 outputs for nano-banana-2, flux-kontext-max and flux-2-max before writing this; visual claims below marked "(verified)" are mine, not just the judges'.

## Leaderboard

| # | Model | Fidelity | Style | Realism | Iteration | Ad usability* | Composite | Δ vs baseline | Price/image |
|---|-------|---------:|------:|--------:|----------:|--------------:|----------:|--------------:|------------:|
| 1 | **nano-banana-2** (baseline) | **7.2** | **7.9** | **7.4** | **8.0** | **6.7** | **7.63** | — | $0.039 |
| 2 | flux-kontext-max | 6.3 | 5.8 | 6.4 | 5.0 | 5.5 | 5.88 | −1.75 | $0.08 |
| 3 | flux-2-max | 5.9 | 6.1 | 6.2 | 4.5 | 3.9 | 5.67 | −1.96 | ~$0.12 |
| 4 | flux-2-pro | 5.7 | 6.8 | 6.1 | 4.0 | 4.4 | 5.65 | −1.98 | ~$0.06 |
| 5 | seedream-4 | 5.9 | 5.6 | 5.7 | 4.0 | 4.2 | 5.30 | −2.33 | $0.03 |
| 6 | photon | 2.5 | 4.3 | 4.7 | 2.0 | 2.0 | 3.38 | −4.25 | ~$0.03 |
| 7 | ideogram-v3-quality | 1.9 | 3.7 | 4.6 | 0.0 | 0.9 | 2.55 | −5.08 | $0.09–0.11 |

\* Ad usability is the judges' "could this run unedited" axis. Note it is the baseline's weakest axis (6.7) — even the winner needs a retouch pass.

## Did anyone beat the baseline on all four axes? (the actual question)

**No. Nobody beat nano-banana-2 on *any* single axis, let alone all four.**

- Fidelity: baseline 7.2; best challenger 6.3 (flux-kontext-max), gap −0.9
- Style: baseline 7.9; best challenger 6.8 (flux-2-pro), gap −1.1
- Realism: baseline 7.4; best challenger 6.4 (flux-kontext-max), gap −1.0
- Iteration: baseline 8.0; best challenger 5.0 (flux-kontext-max), gap −3.0

The iteration gap is the decisive one. T3 asked for two surgical changes (background to aubergine plum `#3A1F33`, headline to fluoro) with everything else pixel-frozen. nano-banana-2 scored 8 — both edits near-target, layout locked, even the lid refraction correctly relit. The next best score was 5, and every challenger silently regenerated things it was told to freeze: substrates, lighting, graffiti, letter materials, the product itself. If "iteration control" is the workflow (and for an in-house ad tool, it is), there is no second place.

One asterisk: a single *image* did beat the baseline — flux-2-pro's T1 (8/9/8 + ad 8 vs nano's 7.5/9/8 + ad 7.5). One frame out of 30 challenger frames. That is a ceiling demonstration, not a contender.

## Per-model profiles

**nano-banana-2 (7.63, $0.039)** — Won 4 of 5 tests and every axis. Its T5 bar shot (`nano-banana-2__T5.jpeg`) is the only image in the whole bake-off that reads as a photograph at first *and* second glance (verified) — convincing hand, marble, flash falloff; undone at 100% by a synthetic 12-ray starburst on the lid rim and slightly grubby knuckles on a lip product. Its T3 is the only genuinely surgical edit of the seven. Where it dies: third-party IP — its T2 smiler copies East London Liquor's "EL" monogram verbatim (verified in `nano-banana-2__T2.jpeg`); plus a persistent satin-not-matte body, mild 923 C drift, and invented bottom-of-tube structure in T1 (flared podium base). Strong everywhere, runnable nowhere without one retouch pass.

**flux-kontext-max (5.88, $0.08)** — The most consistent challenger (no test below 5.5 composite-equivalent) and the only model in the top four with a **clean brand-safety record across all five tests**: its T2 smiley is a generic acid-house face, not the EL monogram (verified in `flux-kontext-max__T2.jpg`). Its T1 slate hero (7.5/8/8) is near magazine-grade. Where it dies: colour discipline — T3 background landed `#5D054B` vivid magenta against the `#3A1F33` plum spec, with recolour bleed into the locked product zone — and style language: its T5 (`flux-kontext-max__T5.jpg`, verified) is a sombre soft-lit whisky still-life on green serpentine with a petri-dish lid and fingers pinching the bare wax; none of ELLC's hard-flash candid DNA.

**flux-2-max (5.67, ~$0.12)** — Best print/paper texture feel of the set and the best challenger T5 (limp hand aside, the stubbiest, most on-spec tube proportions of the three T5s I viewed — `flux-2-max__T5.jpg`, verified). Where it dies: copy integrity and IP — its T2 (verified, `flux-2-max__T2.jpg`) garbles the headline into "FOR LIPS NOT B__H D_TH SHELF" (BLAH-poster contamination from the ELLC refs) and stamps the EL monogram twice; T3 inherits the garble and secretly rebuilds substrate, lighting and drip materials; T4 renders the balm as sugar scrub. Also the most expensive model in the field — 3x the baseline's price for −1.96 composite.

**flux-2-pro (5.65, ~$0.06)** — The highest single peak (T1, 8/9/8 — the only image to outscore the baseline anywhere) and the deepest troughs. T4 collapses to fidelity 3: a crumbly scrub-stick column instead of a waxy dome, olive drift, geometry collisions. T5 plagiarises the reference shoot down to ELLC's branded smiley cork on the marble, merges male and female hands (hairy knuckles, crimson manicure) and kinks the wordmark baseline. T2 copies the EL monogram twice plus a gin-tap prop. Variance this wide makes it a slot-machine: brilliant one frame in five, unusable the rest.

**seedream-4 (5.30, $0.03)** — A respectable T1 (7/8/7, correct wordmark spelling but lost the oblique slant) and the cheapest credible output. Where it dies: copy and brand integrity — T2 duplicates a garbled "GIPS" headline line plus the verbatim EL monogram; T3 lands burgundy `#490014` (zero green channel — not even the right hue family) and repaints all four protected graffiti marks; T4 is a textureless CGI render that ignores every specific ask; T5 garbles the wordmark into "phststjd" with an invented ribbed base — a counterfeit product.

**photon (3.38, ~$0.03)** — Renders the wrong product in three of five tests: a crimp-sealed squeeze tube in T2/T3 and a ~6:1 elongated cigar tube with threads at both ends in T5. T2's headline is glyph soup ("LIPS" and "SHELF" never legibly appear). T3 is a full re-roll masquerading as an edit (iteration 2). Only the moody T1 staging (4/6/6.5) shows any craft, and even that flips the wordmark direction and invents an ® glyph. Cheap, and worth exactly that.

**ideogram-v3-quality (2.55, $0.09–0.11)** — Catastrophic on a brand account. Misspells the wordmark in four of five tests (PHatsix, PATSIX, PHASSIX), invents a second SKU ("PHATICO"), prints "EAST LONDON LIQUOR CO." on the talent's shirt in T5, and produces the bake-off's most damning artefact: in T3 (iteration score **0**) it ignored both requested edits and printed the instruction's hex code on the product label as "#3A1-F33" — literal prompt leakage as packaging copy. Also the second most expensive model in the field.

## Per-test winners

- **T1 — editorial slate hero: flux-2-pro** (8/9/8, ad 8). The only frame in 35 to beat the baseline anywhere: nails the brief, 923 C and a near-flawless wordmark; docked only for a mushroomed dome seam, one merged thread ridge and sub-1080 output. Runner-up: nano-banana-2 (7.5/9/8) — equal style, but a too-tall invented podium base and melted lid threads.
- **T2 — ELLC flat-lay poster: nano-banana-2** (6.5/7/7). The only model with the exact headline in the correct condensed print cut and clean type-vs-spray tension (verified) — killed for delivery by the verbatim EL monogram and a glossy green-shifted body, but it's one retouch from runnable; nothing else is. Runner-up: flux-2-pro (compositionally superb, two EL monograms + copied gin tap).
- **T3 — surgical iteration: nano-banana-2** (iteration 8 vs next-best 5). Both requested edits hit near-target with the layout pixel-locked; faults are forensic (ripple shadows, lost paper grain), not structural. Runner-up: flux-kontext-max (iteration 5) — geometry flawless but plum landed magenta and the recolour bled into the frozen product.
- **T4 — macro texture: nano-banana-2** (7/7.5/7.5). Closest to the matte-dome/gloss-collar brief; loses points to a melted caramel band at the dome base and olive drift. Runner-up: flux-kontext-max (7/6/6.5) — genuinely waxy texture but no lid in frame and banned flat e-com lighting.
- **T5 — flash bar snapshot: nano-banana-2** (7.5/8/7.5). Commissioned-shoot-quality hand/glass/flash language (verified); one colour-correct-and-retouch pass from runnable. Runner-up: flux-2-max — right proportions and mood but the hand reaches for nothing and the lid is a third the correct height. (flux-2-pro styled closer but disqualified itself with ELLC's branded cork.)

## Recurring failure modes (the patterns Thom must know)

1. **Style references leak trademarks.** 6 of 7 models reproduced East London Liquor IP verbatim somewhere: EL monogram smileys (nano-banana-2 T2; flux-2-pro T2 x2; flux-2-max T2 x2; seedream-4 T2), ELLC's branded cork (flux-2-pro T5), an "EAST LONDON LIQUOR CO." tee (ideogram T5), and BLAH-poster text contamination (flux-2-max T2/T3). Only flux-kontext-max stayed clean across all five tests. Any pipeline using competitor moodboards needs an explicit no-third-party-marks guard plus a human IP check on every output.
2. **The matte body spec fails almost universally.** Nearly every frame renders the barrel satin/glossy with specular streaks; the packshot's matte-body/glossy-collar split survived almost nowhere. This is the single most common fidelity defect in the dataset.
3. **Pantone 923 C never survives.** Hue drifts to lime, olive, lemon or safety-fluoro in essentially every output (measured drifts e.g. nano T1 `R=G≈236,B114` vs target; photon T1 hue 70° vs 62°). Budget a grade pass on every asset.
4. **"Iteration" means regeneration, not masking.** All 7 models missed the `#3A1F33` background hex (closest: nano at `#351D2D`; worst: ideogram, which printed the hex on the tube). Substrates, lighting, graffiti and letter materials drifted in every challenger. Only nano-banana-2 approached a surgical edit — and even it regenerated rather than masked.
5. **The balm dome is the hardest object in the brief.** Caramel/tan tints (flux-kontext T1, flux-2-max T1), sugar-scrub granularity (flux-2-pro T4, flux-2-max T4), melted-gloss bands (nano T4), soap-translucency (nano T5). The "dense waxy matte cream" read failed in some form in every model.
6. **The transparent acrylic lid breaks physics or vanishes.** Half-on hybrid lids (flux-2-max T1), petri-dish lids a third the correct height (flux-kontext T5, flux-2-max T5, ideogram T5), or omitted entirely — 4 of 7 models dropped the briefed set-aside lid in T4.
7. **Lighting briefs are ignored in favour of soft e-com light** — the one look the brand system explicitly bans. "Single hard side light" and "flash against dusk" were each delivered maybe twice in 35 frames; black voids replaced dusk ambience in every T5.
8. **Typography integrity is the tier divider.** Top tier (nano, flux family) holds the wordmark and headline; below it, copy collapses — garbled duplicated lines (seedream "GIPS", flux-2-max "B_TH/D_TH"), glyph soup (photon T2), misspelled wordmarks (ideogram x4, seedream T5 "phststjd"). For a brand whose tube carries one word, this alone eliminates the bottom three.
9. **Zero of 35 images were judged runnable unedited.** Even the per-test winners carry at least one disqualifying or near-disqualifying defect. The realistic workflow is generate → retouch/grade → traffic, never generate → traffic.

## What this means for the "generate these myself" recommendation

**Engine choice is settled: keep nano-banana-2.** The incumbent engine won every axis, 4 of 5 tests, and the iteration test by 3 clear points — at $0.039/image it is also cheaper than every challenger except seedream-4 and photon, both of which are unusable for brand work. There is no case for swapping the engine in the in-house tool; flux-2-max at ~$0.12 buys a worse result at 3x the price.

Two qualifiers:

1. **flux-2-pro as a T1-only sidecar is defensible.** At ~$0.06 it produced the single best frame of the bake-off on the editorial-hero brief. If you want hero statics, running both engines on hero briefs and picking is cheap (~$0.10/pair). Do not let it near iteration work (4.0) or anything fed ELLC references (it plagiarised the cork *and* the monogram).
2. **This bake-off answers "which engine", not "build vs KIVE".** nano-banana-2 previously LOST to KIVE, and the KIVE comparison from the research workflow is still pending. Winning a seven-way raw-engine bake-off does not prove the in-house tool beats KIVE's full workflow (their value layer is the editing/consistency tooling, exactly where every raw model here is weakest). Hold the final "generate these myself" call until the KIVE head-to-head lands.

Operationally, if you proceed in-house: hard-strip competitor marks from style refs (or add a negative-prompt/logo-detection gate — 6 of 7 models will copy them), budget a 923 C grade pass and a retouch pass into every asset's cost, and treat the matte-body and lid geometry as known weak points to brief around. Real per-usable-image cost is the $0.039 generation plus ~10–15 min of retouch, times a 1-in-2 to 1-in-3 keeper rate on current evidence.
