# reel stack compare · AI Mindset Shaper

> **Live:** https://apowall.github.io/reel-stack-compare/

A single-file, Shaper-style dashboard that compares two video-editing stacks **layer by layer** — and a worked example of how AI Mindset turns a piece of shared knowledge into a publishable artifact.

It was built as a response to a montage stack shared by **Max Postnikov** (a deterministic long-form YouTube cut-chain). The dashboard sets that stack side by side with our short-form Instagram Reels system, names what we borrow, what stays our moat, and where the stack evolves next — with the technology survey backed by EXA research.

![cover](assets/og-cover.png)

---

## What's inside

Both systems share one primitive — **cutting by word-level transcript timecodes** — then diverge. Max's stack is tuned for **long-form YouTube** (deterministic script chain, монтажный план as the single source of truth, draft→final). Ours is tuned for **short-form Reels** (visual block editor, mode/style library, audio-first lip-sync).

Five tabs:

| tab | what it shows |
|-----|---------------|
| **сравнение** | the two pipelines side by side + a layer-by-layer verdict table (who's stronger, what move we make) |
| **короткий формат** | Max's pipeline *ported* to short-form vs our short-form answer + 2026 short-form KPIs + 5 short-form techniques |
| **заимствуем** | six borrows ranked by leverage (cut-plan→OTIO, draft→final, Scribe…) + our moat |
| **развитие** | the target merged architecture + three roadmap horizons |
| **технологии** | EXA-researched technology matrix with fit ratings |

Keyboard: `1`–`5` switch tabs · `d` toggle dark/light.

## The three highest-leverage takeaways

1. **cut plan → OpenTimelineIO** — Max's "монтажный план = single source of truth" is literally the industry-standard OTIO (`.otio`): a JSON cut graph with `RationalTime`, tracks/clips, adapters to FCP / Resolve / Premiere. Adopting it makes our `realself.timeline.v1` manifest frame-accurate and round-trippable with any NLE.
2. **draft → final two-tier render** — a 180p ultrafast draft from the *same* plan to check the cut cheaply, then a 1080p hero render. Encode research confirms the role split: hardware `videotoolbox` for the fast draft, software `libx264` CRF for the hero.
3. **ElevenLabs Scribe for word timestamps** — Scribe v2 ≈ 3.1% WER (2nd in the 2026 Coval benchmark) and roughly **2× more accurate than local Whisper on Russian**. Word timestamps are the foundation of cut accuracy, so this directly tightens every edit boundary.

## Technologies surveyed (EXA, 8 queries)

Remotion · OpenTimelineIO · Hyperframes (HeyGen, html→video for agents) · X-Cut (MeiGen, agent × timeline × Remotion) · Diffusion Studio Core + Mediabunny (browser WebCodecs) · WebCodecs + FFmpeg.wasm · ElevenLabs Scribe vs Whisper · videotoolbox vs libx264 · RIFE · Revideo / Motion Canvas · Submagic / Opus / Gling / Descript.

---

## How to use this repo

### Read it
Just open the live link, or `open index.html` locally. No build step, no dependencies — one static HTML file, fonts loaded from Google Fonts CDN.

### Fork it as a Shaper comparison template
The whole thing is one `index.html`. To repurpose it for a different "stack A × stack B" comparison:

1. Copy `index.html`.
2. Edit the `<header>` (brand / crumb / meta) and the `.thesis` block.
3. Replace the two `.pipe` columns and the `table.cmp` rows.
4. Keep the design tokens (`:root` block) untouched — that's the Shaper register (pure B&W, JetBrains Mono, 1px frames, inversion = active/ours).

### Regenerate the social cover
The Open Graph cover is generated from `assets/og-cover.src.html`:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless=new --disable-gpu --hide-scrollbars \
  --force-device-scale-factor=1 --window-size=1200,630 \
  --virtual-time-budget=4000 \
  --screenshot=assets/og-cover.png \
  "file://$PWD/assets/og-cover.src.html"
```

The `<meta property="og:image">` / `twitter:image` tags in `index.html` point at the absolute Pages URL of that PNG, so the link unfurls with a preview in Telegram, Slack, X, etc. After editing, push and re-trigger the preview (in Telegram: send the link → "Update link previews").

### Publish your own
```bash
gh repo create <you>/<name> --public --source=. --push
gh api -X POST repos/<you>/<name>/pages -f 'source[branch]=main' -f 'source[path]=/'
```
Pages serves the root; the live URL appears within ~1–2 minutes.

---

## Files

```
index.html              — the dashboard (single file, 5 tabs)
favicon.svg             — Shaper mark (two blocks: outline = theirs, filled = ours)
assets/og-cover.png     — 1200×630 social cover
assets/og-cover.src.html— source for the cover (regenerate via the command above)
```

Researched and built with Claude Code (Opus 4.8) via EXA MCP, 2026-06-15. Comparison subject: Max Postnikov's video-editing stack. Aesthetic: AI Mindset Shaper.
