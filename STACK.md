# Reels Pipeline OS - stack spec

> Public stack specification for agentic short-form editing: source audit → ingest → transcript → source blocks → EDL → overlay lanes → cover lane → render → QA → monitor loop.

- **Live dashboard:** https://apowall.github.io/reels-pipeline-os/
- **Repo:** https://github.com/aPoWall/reels-pipeline-os
- **Last rebuilt:** 2026-06-27

This document is the downloadable operating spec behind the dashboard. It is intentionally public-safe: examples are generic, branch names are generic, and raw media/transcripts stay outside the repo.

## 1. Core Thesis

A Reels pipeline should choose the **editing contract** before it renders.

The reusable state is:

```text
source audit -> ingest/color -> transcript/timing -> source blocks -> EDL state -> overlay lanes -> cover lane -> render route -> QA -> monitor loop
```

The MP4 is an output. The durable artifact is the recipe: source blocks, cut map, overlay tracks, QA decisions and branch status.

## 1.1 Editing Protocol Stack

| id | protocol | output |
|----|----------|--------|
| P0 | brief protocol | goal, audience, source type, mode, privacy boundary, expected artifacts |
| P1 | source protocol | `source_map.md`, ffprobe facts, orientation, fps, audio channels, HDR/SDR decision |
| P2 | timing protocol | transcript JSON, word timings, pause map, dead-air/stutter hints |
| P3 | story protocol | source blocks: hook, setup, tension, proof, turn, closer, tail |
| P4 | EDL protocol | `edit_decision_list.json` with `cuts[]`, `overlay_tracks[]`, `render.outputs`, branch status |
| P5 | overlay protocol | caption/card/rail/proof/stamp/cover lanes, safe-zone rules, viewer-facing copy |
| P6 | render protocol | draft, review MP4, text-free base, final 1080x1920 MP4 |
| P7 | QA protocol | ffprobe, listening pass, first-3s check, safe-zone pass, privacy pass, monitor note |

## 2. Decision Order

### 2.1 Source

| source | default pack | notes |
|--------|--------------|-------|
| `iphone_single` | `portable-iphone-kit` | iPhone `.MOV`, HLG/bt2020, one creator |
| `multi_take` | `edl-block-editor` | repeated takes, good/bad doubles, reusable cut map |
| `collab` | `collab-spine` / `creator-collab-challenge` | interruptions and handoffs are story beats |
| `screen_system` | `proof-flow` | terminal/agent/proof overlays with redaction |
| `public_reference` | `reference-echo` | borrow mechanics, keep own footage and copy |
| `visual_board` | `cover-visual-board` | style tokens, cover references and public-safe visual mechanics |
| `public_package` | `public-dashboard` | open-source branch recipe and docs |

### 2.2 Goal

| goal | output |
|------|--------|
| `publish_fast` | fastest safe MP4 + QA |
| `reusable_recipe` | EDL + branch matrix |
| `manual_review` | block/timeline state + contact sheet |
| `public_branch` | sanitized README + sample EDL |
| `challenge` | metrics schema + collab EDL |
| `skill_update` | survivor mechanics, private skill note and public-safe protocol delta |
| `article_demo` | schematic EDL, library list and public citation paragraph |

### 2.3 Mode

Pick the editorial mode before the visual style:

| mode | use when | default surface |
|------|----------|-----------------|
| `dry_preserve` | raw take already works | clean trim, almost no overlay |
| `caption_only` | social default | sparse bottom or top captions |
| `proof_overlay` | evidence matters | one proof lane, cropped/redacted |
| `visible_flow` | process is the point | rail / graph / sequence |
| `cover_set` | cover or visual-board pass | clean no-text base, title layer and contact sheet |
| `social_short` | punch and CTA carry the reel | tight cut and minimal copy |
| `experimental` | deliberate exploration | one wild branch with clean baseline preserved |

### 2.4 Route Session

The configurator selects a route after `source`, `goal` and `mode`. Route controls which skill/system owns the next pass.

| route | owner | output |
|-------|-------|--------|
| `reel_edit` | `reel-edit` | private render spine: source audit, transcript, EDL, overlay lanes, review/final MP4, text-free base and QA |
| `block_timeline` | `reel-block-edit` | inspectable timeline board: source blocks, cuts, overlay events, contact sheet and manual trim notes |
| `palmier` | `reel-palmier-board` | editable board: base video/audio, frame shells, native text clips and winner cutlane |
| `shaper` | `shaper-reels` | public proof/process overlay: monochrome rails, labels, safe-zone contact sheet and style reuse note |
| `cover_lab` | `inside-insanity` + `reel-edit` | cover/visual-board pass: clean bases, text layer, contact sheet, style tokens and public-safe protocol delta |
| `article_research` | `stack-compare` | public article branch: research sessions, source credits, route matrix and prompt contract |

### 2.5 Soft Communication Layer

| layer | question | editing signal |
|-------|----------|----------------|
| hook | why watch | conflict, question, useful promise or visible proof in the first seconds |
| voice | how it speaks | tone, tempo, pauses, caption density, persona lane and visible process level |
| proof | why believe | screen, object, timeline, before/after, quote-safe source or action inside the frame |
| turn | why finish | reversal, conclusion, CTA, next branch and learning note |

## 3. Pack Catalog

## 3.0 Community Borrow Map

| source | strongest contribution | adopted layer |
|--------|------------------------|---------------|
| [Voronik1801/reel_pipline](https://github.com/Voronik1801/reel_pipline) | portable macOS/iPhone kit, `PIPELINE.md`, `pipeline_check.sh`, `avconvert`, templates, stutter/dead-air review | P1 source, P2 timing, P7 QA |
| [FeezRM/AI-ShortForm-Editor](https://github.com/FeezRM/AI-ShortForm-Editor) | transcript, scene/silence/audio rhythm, EDL, confidence gate, vertical render | P2 timing, P4 EDL, P7 QA |
| [CarlAmine/AI_Editor](https://github.com/CarlAmine/AI_Editor) | OCR/SceneDetect, brief-to-edit-plan JSON, overlay planner, timeline render | P1 source, P3 story, P5 overlay |
| [MeiGen-AI/X-Cut](https://github.com/MeiGen-AI/X-Cut) | chat-driven video agent, dynamic skills, asset analysis, real-time Remotion timeline | P0 brief, P3 story, future editor surface |
| [birchrust/OpenScript](https://github.com/birchrust/openscript) | MCP tools, EDL v2, multi-track timeline, verification layer | P4 EDL, P7 QA, agent control |
| [floomhq/opencut](https://github.com/floomhq/opencut) | TypeScript timeline-as-code, Whisper word captions, validate/render CLI, Remotion route | P4 EDL, P5 captions, P6 render |
| [neutral-Stage/remotion-captioneer](https://github.com/neutral-Stage/remotion-captioneer) | word-level caption components, presets, SRT/VTT/ASS exports | P2 timing, P5 caption lane |
| [heygen-com/HyperFrames](https://github.com/heygen-com/hyperframes) | HTML/CSS/media render loop, CLI lint/preview/render, agent skills | P5 overlays, P6 render |
| [jurczykpawel/ReelStack](https://github.com/jurczykpawel/reelstack) | CLI/API production stages, provider registry, prompt templates, self-hosting | P0 brief, P6 render, automation |
| [tsensei/OpenReels](https://github.com/tsensei/OpenReels) | topic-to-short pipeline: research, script, voiceover, visuals, music, captions, assembly | article-demo pack |
| [OpenTimelineIO](https://github.com/AcademySoftwareFoundation/OpenTimelineIO) | editorial timeline interchange format | EDL export and NLE bridge |
| [Mediabunny](https://mediabunny.dev/guide/output-formats) / [WebCodecs](https://www.w3.org/TR/webcodecs/) | browser media containers and decode/encode primitives | future browser preview/export |
| [thumbnail-guru](https://github.com/sebastianhardy/thumbnail-guru), [Adobe Express](https://www.adobe.com/express/create/ai/thumbnail), [Canva](https://www.canva.com/ai-thumbnail-maker/) | clean base, separate text layer, variant contact sheet, small-size QA | cover-visual-board |

### 3.1 `portable-iphone-kit`

Use for one-creator iPhone footage and a fast production handoff.

**Borrowed mechanics from `Voronik1801/reel_pipline`:**

- `pipeline_check.sh` before creative work;
- Apple `avconvert -p Preset1920x1080` for iPhone HLG/bt2020 → SDR bt709;
- copy-fill-run templates (`build_TEMPLATE.py`, `make_reel_TEMPLATE.py`);
- stutter review via long words, pauses and near-repeats;
- RMS dead-air detector;
- replaceable subtitle, cover, finale, music and sticker slots;
- final QA by frames and re-transcription.

**Artifacts:**

- `source_map.md`
- `raw_sdr/*.mov`
- `transcripts/*.json`
- `edit_decision_list.json`
- `QA.md`
- `renders/final.mp4`

**Canon:**

```bash
ffprobe -v error -select_streams v -show_entries stream=color_transfer,color_primaries,color_space -of default=nw=1 raw/SHOT.MOV
ffprobe -v error -select_streams v -show_entries side_data=rotation -of default=nw=1 raw/SHOT.MOV
avconvert -p Preset1920x1080 -s raw/SHOT.MOV -o raw_sdr/SHOT.mov --replace
```

Then grade/render from SDR and tag exports as bt709.

### 3.2 `edl-block-editor`

Use when the edit should be reusable and inspectable.

**Adds:**

- source blocks;
- `cuts[]` as single source of truth;
- independent `overlay_tracks[]`;
- branch surface: `clean`, `captions`, `micro`, `low_rail`, `soft_card`, `challenge`;
- preview state for a timeline board UI;
- render adapters for ffmpeg, Remotion or browser.

### 3.3 `caption-lane`

Use when subtitles and short viewer-facing text are the main social layer.

**Rules:**

- transcript must include word timings;
- captions stay sparse and safe-zone aware;
- export a portable subtitle file when useful: ASS, SRT or WebVTT;
- burn-in captions can be rendered as PNG overlays or ffmpeg/libass;
- review the first 3 seconds, face/object overlap and final mobile readability.

### 3.4 `collab-spine`

Use for creator exchanges where the story lives in the handoff.

**Contract:**

```json
{
  "collabBeats": [
    {
      "id": "beat_01",
      "speaker": "creator_a",
      "start": 0.0,
      "end": 2.8,
      "type": "handoff | interruption | acknowledgement | response",
      "preserve_reason": "sets the next turn"
    }
  ]
}
```

Public branch files can say `creator_a`, `creator_b`, `turn`, `hook`, `handoff`, `motif_insert`. They must not include real handles, private screenshots, raw transcript text or local paths.

### 3.5 `creator-collab-challenge`

Use when the collab is also a public growth experiment.

**Metrics:**

| metric | meaning |
|--------|---------|
| `followers_start` | baseline follower count |
| `followers_current` | current public count |
| `followers_delta` | current minus baseline |
| `content_count` | posts/reels inside the window |
| `prep_minutes` | preparation time per artifact |
| `paid_ads_allowed` | default `false` |
| `trial_results` | aggregated Trial Reels summary |

### 3.6 `proof-flow`

Use for terminal, product, agent, workflow and proof reels.

**Rules:**

- crop and redact before render;
- one proof lane per beat;
- no secrets, tokens, emails or private links;
- viewer-facing text only;
- proof must be readable on mobile.

### 3.7 `reference-echo`

Use when a public reference gives a useful visual grammar.

**Contract:**

- extract mechanics: frame, contrast, caption density, pacing, transition logic;
- use own footage and own words;
- one borrowed mechanic per branch;
- document source attribution in private notes or public-safe sources tab.

### 3.8 `public-dashboard`

Use for open-source packaging.

**Artifacts:**

- `index.html`
- `STACK.md`
- `CONTRIBUTING.md`
- `branches/*/README.md`
- `branches/*/edit_decision_list.json`
- `assets/og-cover.png`

### 3.9 `cover-visual-board`

Use for cover sets, visual-board updates and style survivor extraction.

**Rules:**

- start from a private visual board or reel review, then strip it to mechanics;
- generate or select clean no-text cover bases;
- keep title/caption as a separate editable layer;
- produce 5-10 variants and one contact sheet;
- check readability at small mobile-preview size;
- write `style_tokens.json` and `public_pipeline_delta.md`;
- publish only generated/sanitized imagery and generic mechanics.

**Artifacts:**

- `visual_board.md`
- `covers/clean/*.png`
- `covers/text/*.png`
- `cover_contact_sheet.png`
- `style_tokens.json`
- `public_pipeline_delta.md`

## 3.10 AI Mindset Skill System Placement

Public-safe skill chain:

```text
inside-insanity -> reel-edit -> reel-block-edit -> reel-palmier-board -> shaper-reels -> stack-compare -> GitHub Pages
```

| layer | role |
|-------|------|
| `inside-insanity` | defines personal visual board, motif, tone, caption grammar, cover references and public-safe summary |
| `reel-edit` | owns private video state: source audit, transcript, source blocks, EDL, overlay lanes, cover set, render outputs and QA |
| `reel-block-edit` | visual/timeline surface: branch surface, overlay density, manual review and safe-zone checks |
| `reel-palmier-board` | editable review and winner cutlane board with base tracks, frame shells and native text clips |
| `shaper-reels` | monochrome proof/process overlay grammar for public-safe visible flow |
| `stack-compare` | owns public dashboard, library stack, research sessions, sanitized examples, README and STACK.md |
| GitHub Pages | publishes the public artifact and verifies live tab text after release |

Raw media, transcripts and local paths stay in the private video project. The public artifact carries mechanics only.

## 4. EDL Schema

Generic public-safe shape:

```json
{
  "schema": "reel.edl.v3",
  "source": {
    "kind": "iphone_single | multi_take | collab | screen_system | public_reference | visual_board",
    "media": "private/source.mov",
    "public_name": "source_take_a"
  },
  "strategy": {
    "pack": "portable-iphone-kit",
    "mode": "caption_only | cover_set",
    "style": "minimal_caption",
    "audio_policy": "single_source"
  },
  "source_blocks": [
    {
      "id": "b01",
      "role": "hook",
      "summary": "public-safe summary",
      "sourceStart": 0.0,
      "sourceEnd": 3.8
    }
  ],
  "cuts": [
    {
      "id": "c01",
      "block": "b01",
      "sourceStart": 0.0,
      "sourceEnd": 3.8,
      "outStart": 0.0,
      "outEnd": 3.8,
      "risk": "baseline | insert | experimental | safe-zone-watch"
    }
  ],
  "overlay_tracks": [
    {
      "id": "captions",
      "type": "caption",
      "items": [
        {
          "start": 0.4,
          "end": 2.6,
          "text": "viewer-facing caption",
          "safeZone": "lower-third"
        }
      ]
    }
  ],
  "cover": {
    "basePolicy": "clean_no_text",
    "textLayer": "separate",
    "variants": 10,
    "qa": "small_size_readability"
  },
  "render": {
    "route": "ffmpeg | remotion | hyperframes | browser",
    "outputs": {
      "review": "renders/review.mp4",
      "final": "renders/final_1080x1920.mp4"
    },
    "video": {"width": 1080, "height": 1920, "fps": 24, "colorspace": "bt709"},
    "audio": {"codec": "aac", "channels": 2, "sampleRate": 48000, "targetLufs": -16}
  },
  "qa": {
    "ffprobe": "pending",
    "contact_sheet": "pending",
    "listening_pass": "pending",
    "privacy_pass": "pending"
  }
}
```

## 5. Overlay Lane Contract

| lane | rule |
|------|------|
| `caption` | one short phrase per beat; viewer-facing |
| `card` | one conceptual beat; no permanent dashboard wallpaper |
| `rail` | process sequence, thin lower/side lane |
| `stamp` | small context marker |
| `proof` | only for proof mode; redacted |
| `cover` | clean base plus separate title/caption layer; never burn private text into reusable base |

On-screen copy must not include internal terms like `EDL`, `branch`, `render`, `segment`, file paths, source filenames or implementation notes.

## 6. Audio Policy

- Default: one continuous voice master.
- Cross-take speech splice: explicit experimental branch only.
- Preserve a clean baseline before experiments.
- Listen to every speech cut boundary.
- Avoid per-segment filters on iPhone MOV.
- Reject silent or suspicious AAC output.

## 7. QA Gate

### Preflight

```bash
command -v ffmpeg
command -v ffprobe
command -v avconvert
python3 -c "import PIL, numpy"
```

### Render

- 1080×1920 vertical output;
- fps stable;
- `yuv420p`;
- SDR `bt709` tags;
- AAC stereo 48k;
- target loudness around −16 LUFS;
- contact sheet for hook, overlay, transition point and ending;
- listening pass;
- privacy pass.

### Public Packaging

Public files must exclude:

- raw footage;
- private transcript text;
- real names and handles;
- screenshots with private UI;
- local paths and filenames;
- provider keys;
- chat IDs and internal CRM details.

## 8. External Research Synthesis

EXA MCP research and primary docs point to a converging pattern:

| source | useful role |
|--------|-------------|
| [Dasha / Voronik1801 reel_pipline](https://github.com/Voronik1801/reel_pipline) | portable iPhone production kit: `PIPELINE.md`, `avconvert`, `pipeline_check.sh`, templates, stutter/dead-air review |
| [FeezRM/AI-ShortForm-Editor](https://github.com/FeezRM/AI-ShortForm-Editor) | transcript, scene/silence/audio rhythm analysis, EDL, confidence gate and vertical render |
| [CarlAmine/AI_Editor](https://github.com/CarlAmine/AI_Editor) | OCR/SceneDetect, brief-to-edit-plan JSON, overlay planner and render timeline |
| [floomhq/opencut](https://github.com/floomhq/opencut) | TypeScript timeline-as-code, Whisper word captions, validate/render CLI and Remotion render |
| [neutral-Stage/remotion-captioneer](https://github.com/neutral-Stage/remotion-captioneer) | word-level caption components, presets, STT providers and SRT/VTT/ASS exports |
| [OpenTimelineIO](https://github.com/AcademySoftwareFoundation/OpenTimelineIO) | editorial interchange: cut information, timing, external media refs and adapter direction |
| [Remotion renderer docs](https://www.remotion.dev/docs/renderer/render-media) and [timeline render docs](https://www.remotion.dev/docs/timeline/render) | typed tracks/items, Player sync and JSON `inputProps` as render state |
| [Mediabunny output formats](https://mediabunny.dev/guide/output-formats) | browser media containers and explicit codec/container constraints |
| [HyperFrames](https://github.com/heygen-com/hyperframes) | HTML/CSS/media to deterministic MP4, agent skills, CLI validation and render loop |
| [X-Cut](https://github.com/MeiGen-AI/X-Cut) | chat-driven video agent, multi-track timeline, reusable style skills and Remotion render |
| [OpenScript](https://github.com/birchrust/openscript) | MCP tools, EDL v2, multi-track timeline, verification layer and agent-directed render |
| [AI Video Editor](https://github.com/tjameswilliams/ai-video-editor) | natural-language edits, multi-track timeline and Remotion motion graphics |
| [Elah](https://github.com/elahlabs/elah), [timeline](https://github.com/webpacked/timeline), [OpenReel Video](https://github.com/Augani/openreel-video) | browser-native editor direction: timeline core, UI adapter and local preview |
| [ReelStack](https://github.com/jurczykpawel/reelstack) | API/CLI production pipeline, templates, effects, provider registry and self-hosting |
| [OpenReels](https://github.com/tsensei/OpenReels) | topic-to-short pipeline, live pipeline visualization, provider mix and critique/rerun loop |
| [Later Reels guide](https://later.com/blog/instagram-reels/), [Captions.ai Reels guide](https://captions.ai/blog/how-to-make-instagram-reels) | third-party platform heuristics: first seconds, captions, 9:16, watch time and shares |
| [thumbnail-guru](https://github.com/sebastianhardy/thumbnail-guru), [Adobe Express thumbnail maker](https://www.adobe.com/express/create/ai/thumbnail), [Canva AI thumbnail maker](https://www.canva.com/ai-thumbnail-maker/) | cover workflow: clean base, separate text layer, multiple variants and readability QA |
| [Whisper](https://github.com/openai/whisper), [FFmpeg](https://ffmpeg.org/ffmpeg.html), [WebCodecs](https://www.w3.org/TR/webcodecs/) | speech timing, render/diagnostics and future browser media primitives |

Practical conclusion: keep the state model small and portable, then route rendering by use case.

## 9. Agent Builder Prompt

```text
Build a public-safe Reels pipeline branch.

Selected source: <iphone_single | multi_take | collab | screen_system | reference | visual_board>
Selected goal: <publish_fast | recipe | reviewable | public_branch | skill_update | article_demo>
Selected mode: <dry_preserve | caption_only | proof_overlay | visible_flow | cover_set | experimental>
Selected route: <reel_edit | block_timeline | palmier | shaper | cover_lab | article_research>

Use this stack:
- Python 3.10+ orchestration, JSON files as contracts, sequential render queue.
- ffmpeg and ffprobe for source audit, crop, scale, concat, audio extraction, AAC, bt709 tags and diagnostics.
- On macOS/iPhone footage, use Apple avconvert for HDR/HLG to SDR bt709 before creative render.
- Whisper or Scribe for transcript, word timings and pause map.
- PySceneDetect/librosa/EasyOCR/PaddleOCR where scene, rhythm or proof analysis matters.
- Pillow and numpy for PNG caption overlays, contact sheets and review frames.
- Cover lab rule: clean no-text bases, independent title/caption layer, 5-10 variants and small-size readability QA.
- EDL JSON as the source of truth: source_blocks[], cuts[], overlay_tracks[], render.outputs and qa.
- Optional adapters: OpenTimelineIO export, Remotion tracks/items render, HyperFrames HTML render, Mediabunny/WebCodecs browser preview.

Required protocols:
- P0 brief: goal, audience, source type, mode, privacy boundary.
- P1 source: ffprobe, orientation, audio, color/HDR check, source map.
- P2 timing: transcript, word timings, pause map, dead-air/stutter hints.
- P3 story: source blocks, hook, tension, proof, turn, closer.
- P4 EDL: cuts[], overlay_tracks[], render.outputs, branch status.
- P5 overlay: caption, card, rail, proof, stamp, cover, safe zones.
- P6 render: draft, review MP4, text-free base, final 1080x1920.
- P7 QA: ffprobe, listening pass, first 3s, privacy pass, monitor note.

Route process:
- `reel_edit`: build source map, transcript, EDL, overlay lanes, review/final MP4, text-free base and QA.
- `block_timeline`: export source blocks, cut map, overlayEvents, board state, safe-zone contact sheet and manual review notes.
- `palmier`: create base video/audio references, frame shells, native text clips, editable board state and winner cutlane.
- `shaper`: apply monochrome proof/process overlay grammar to a clean baseline and run safe-zone QA.
- `cover_lab`: build clean cover bases, separate text layer, contact sheet, style tokens and public-safe protocol delta.
- `article_research`: summarize sources as signal, diagnosis and move; update public article, route matrix and prompt contract.

Same-source comparison:
1. Use the same source_map, transcript baseline and audio policy when comparing routes.
2. Save outputs under test_runs/<route>/.
3. Compare first_3s, safe_zone, cover_readability, editability, render_qa and publish_readiness.
4. Write one route verdict: keep, rerun with notes, or archive.

Public boundary:
- Keep raw footage, transcript text, real names, handles, local paths, private screenshots and keys outside public artifacts.
- Public files may include generic timing, labels, EDL shape, overlay lane types, QA decisions and source links.

Return:
- branch pack summary;
- commands or scripts to run;
- JSON EDL skeleton;
- QA checklist;
- next learning note.
```
