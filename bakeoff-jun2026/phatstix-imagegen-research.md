# AI Product-Photo Tooling for Phatstix — June 2026 Landscape

**Brief:** Replace KIVE (kive.ai) for iterative, art-directed brand photography of the Phatstix tube (Pantone 923 C body ~#C5D135, vertical lowercase "phatstix" wordmark, transparent acrylic lid) in the East London Liquor Co. / Ragged Edge idiom — hi-vis fluorescent accents, deep-shadow editorial lighting, anti-luxury craft energy. The replacement must beat KIVE on **all four axes**: (1) product fidelity / zero drift, (2) style matching from reference images, (3) iteration control (multi-turn, change-only-what-asked), (4) photographic realism. Budget unconstrained. All claims below verified against vendor docs, changelogs, public arenas and practitioner reports as of **11 June 2026**.

---

## The bar: what KIVE actually is in June 2026

KIVE (Kive AB, Stockholm, ~20 staff, ~$9.2M raised, no 2025–26 funding events) has repositioned from moodboard/DAM tool to **"AI Product Photography for Consumer Brands"** ("V.3" era). It is the incumbent to beat, and its anatomy is now well understood:

**Architecture (verified from kive.ai/docs, 11 Jun 2026):**
- Three custom model types: **Product models** (1–4 packshots, ~10s training — almost certainly instant subject-conditioning/IP-Adapter-class, not a LoRA), **Character models** (1–3 images), **Style models** (5–40 reference images, Pro/Enterprise only).
- Models invoked as `@modelname` in prompts. Multiple product models combine (`@product1, @product2`), and a product model + style model + "Studio" preset CAN combine in one prompt — exactly the Phatstix combo. **Hard constraint: one style model per prompt, and a style model cannot be combined with a separate style-reference image.**
- "Studios" = remixable virtual-photo-set presets. Quality tiers Default/Premium/Max ("Max uses the best model" — underlying models deliberately undisclosed; video side confirmed as Veo 3/Kling 2.1, image side never named).
- Iteration: Chat mode / "Creative Agent" — conversational edit panel with history, @model mentions mid-conversation, parallel agent sessions. Region masking is **beta** and "experimental with a higher number of failures"; docs admit "the Edit tool may require a few tries."

**Pricing:** Free 40 credits (~6 images); Basic $15/mo annual (~100 images); Pro $75/mo annual (~500 images, style models, "custom brand style training"); Enterprise custom. Free-form edit = 10 credits per attempt; **failed-but-completed generations are not refunded.**

**Self-admitted weaknesses (from KIVE's own troubleshooting docs):** "AI can occasionally make slight changes" to product details; scale errors "common"; "AI can struggle with fine print" and "text still renders poorly" even from hi-res inputs — i.e. the vertical lowercase phatstix wordmark and exact Pantone are at risk on the exact axes that matter.

**The four exploitable structural gaps:**
1. **No public API** at any tier (docs index has zero API pages; third-party "RESTful API" claims are SEO filler) → no automation, no batch pipelines, no QA gating, no Meta-variant feeds.
2. **No model/seed pinning** — silent model swaps can shift output style between sessions.
3. **Credit-metered retries with no refunds** + beta-only masking → iteration precision is roulette at 10 credits/spin.
4. **One-style-model-per-prompt ceiling** and no colour management of any kind (no hex targeting, no ΔE checking).

**Why it beat the old in-house tool (Nano Banana 2 via Replicate + Claude scaffolding):** not model access. KIVE's edge was (a) per-product instant subject lock instead of per-prompt reference hopes, (b) curated style models/Studio presets encoding good art direction, (c) edit-in-place workflow with library/version history. All three are now reproducible as API primitives — and the model ceiling has moved well past whatever KIVE wraps (the lost tool's NB2-era output is now only #6 on the LMArena Image Edit board; the leader is ~78 Elo points clear).

**Licensing note:** paid-plan KIVE output is licensed for ads; but KIVE's terms **prohibit using its outputs to train other models** — the winning KIVE creatives legally cannot become LoRA training data for the replacement pipeline.

---

## How the four axes map to the landscape

**Leaderboard ground truth (Jun 2026):** LMArena Image Edit (3 Jun, 27.1M votes): #1 gpt-image-2 (1465, ~64 pts clear), #2 mai-image-2.5 (1401), #4 grok-imagine-quality (1388), #5 gemini-3-pro-image-2k (1388), #6 gemini-3.1-flash-image / NB2 (1387), #8 gpt-image-1.5-high-fidelity (1373), #9 reve-2.0 (1356), #15 seedream-4.5 (1300). Artificial Analysis Image Editing disagrees at the top: GPT Image 1.5 (high) 1266 > GPT Image 2 1259 > NB Pro 1251 — i.e. the top four closed editors are statistically close; the bake-off, not the leaderboard, decides.

### Axis 1 — Product fidelity (zero drift on tube / wordmark / Pantone)
- **Winner class: deterministic + gated pipelines, not any single model.** The strongest mechanisms, in descending order of guarantee: pixel-compositing the real packshot (ComfyUI/IC-Light, or pixel-lock SaaS like Photoroom/Claid — guaranteed but editorially limited) → FLUX.2 LoRA + reference images + hex-colour prompting (fal) → NB Pro / GPT Image 2 multi-ref with the canonical packshot re-anchored every turn → KIVE-class subject conditioning.
- **Wordmark text:** GPT Image 2 is the practitioner consensus leader on text/wordmark rendering; NB Pro headline strength is "industry-leading text rendering" too; everything else (Seedream, Reve, open weights, Midjourney) drifts on microtext.
- **Pantone 923 C:** verified computationally — both the Pantone digital master (#ECF166) and the brand's working hex (#C5D135) are **inside sRGB** (the "out of gamut" worry is false), but they disagree with each other by ΔE2000 = 7.9, so a canonical colour target must be declared before any gate exists. Generation-time colour can land (Google case study: ΔE2000 0.90 when the colour is supplied as a **swatch image**, vs 11.25 as text hex) but every multi-turn edit re-samples colour → **a deterministic post-pass (masked Lab snap + ΔE gate) is structurally required regardless of generator.** No vendor guarantees spot-colour accuracy. KIVE has zero colour tooling — this layer is a differentiator, not parity.

### Axis 2 — Style matching from reference images
- **Winner class: high-count multi-reference frontier models + optional style LoRA.** Nano Banana Pro is the strongest documented mechanism: feed packshot + ELLC-style refs + swatch + texture refs in one call (Google's documented workflow is literally "feed the brand style guide"). GPT Image 2 takes up to 16 refs. FLUX.2 takes 8–10. Seedream 4.5/5.0-Lite take 10.
- KIVE's style models (5–40 images) are a genuine turnkey style finetune — the API-world equivalents are a **Krea/fal/Scenario style LoRA** (train the ELLC look once, reuse forever) or **Recraft's persistent style_id** (best pure style-locking API, but no product lock).
- KIVE's ceiling: one style model per prompt, no style model + style image combo. A fal FLUX.2 call with stacked LoRAs + 8 refs is a strict superset.

### Axis 3 — Iteration control (multi-turn, change only what is asked)
- **Winner class: conversational reasoning models + deterministic harnesses.** Gemini API (NB Pro/NB2) has native multi-turn conversational editing in a threaded chat — the class leader for "chat with the image." GPT Image 2's Responses API (`previous_response_id`, action=edit) is the strongest on paper; practitioners report drift creeping in around round 5–6 (mitigate by re-anchoring the canonical packshot every 2–3 turns).
- The most *surgical* paradigms are non-conversational: Reve 2.0's layout-as-code (edits are deterministic deltas to a structured representation), node canvases (Figma Weave: deterministic edit nodes interleaved with gen nodes — a relight tweak doesn't regenerate the product), and Qwen-Image-Edit-2511 (explicitly tuned to "keep edits localized to what you asked" for product/industrial work).
- KIVE's chat mode is real multi-turn but precision is its admitted weak spot (beta masking, "may require a few tries", paid retries). This is the easiest axis to beat.

### Axis 4 — Photographic realism
- **Winner class: Nano Banana line, finished with a non-generative pass.** Reddit/practitioner side-by-side consensus: NB Pro/NB2 "just looks more real" (material rendering, metal/plastic finishing) vs GPT Image 2's slight instruction-following-over-realism trade. Higgsfield Soul 2.0 and Midjourney V8.1 have the best native *editorial mood* but fail axes 1 and 3.
- Practitioners universally finish with an upscale + grain/grade pass (Magnific Precision V2 / Topaz deterministic / Recraft Crisp) to kill the "AI sheen" — which also degrades provenance watermarks as a side effect (see Licensing).

**Net:** no single tool wins all four axes. The thing that beats KIVE is a **pipeline**: NB Pro or GPT Image 2 multi-ref generation → conversational/threaded iteration with packshot re-anchoring → targeted repair (inpaint/Qwen/MAI-class single-image fix) → Precision-v2/Crisp upscale → deterministic colour snap → ΔE gate + Claude-vision brand QA gate. KIVE structurally cannot run gates at all (no API).

---

## The candidates (full table)

### Tier 1 — Frontier API models (the bake-off pool)

| Model | Class | Access | Multi-ref | Iteration | Price/image | Evidence | Caveats |
|---|---|---|---|---|---|---|---|
| **Nano Banana Pro** (Gemini 3 Pro Image) | API model | Gemini API `gemini-3-pro-image` (GA Jun 2026); Replicate `google/nano-banana-pro`; fal `fal-ai/nano-banana-pro/edit` | Best in class: 6 hi-fi object + 5 character refs per Google docs (fal/Runway market "14"); designed for product consistency across refs | Native conversational multi-turn in Gemini API chat; 1K draft → 4K final | $0.134 (1K/2K), $0.24 (4K) direct; $0.067 batch; fal $0.15/$0.30 | #5 LMArena edit (1388); practitioner consensus realism winner; "feed the brand style guide" documented workflow | SynthID on all outputs (Meta can't read it today); dense-text slightly below GPT-2; can restyle typography on text edits |
| **GPT Image 2** | API model | OpenAI API `gpt-image-2`; Replicate `openai/gpt-image-2`; fal `openai/gpt-image-2/edit` (mask support) | Up to **16 input images** per edit call (verified vs API reference + Runway changelog); all refs forced high-fidelity | Strongest on paper: Responses API `previous_response_id` threads; persistent identity mechanism; drift ~round 5–6 | $0.006 low / $0.053 med / $0.211 high (1024²); fal $0.01–0.41 | #1 LMArena Image Edit (1465, 64 pts clear) AND #1 T2I — only model leading both; best-in-class instruction following + text/wordmark | Realism slightly behind NB line; priciest at high tier; input fidelity locked high (cost); now carries BOTH C2PA + SynthID; no transparent backgrounds |
| **Nano Banana 2** (Gemini 3.1 Flash Image) | API model | Gemini API `gemini-3.1-flash-image-preview`; Replicate `google/nano-banana-2` | Published cap: **10 object + 4 character = 14 refs** (ai.google.dev — higher than NB Pro's 11) | Conversational multi-turn; fastest loop of the frontier | $0.045–0.151 by res (~$0.067 @1K); batch half | #6 LMArena edit (1387) — Pro-level quality at Flash cost. **The lost in-house tool's model line — the mandated baseline** | "Preview" API id; label/fine-text work below Pro; brand already lost with NB2-era output (harness, not model, suspected) |
| **GPT Image 1.5** | API model | OpenAI API `gpt-image-1.5`; Replicate `openai/gpt-image-1.5` | Multiple inputs; exposes `input_fidelity=high` (unlike GPT-2 where locked) | Responses API multi-turn; KNOWN BUG: input_fidelity doesn't propagate across turns — re-send packshot each turn | $0.009 low / $0.034 med / $0.133 high | Still **#1 on Artificial Analysis editing arena** (1266) — blind votes favour its high-fidelity preservation mode | Superseded by GPT-2 on instruction following; in roster purely because AA says its edit preservation is genuinely strong |
| **FLUX.2 family** [max/pro/flex/dev/klein] | API model (+ open weights) | BFL API; Replicate `black-forest-labs/flux-2-max/-pro/-flex/-dev/-klein-*`; fal `fal-ai/flux-2-pro/edit` etc. | 8 refs (pro) / 10 (flex/dev); product + style simultaneously; **understands exact hex codes** | Single-shot edit endpoints — pipeline, not chat; flex exposes steps/guidance/typography control | klein from $0.014; pro from $0.03; max from $0.07 | Strong product-photography reputation; "adheres to brand constraints like exact hex colors and legible text" (BFL); the only frontier family with a LoRA route | Closed tiers unranked on public edit arenas; instruction-following on surgical edits historically below reasoning models |
| **Seedream 4.5** (ByteDance) | API model | fal `fal-ai/bytedance/seedream/v4.5/edit` + `/text-to-image`; BytePlus ModelArk; OpenRouter | Up to 10 refs per edit (fal-verified) | Single-call edit; chain outputs; ~60s/edit | **$0.04 flat** up to 4MP — cheapest frontier-adjacent editor | #15 LMArena edit (1300); changelog targets exactly these axes ("preservation of subject details, lighting, color tone") | Mid-table Elo — volume/variant engine, not hero-shot engine; 60s latency |
| **Seedream 5.0 Lite** | API model | fal `fal-ai/bytedance/seedream/v5/lite/edit`; Replicate `bytedance/seedream-5-lite`; BytePlus `seedream-5-0-260128`. **No "Pro" exists in the West**; full 5.0 is ByteDance-consumer-apps only | `image_urls` list, max 10 (over 10 → last 10 kept); undifferentiated refs | No native conversation on fal standard endpoint; WaveSpeed ships an "Edit Sequential" variant for step-by-step edits | $0.035 (fal) | First Seedream with multi-round image-text editing + reasoning; "500 lifestyle variants" positioning | Thin practitioner evidence; reasoning/web-search may "improve" the product (fidelity risk); 9MP max |
| **Qwen-Image-Edit-2511** (Alibaba, Apache 2.0) | API model / open weights | Replicate `qwen/qwen-image-edit-2511`; fal `fal-ai/qwen-image-edit-2511` (+ `/lora`, `-multiple-angles`, trainer at $0.004/step) | Works best with 1–3 inputs — tight for product+style | Strongest open-model claim on axis 3: "reduce drift, preserve identity, keep edits localized... for product/industrial design" | Low cents (fal/Replicate metered); free self-hosted | Dual-path architecture (semantic + appearance VAE) purpose-shaped for preserving appearance while editing; multiple-angles endpoint for packshot coverage | Below frontier on photorealism; 1–3 ref ceiling; 20B self-host needs serious GPU |
| **MAI-Image-2.5** (Microsoft) | API model | Azure Foundry / OpenRouter only — **NOT on Replicate/fal/Gemini → excluded from automated roster** | **NO multi-ref** — edits API takes ONE image (verified, MS Learn 2 Jun 2026) | Stateless single-image edits; no conversation | ~$0.05–0.19 (token-priced) | #2 LMArena edit (1401), 9 days old | Disqualified as primary (no product+style refs); candidate *secondary* single-image finishing pass; 1MP output cap, 2–12 RPM |
| **Grok Imagine Image** (xAI) | API model | xAI API; fal `xai/grok-imagine-image/edit`; OpenRouter | Multi-image edit composition (count unpublished) | Prompt-driven edits, no threading; 300 rpm | $0.02 std; $0.05–0.07 quality | #4 LMArena edit (1388) — tied with NB Pro at half the price | Thin brand-fidelity track record; sparse docs; optics |
| **Reve 2.0** | API model | Reve API beta (create/edit/remix); fal `fal-ai/reve/remix` (1–6 refs) | 1–6 refs; layout elements carry per-element descriptions | **Layout-as-code**: edits are deterministic deltas — architecturally the most surgical paradigm | ~$0.0067–0.024 | #2 LMArena T2I (1273); #9 edit (1356); agent-native API | VibeDex eval: Subject/Object Integrity 13/18, "collapses under precise details" — documented weakness exactly where Phatstix needs strength. Wildcard only |
| **Midjourney V8.1** | SaaS | Web only; API still limited rollout; `--oref` not confirmed on V8 | 1 oref + srefs — weak vs 10–16-ref API models | Vary Region/Editor; known weakness | $10–120/mo subs | Best native editorial *feel* for the ELLC mood | Fails axes 1 and 3; style-exploration sidecar only |
| **Ideogram 4.0** (open weights, 3 Jun 2026) | API model | ideogram.ai API; self-host (commercial license) | Style/brand conditioning + JSON bounding-box layout; NOT packshot-preserving editing | Deterministic layout JSON regeneration | Credit-based; free self-host | 0.97 English OCR; typography/packaging-copy leader | Not an editor — typography/ad-layout layer on top of a photo pipeline |
| **HunyuanImage 3.0 Instruct** (Tencent, open) | open weights | HF; fal `fal-ai/hunyuan-image/v3/instruct/edit` | i2i editing; limited multi-ref compositing | Single-call instruct edits with chain-of-thought | cents (fal); free self-host | #1 open-weights on AA editing (1232) | ~35 Elo below frontier closed; heavyweight; backup if full self-ownership ever required |
| **Luma Uni-1.1 / Max** | API model | Luma API; fal; Runware | Up to 9 refs **each with a defined role** ("this is the product, preserve exactly") — cleanest ref-role API | Modify mode + seeds = reproducible iteration | ~30% below Google/OpenAI | #11/#13 LMArena edit; SOTA RISEBench reasoning-edit | Vendor Elo claims contradicted by arena; few packshot-fidelity reports |

### Tier 2 — SaaS product-photo platforms (KIVE-shaped)

| Tool | Verdict | Key facts |
|---|---|---|
| **Photoroom** | Strongest SaaS challenger mechanism | May 2026 **Product Fixer** brush (corrects drift against the original product image), multi-ref fidelity for video, **Claude MCP connector** (mcp.photoroom.com) = only SaaS with native conversational iteration; API $0.02–0.10/img. Weakness: e-com staging DNA, no custom style training — ELLC deep-shadow is not its idiom |
| **Nightjar** | Cheap lottery ticket | Feature-for-feature KIVE clone (product anchoring + custom Photography Styles from refs + text-based localized edits, 4K, $25/mo) but Shopify rating 3.0 from 2 reviews; "works 10% of the time at best." Cleanest small-vendor ToS (full output ownership, no training without opt-in). No API |
| **Claid.ai** | API-first hybrid | Only documented `style_reference_image` API (style/depth/denoising strengths) + 20–30-image brand training; fashion/catalog DNA; **no public API overage pricing** (sales-gated) and no surfaced ads-licence — both block promotion |
| **Omi** | Fidelity benchmark only | 3D digital twin = perfect tube/wordmark/Pantone by construction, deterministic lighting iteration; but CG-render realism likely loses axis 4 |
| **Flair.ai** | Avoid | Right shape on paper (canvas, custom product+aesthetic models, API early access) but most-documented fidelity failures in category ("branding altered… even using 35 images") |
| **Recraft V4/V4.1 Pro** | Style engine, not product engine | Best persistent style-locking (1–5 ref images → reusable style_id) + **only generator family with a first-class exact-RGB colour parameter** (`controls.colors [{rgb:[197,209,53]}]`); product subject lock absent. Also: Crisp Upscale at $0.004 |
| **Pebblely / soona-Mokker / Caspa / ProductScope / CGDream** | Exclude | E-com template factories; incapable of art-directed editorial. Booth.ai is dead (category cautionary tale) |
| **Pikes AI** | Unvetted | Claims are its own blog + affiliate reviews; pricing/API unverifiable. Do not shortlist without hands-on |
| **Higgsfield** | Skip for stills | Wrapper over the same NB/Seedream models; Soul 2.0 aesthetic is on-mood but iteration control is preset-grade; no API. Possible later for motion versions |

### Tier 3 — Multi-model canvases / workspaces

| Tool | Verdict | Key facts |
|---|---|---|
| **FLORA (flora.ai)** | Strongest single KIVE-replacement canvas | Node canvas + shared library/version history (KIVE's two halves); unmetered NB2/Pro **through 1 Jul 2026** (then metered ~$0.151/img NB2); FAUNA agent free/unlimited; **API + MCP from $18/mo** — the old Claude scaffolding could drive it. Cleanest canvas ToS: you own output; Flora contractually firewalls Google from training on uploads |
| **Krea** | Cheapest serious option | Krea 2 style-control model + in-app **style LoRA training** (the ELLC-look lock); Nodes + Node Agent on Pro ($35/mo); commercial licence from Basic ($9/mo) — free tier output NOT licensed for commercial use |
| **Figma Weave** (ex-Weavy) | Most surgical iteration | Deterministic edit nodes interleaved with gen nodes — manual corrections propagate downstream without regenerating the product; ControlNet + LoRA support; mid-migration into Figma through 2026, pricing in flux |
| **Magnific (ex-Freepik)** | Cheapest flat-rate frontier access + the finishing layer | Premium+ $33.75/mo = unlimited NB2/Flux.2-Pro images; **Precision V2 upscaler** ("Color-Lock", photo flavour, ~$0.08/2K, $0.16/4K) is the de-facto finishing pass; Business tier has €50k-capped IP indemnity. Trust wobble on "unlimited" reversals |
| **Adobe Firefly Boards + Adobe MCP** | Ideation + deterministic finishing via MCP | NB Pro/NB2/Flux partner models in Boards; **Adobe MCP (available in this environment) exposes select-by-prompt + HSL/colour ops — i.e. the selective-colour snap can be prototyped today with zero new accounts — but NOT partner-model generation** |
| **Comfy Cloud / ComfyUI** | Escape hatch | Maximum control (pixel-composite + IC-Light relight = provably zero drift); $20–100/mo + expertise; agencies productionize on it (SVEDKA Super Bowl 2026); wrong daily driver for one person |
| **Scenario** | Fine-tune layer | Trains on FLUX.2/FLUX.2-Edit/Z-Image/Qwen bases + **model merging** (subject model + style model); $15–75/mo; the closest managed like-for-like to KIVE's custom models; realism ceiling = open-weight bases |
| **Visual Electric** | Dead | Perplexity acqui-hire Oct 2025, sunset. Users migrated to Flora/Krea/Weavy |
| **Glif / Leonardo / Canva** | Periphery | Glif = scratchpad; Leonardo = API workhorse below frontier; Canva = downstream ad-format assembly via MCP, not hero-shot generation |

---

## Recommended bake-off roster (API-testable now, with EXACT model IDs)

All eight are callable today via Replicate, fal.ai, or the Gemini API — no sales calls, no waitlists. Ordered by likelihood of beating KIVE on all four axes. Standard harness for every contender: canonical packshot + flat #-target colour swatch image + 2–3 ELLC-style refs in every call; 5-round iteration script (relight → prop swap → crop/extend → wordmark check → colour check); outputs run through the same finishing/QA stack (Precision-V2 or Crisp upscale → masked Lab colour snap → ΔE2000 gate on segmented tube hue/chroma → Claude-vision wordmark/geometry gate).

| # | Contender | Exact access | Per image | What it tests |
|---|---|---|---|---|
| 1 | **Nano Banana Pro** | Gemini API `gemini-3-pro-image` (GA; preview: `gemini-3-pro-image-preview`); Replicate `google/nano-banana-pro`; fal `fal-ai/nano-banana-pro/edit` | $0.134 1K/2K, $0.24 4K (Google direct; $0.067 batch); fal $0.15/$0.30 | The favourite: highest-fidelity multi-ref (object vs character ref roles), consensus realism winner, native conversational multi-turn. Tests whether "feed the brand style guide" + threaded chat beats KIVE's product+style models outright |
| 2 | **GPT Image 2** | Replicate `openai/gpt-image-2`; fal `openai/gpt-image-2` + `openai/gpt-image-2/edit` (mask support); OpenAI direct for Responses-API threading | $0.053 med / $0.211 high (1024²); fal $0.01–0.41 | The arena leader: 16-ref edits, best wordmark/text rendering, strongest multi-turn API. Tests instruction-following precision and whether re-anchoring every 2–3 turns kills the round-5 drift |
| 3 | **FLUX.2 LoRA stack (fal)** | Train: `fal-ai/flux-2-trainer` ($0.008/step, ~$8/run); infer: `fal-ai/flux-2/lora` + `fal-ai/flux-2/lora/edit` (LoRA list + up to 8 refs via @image syntax); plain editing: Replicate `black-forest-labs/flux-2-pro` (8 refs) | ~$0.021/MP (~$0.04 per 1MP edit) + ~$8–24 one-off training | The product-identity-lock route: ONLY frontier stack combining fine-tuned LoRA + multi-ref in one call (a superset of KIVE's one-style-model limit). Tests hex-exact Pantone adherence and whether a style-LoRA + packshot-ref combo guarantees zero drift |
| 4 | **GPT Image 1.5 (high fidelity)** | Replicate `openai/gpt-image-1.5`; OpenAI direct with `input_fidelity=high` | $0.034 med / $0.133 high | The preservation specialist: still #1 on AA's blind editing arena. Tests whether its high-fidelity edit mode holds the tube better than GPT-2; workaround the multi-turn fidelity bug by re-sending the packshot each turn |
| 5 | **Seedream 4.5** | fal `fal-ai/bytedance/seedream/v4.5/edit` (10 refs, ~60s) | $0.04 flat (≤4MP) | The volume engine: can 1/5th-cost output pass the same QA gates? If yes, it owns variant/batch production even if it loses hero shots |
| 6 | **Qwen-Image-Edit-2511** | Replicate `qwen/qwen-image-edit-2511`; fal `fal-ai/qwen-image-edit-2511` (+ `fal-ai/qwen-image-edit-2511-trainer` $0.004/step, `-multiple-angles`) | low cents (metered) | The surgical-edit arm: explicit "keep edits localized, preserve identity" tuning. Tests as the repair-pass layer (axis 3) inside another model's pipeline, plus multiple-angles for packshot coverage |
| 7 | **Seedream 5.0 Lite** | fal `fal-ai/bytedance/seedream/v5/lite/edit` (image_urls ≤10); Replicate `bytedance/seedream-5-lite`; (sequential-edit variant on WaveSpeed) | $0.035 | Wildcard: first Seedream with multi-round editing + reasoning. Tests whether reasoning helps or hurts strict packshot fidelity (risk: model "improves" the product) |
| 8 | **Nano Banana 2** — BASELINE | Gemini API `gemini-3.1-flash-image-preview`; Replicate `google/nano-banana-2` (14 refs: 10 object + 4 character) | $0.045–0.151 by res (~$0.067 @1K) | The control arm: the lost tool's model line, now improved and run through the NEW harness (refs + gates + repair). Isolates whether the old loss to KIVE was model or harness. If NB2-plus-harness beats KIVE, the conclusion is definitive |

**Excluded from the automated roster and why:** MAI-Image-2.5 (#2 edit arena but single-image-input only, and not on Replicate/fal/Gemini — re-test later as a finishing pass via OpenRouter); Grok Imagine (arena-strong but thin fidelity record — substitute in if a slot frees up); Reve 2.0 (documented subject-integrity weakness); Midjourney (no real API, fails axes 1+3); Luma Uni-1.1 (vendor claims contradicted by arena — second-round candidate for its per-ref roles + seeds).

**Scoring:** blind-rank outputs per axis; KIVE Pro runs the same brief as the control arm (manual trial, below). A challenger must win or tie on ALL FOUR axes — the brand has already lived through winning three of four.

---

## SaaS/canvas tools worth a manual trial (and what to look for)

1. **KIVE Pro ($75/mo) — the control arm.** Re-train product model from the canonical packshots at Max quality, train a style model from 5–40 ELLC refs, run the identical 5-round brief. Also: pull the old top-performing KIVE ad from Meta Ad Library and check whether it carried the "AI info" label — if it ran labeled and still won, the provenance axis collapses entirely as a selection criterion.
2. **FLORA Pro ($50/mo) — the canvas favourite.** Decide BEFORE 1 Jul 2026 to exploit unmetered NB2/Pro launch terms. Look for: does the node graph + FAUNA agent reproduce the old tool's iteration loop with KIVE's library bolted on; test API/MCP from Claude Code directly.
3. **Photoroom Max + Claude MCP (~$35/mo + API credits).** Look for: can Product Fixer + MCP-chained edits produce a deep-shadow editorial look at all, or does it cap out at clean e-com staging? It's the only SaaS with conversational iteration in Thom's existing Claude workflow.
4. **Krea Pro ($35/mo).** Train an ELLC style LoRA from the moodboard (the cheapest persistent style-lock anywhere); test stacking it with packshot refs in the edit workflow. Free-tier output is NOT commercially licensed — generate on paid only.
5. **Figma Weave (free tier first).** Look for: deterministic edit-node propagation ("relight without touching the product") — if it delivers, it's the iteration-control benchmark the API pipeline must match. Pricing/roadmap in flux mid-migration.
6. **Nightjar Studio ($25/mo) — lottery ticket.** KIVE-clone feature set (product anchoring + custom styles from refs); 6 free images tell you in an hour whether the "10% of the time" review is fair.
7. **Recraft (free tier → API)** — as the style engine + colour benchmark only: mint a persistent ELLC style_id, test `controls.colors` exact-RGB on scene plates; never expect product lock from it.
8. **Magnific Premium+ ($33.75/mo)** — not as a generator but to bench Precision V2 colour-lock on a Phatstix render (one $0.16 test against the swatch) and as the pipeline's standing finishing pass.

**Universal look-fors:** (a) wordmark survives 5 consecutive edits unchanged; (b) tube hue stays within ±2° hue / 0.9–1.1 chroma ratio of the canonical packshot region; (c) an instructed single change leaves everything else pixel-stable; (d) output ≥2K without re-generation artifacts; (e) exportable at volume without per-retry credit bleed.

---

## The fine-tune question (LoRA vs multi-reference — verdict for a single-hero-product brand)

**Verdict: do NOT bet the fidelity axis on a product LoRA. Multi-reference conditioning + repair pass + QA gates is the primary mechanism; a STYLE LoRA is the high-value fine-tune; the fal FLUX.2 stack is the one place both combine.**

The practitioner consensus (VP Land bake-off Jan 2026; Segmind; BudgetPixel; zsky) inverted the 2024 playbook: reference-image models (NB Pro class) now beat LoRAs on exact structure, readable text/wordmarks and asset fidelity, while LoRAs win at locking a global *style* cheaply across hundreds of generations. KIVE itself proves the point — its 10-second "product models" from 1–4 images are reference-conditioning, not weight training.

Practical programme for Phatstix:
1. **Primary:** packshot + swatch + style refs per call on NB Pro / GPT Image 2 (roster #1–2). No training, instant, and the wordmark rides on the strongest text renderers.
2. **Style LoRA (recommended):** train the ELLC look once — fal `fal-ai/flux-2-trainer` (~$8) from 10–30 curated style refs, or Krea/Scenario in-app. This replicates KIVE's style-model moat as an owned asset.
3. **The superset play (roster #3):** fal `fal-ai/flux-2/lora` + `/lora/edit` accept a LoRA **list** plus up to 8 reference images in one call — subject-LoRA + style-LoRA + canonical packshot ref simultaneously. KIVE caps at one style model and zero style images alongside it; this is strictly more.
4. **Edit-LoRA (later, if the look stabilises):** fal `fal-ai/flux-2-trainer/edit` (~$19/1k steps) learns "flat packshot → ELLC editorial scene" as a deterministic repeatable transformation from 15–50 before/after pairs — the most deterministic iteration mechanism in the fine-tune space. Note: pairs must NOT be built from KIVE outputs (their ToS prohibits training other models on KIVE output).
5. **Avoid:** Replicate's training stack (FLUX.1-era, a generation behind — train on fal, infer anywhere); Astria (2024-vintage); a *product* LoRA as the sole fidelity bet (small-wordmark fidelity from weights alone is unproven; refs win on text).
6. **Self-host (ostris ai-toolkit, klein 4B ~$0.50/run)** only if fal's managed endpoints prove insufficient for masking/ControlNet — ops burden not justified at single-brand scale.

---

## Pipeline patterns from practitioners (including post-processing)

What DTC brands/agencies actually ship for Meta in mid-2026 is a pipeline, not a tool:

```
canonical packshot + swatch image + 2–4 style refs
        │
        ▼
[1] GENERATE  — NB Pro / GPT Image 2 multi-ref (hero) · Seedream 4.5 (volume) · FLUX.2+LoRA (hex-critical)
        │
        ▼
[2] ITERATE   — threaded multi-turn (Gemini chat / Responses API), re-anchor packshot every 2–3 turns,
                restate invariants ("change only X; tube, wordmark, colour unchanged")
        │
        ▼
[3] REPAIR    — targeted inpaint of wordmark/label only (Qwen-2511 localized edit, NB in-context edit,
                or MAI-2.5 single-image pass) — never full regeneration
        │
        ▼
[4] FINISH    — upscale: Magnific Precision V2 (flavor=photo; "Color-Lock"; ~$0.08–0.16) for hero,
                Recraft Crisp ($0.004) for feed sizes. BAN generative modes (Magnific Creative,
                Topaz Redefine) — documented text/logo changers. Then grain/grade to kill AI sheen
        │
        ▼
[5] COLOUR SNAP — deterministic masked Lab correction AFTER the upscaler: segment tube (SAM-2 ~$0.002
                or Adobe MCP image_select_by_prompt) → snap hue/chroma to canonical target
                (Photoshop API actionJSON ~$0.15/call, or free Python/ImageMagick)
        │
        ▼
[6] QA GATES  — numeric: ΔE2000 on segmented tube (gate hue ±2°, chroma ratio 0.9–1.1 — NOT whole-tube
                flat ΔE; a lit 3D tube legitimately spans L*). Vision: Claude judge, packshot vs render,
                scoped to wordmark spelling/orientation, lid transparency, geometry (~$0.01–0.03/img).
                Fail → route back to [3] with the defect named
        │
        ▼
[7] SHIP      — lock Meta Advantage+ "enhancements" OFF on the ad account (they re-enable themselves);
                A/B labeled vs unlabeled creative to measure the actual AI-label penalty
```

**Key practitioner facts behind the design:**
- "If a workflow does not contain an explicit fidelity gate, the model will trade accuracy for plausibility" — the gate layer is what KIVE structurally lacks (no API), and at ~$0.01–0.17/image all-in it converts "probably on-brand" into "guaranteed on-brand."
- **Colour target bug found during research:** Pantone 923 C's digital master (#ECF166) and the brand's working hex (#C5D135) are ΔE2000 = 7.9 apart — ~4x any sane gate threshold. Thom must declare ONE canonical target before "zero drift" is definable; recommend the measured median Lab of the tube body in the canonical packshot. Both candidates are inside sRGB (gamut panic unfounded; #C5D135 is at ~88% of the sRGB chroma ceiling, so watch hot highlights).
- Supplying the colour as a **swatch image** rather than a hex string in text took Gemini from ΔE 11.25 to 0.90 in Google's own study — always include the swatch in the ref set.
- Failure modes still burning teams in 2026: wordmark warp/colour drift across generations; "AI sheen" (Snag Tights called it out publicly); Meta auto-detecting AI imagery via C2PA-style signals and applying the "AI info" label (engagement penalty concentrated on emotional/aspirational creative — Phatstix's lane — but no documented delivery throttle for ordinary commerce ads, and Meta cannot read SynthID today); Advantage+ silently mutating creative.
- Agencies productionize in node canvases (ComfyUI for code-comfortable teams; Weave/Flora for design teams). The single biggest insight: **KIVE won on harness, not model — and the harness is now commodity API primitives.**

---

## Licensing/commercial notes for ad usage

| Route | Commercial/ads status | Training on your data | Indemnity | Notes |
|---|---|---|---|---|
| **Gemini API (NB Pro/NB2), paid tier** | Clean; Google claims no output ownership; no ads restriction | Paid tier: not used for training. **Never generate brand assets on the free tier** (free = used for improvement, human review) | Only via **Vertex AI** paid endpoints (Google's generated-output + training-data indemnity); plain AI-Studio keys not clearly covered | Route production through Vertex for indemnified output. SynthID on 100% of outputs, non-disableable |
| **OpenAI API (gpt-image-1.5/2)** | Strongest ownership language anywhere: OpenAI **assigns** all right/title/interest in output; marketing use explicit | Not used for training by default | **Copyright Shield** — uncapped, included for API customers | Voided if inputs lacked rights — feeding *East London Liquor's actual photography* as style refs is the riskiest pattern in the whole programme (use original/owned/licensed style refs for production; competitor refs for internal exploration only). Outputs carry C2PA + SynthID since May 2026 |
| **FLUX.2 via fal** | Clean for ads when generated **through fal's licensed endpoints** ("full commercial rights, no additional license"); fal model pages badged "Commercial use" | Customer retains inputs; fal may use anonymized usage data | None (fal disclaims; customer indemnifies fal) | **The trap:** downloading the trained LoRA and self-hosting it = outside BFL's non-commercial dev licence → needs a paid BFL Builder licence. Keep training AND inference on fal. Also the lowest-provenance route (strippable C2PA only, no pixel watermark) — a legitimate tiebreaker, not an evasion strategy |
| **Replicate (OpenAI/Google/Qwen models)** | Inherits upstream model terms; standard metered platform terms | — | None | Convenient for the bake-off; for production prefer direct OpenAI/Vertex for the indemnities |
| **KIVE** | Paid plans only ("e-commerce, marketing, advertising" — docs FAQ); free tier has no commercial grant | "We do not use your data to train our AI" | None | ToS is an unfetchable JS SPA — verbatim ownership clause unaudited. **Outputs may not be used to train/benchmark other models** — blocks LoRA-training on the winning KIVE ads |
| **FLORA** | Cleanest canvas ToS: "you own all rights, title, and interests in and to all Output"; no tier restriction | Contractually firewalls third-party model providers (incl. Google) from training on uploads | None | Unmetered NB2/Pro launch offer expires 1 Jul 2026; post-offer ≈$0.151/NB2 image against included usage |
| **Krea** | Commercial licence from Basic ($9/mo) up, incl. marketing/client work; **free-plan output NOT licensed commercially** | Firm no-training only at Business/Enterprise; assume sub-Business uploads may be used | None below Enterprise | ToS lives on an unrenderable Notion page — ownership language is from marketing pages |
| **Magnific** | Commercial AI licence with paid plans (covers Premium+ "unlimited" volume in ads) | — | Business tier: IP indemnity **capped €50,000** | Legal/pricing pages 403 to fetchers — confirm verbatim in-app before contracting |
| **Nightjar** | "Full ownership rights to the Output"; any lawful commercial purpose, no per-image fee | No training without explicit opt-in | None | Free-tier output may carry attribution — paid tier only for ads |
| **Claid** | **Unresolved** — no commercial-licence statement on pricing/API pages; API overage pricing sales-gated | — | — | Two open items block any production commitment |
| **Adobe Firefly** | Indemnity is **enterprise-contract only** (contradicting blog folklore about cheap-tier indemnification); partner models (NB et al.) NOT covered | — | Enterprise entitlement required | Compositing a real packshot in weakens coverage exactly where Phatstix needs it |
| **Bria** | Only vendor with indemnity at PAYG entry ($0.03/img, "standard indemnification"; Business = unlimited) | Trained 100% on licensed data (Getty/Alamy/Envato) | Capped at PAYG; unlimited at Business | Licensing-safest ≠ quality-best — fidelity vs frontier unproven; keep as risk-averse fallback |

**Cross-cutting ads-compliance position (UK/Meta):**
- **UK ASA/CAP:** no AI-specific rules, no mandatory AI disclosure. The binding constraint is that the depicted product must accurately reflect the real product (misleadingness — CAP's worked example is cosmetics product-effect imagery). The fidelity axis IS the compliance axis.
- **Meta:** detects AI via C2PA/IPTC metadata + its own classifiers (not SynthID today; re-check quarterly — Google's SynthID detection API went commercial 19 May 2026). The finishing pass (re-export + diffusion upscale) kills metadata and plausibly SynthID as a side effect — run it for quality, never pair it with a false "no AI" declaration (that is itself a violation with retroactive-flag risk). Quantified label penalties are user-side and concentrated on emotional creative; KIVE's AI creative already top-performed under this exact regime — pull the old winner from Ad Library and check its label status before weighting this at all.

---

## Bottom line

KIVE in June 2026 is a $75/mo workflow moat around undisclosed models: instant product models + style models + studios + chat editing — with no API, no seed pinning, paid retries, beta masking, admitted text/colour drift, and zero colour management. The frontier has moved past whatever it wraps. The replacement is not a tool but a Claude-orchestrated pipeline — NB Pro and GPT Image 2 as the hero engines, FLUX.2-LoRA-on-fal as the identity-lock superset, Seedream for volume, Qwen-2511 as the surgical repair arm, NB2 as the controlled baseline — finished with Precision-V2/Crisp, a deterministic colour snap, and twin QA gates (ΔE numeric + Claude vision) that make zero drift a guarantee KIVE structurally cannot offer. Run the eight-model bake-off against KIVE Pro as control, decide the FLORA question before its 1 Jul unmetered offer lapses, and declare the canonical colour target first — without that, "zero drift" is not even definable.
