# AI Mindset Reels Pipeline OS – the full stack

> An open, community OS for **agentic short-form (Reels) editing**: a stack you fork, configure, adapt, and send your branch back to.

This file is the downloadable export of the whole stack: every layer, the tools, the contracts between layers, the EDL schema, the skills, and how to run it. All examples are **generic** – no personal footage, names, or content (see [CONTRIBUTING](CONTRIBUTING.md) for the privacy rule).

- **Live dashboard:** https://apowall.github.io/reels-pipeline-os/
- **Repo (fork me):** https://github.com/aPoWall/reels-pipeline-os

---

## Where this came from

It started inside **AI Mindset {space}** as a small Shaper dashboard prototype: map your montage contour, make the reusable parts visible, and fold useful mechanics back into the stack. Then the package absorbed a public portable iPhone workflow (`Voronik1801/reel_pipline`), an answer-card style branch, and a creator-collab challenge branch.

From there it grew into a reusable **pipeline OS**: a worked example (the "answer-card" recipe), a family of overlay-density branches, a portable iPhone kit, and an open contribution model so other people's branches live next to ours.

**Thesis:** *a short-form editor should store the **recipe** first: source blocks + EDL + overlay lanes + QA/audio. The final mp4 becomes an output of a pinned, reproducible, forkable editing state.*

---

## The stack, layer by layer

```
transcript → source blocks → EDL (SSOT) → overlay lanes → draft → QA → final → publish
```

Each layer has a **tool** and a **contract** (what it must guarantee to the next layer).

| # | layer | tool | contract |
|---|-------|------|----------|
| 0 | **transcript** | ElevenLabs Scribe (word-level) · Whisper/WhisperX local fallback | word-level timecodes; cuts only land in pauses, never mid-word |
| 1 | **source blocks** | transcript + manual editorial map | rhetorical blocks (hook/tension/turn/setting/punch/theme/closer); each block is a self-contained thought from one source |
| 2 | **EDL (SSOT)** | `edit_decision_list.json` | the single source of truth: every cut + overlay decision lives here, not in code; draft and final render from the same file |
| 3 | **overlay lanes** | overlay renderer (ffmpeg + OpenCV/Pillow) | lanes (`caption`/`card`/`rail`/`stamp`) are **independent of the cut map**; on-screen copy stays viewer-facing – no proof/render/EDL labels in frame |
| 4 | **draft** | ffmpeg (videotoolbox HW, low-res) | cheap fast preview from the EDL to check cut/timing before the hero render |
| 5 | **QA** | ffprobe + contact sheet | resolution/fps/audio checks; frame-strip sanity at hook/card/rail/end; safe-zone pass; gate against silent audio / tiny AAC bitrate |
| 6 | **final** | ffmpeg (libx264 CRF) | 1080×1920, one continuous voice master (AAC stereo 48kHz), loudnorm; audio-first lip-sync (stream-copy, no per-segment filters) |
| 7 | **publish** | Instagram Trial Reels (A/B) | publish two versions when goals differ; watch-time, not views; deterministic metadata |

**Audio-first rule (critical) – default + exception:**
- *default* – one reel = one continuous voice master; visual variety comes from swapping *video* in speech pauses (visual swap), not from cutting speech.
- *exception* – a cross-take speech splice is allowed **only as an explicit experimental branch** (`single_source_multi_take_remix_experimental`), with a clean baseline preserved and the seam checked by listening.

A/B audio alternation *by default* is the #1 cause of bad reels. A consciously marked experimental remix (same source/mic/place) is a valid branch when the baseline is preserved.

---

## Modes – pick the editorial job before the visual style

Choose the **mode** (what the edit is *for*) before the style (how it *looks*):

| mode | when | default output |
|------|------|----------------|
| `dry_preserve` | source already works | clean trim, no overlay |
| `caption_only` | social default, sound-off readability | sparse bottom captions |
| `proof_overlay` | proof matters (terminal / receipt) | one lower/side proof overlay |
| `visible_flow` | the process/sequence is the point | timeline rail / graph |
| `social_short` | punch + CTA are enough | tighter cut, minimal captions |
| `experimental` | explicit exploration | one wild branch per batch |

Style (`terminal / cinematic / minimal / hybrid / tutorial / sermon`) is chosen *after* the mode. ~35% of viewers watch sound-off → captions are not optional.

---

## EDL schema (generic example)

The Edit Decision List is the heart of the stack. Generic shape:

```json
{
  "schema": "reel.edl.v2",
  "source": "source/take.mov",
  "strategy": {
    "mode": "caption_only | visible_flow | proof_overlay | experimental",
    "style": "answer_card_terminal_caption | low_rail | soft_card | ...",
    "audio_policy": "single_source | single_source_multi_take_remix_experimental"
  },
  "cuts": [
    {
      "id": "c01",
      "label": "hook",
      "sourceStart": 0.0, "sourceEnd": 3.7,
      "outStart": 0.0, "outEnd": 3.7,
      "risk": "baseline | insert | remix | safe-zone-watch",
      "note": "generic public-safe note"
    }
  ],
  "overlay_tracks": [
    { "id": "captions", "type": "caption",
      "items": [ {"start": 0.8, "end": 2.8, "text": "caption", "safeZone": "lower-third", "visibleToViewer": true} ] },
    { "id": "cards", "type": "card",  "items": [] },
    { "id": "rail",  "type": "rail",  "items": [] },
    { "id": "stamp", "type": "stamp", "items": [] }
  ],
  "render": {
    "outputs": { "draft": "renders/draft_360p.mp4", "final": "renders/final_1080x1920.mp4" },
    "video": { "width": 1080, "height": 1920, "fps": 24 },
    "audio": { "codec": "aac", "channels": 2, "sampleRate": 48000, "targetLufs": -16 }
  }
}
```

Notes:
- `cuts[]` carry `sourceStart`/`sourceEnd` so the same map renders by MoviePy, ffmpeg, or (later) Remotion/OTIO.
- `insert: true` marks a cross-take splice – only allowed in a branch explicitly tagged experimental, with a clean baseline preserved (`single_source_multi_take_remix` policy).
- `overlay_tracks[]` are independent lanes; they do not move when the cut map changes.

---

## Contracts – overlay lanes, audio, QA

**Overlay lane contract** (each lane has one job; lanes are independent of the cut map):

| lane | rule |
|------|------|
| `caption` | sparse bottom copy, max one short phrase per beat |
| `card` | one conceptual beat – never constant dashboard wallpaper |
| `rail` | visible flow/state sequence, thin lower/side lane |
| `stamp` | tiny recurring context label |
| `proof` | only in `proof_overlay` / technical demo, never a default |

**Visible-copy ban (public / viewer reels):** never put `branch`, `render`, `EDL`, `QA`, `segment`, source filename, local path, `proof`, or implementation notes *on screen*. On-screen text is viewer-facing only.

**Audio policy:**
- default: one continuous voice master.
- multi-take speech splice only as an **explicit experimental branch** – same source/mic/place, clean baseline preserved, seam checked by listening.
- avoid per-segment audio filters on iPhone MOV; build one master, then overlay/mux.
- reject silent / tiny AAC: near −90 dB, mono mismatch, suspiciously low bitrate, clipping.

**QA gate (before publish):**
- `ffprobe`: resolution, fps, duration, audio codec/channels/sample-rate/bitrate.
- loudness: ~−16 LUFS, sane peak limiter.
- contact strip: hook, overlay/card/rail moments, seam, ending.
- safe-zone: captions above IG controls; face/body/object not covered.
- **viewer pass:** would a cold viewer understand the idea without reading internal labels?

---

## Branch surface – one cut map, many overlay densities

A single soft cut map can carry a ladder of overlay densities – pick the branch by goal:

| branch | overlays |
|--------|----------|
| `clean`     | none – soft phrase-level cuts, stereo master (baseline) |
| `captions`  | captions only |
| `micro`     | tiny stamp + sparse captions |
| `low_rail`  | low-opacity timeline rail + sparse captions |
| `soft_card` | compact low-opacity card (most explicit) |

KPI-aware (from short-form research): **seamless cuts → likes (fluency)**, **overlapping cuts → completion/rewatch (prediction-error)**. Match the cut to the KPI; publish two versions when goals differ and A/B them.

---

## Configurator – choose your pack

Use this before running the stack. A branch pack is a set of contracts, tools, and QA gates.

| pack | use when | includes | skip |
|------|----------|----------|------|
| `portable-iphone-kit` | one creator wants a transferable iPhone workflow | preflight, avconvert HDR→SDR, templates, stutter/dead-air review, cover/finale slots | block-editor complexity |
| `edl-block-editor` | the edit must become reusable state | source blocks, branch surface, trim, overlay lanes, `realself.timeline.json` | account-specific cover/finale |
| `inside-insanity` | Alex personal account / @_inside_insanity | raw Lisbon/iPhone/Fuji DNA, `_inside_insanity` marks, lowercase captions, color-return/kitchen/object patterns | team brand voice |
| `creator-collab-challenge` | public collab growth experiment | aggregate metrics, preserved interruptions, generic collab EDL, no-personal-data boundary | private CRM details |
| `public-dashboard` | publish mechanics as an open repo | `STACK.md`, `CONTRIBUTING.md`, sanitized branch folders, Shaper HTML dashboard | raw footage, real captions, handles |

Minimum local install:

```bash
brew install ffmpeg
python3 -m pip install pillow numpy opencv-python
# optional: openai-whisper or a cloud transcription provider
```

macOS/iPhone pack:

```bash
command -v avconvert
ffprobe -v error -select_streams v -show_entries side_data=rotation -of default=nw=1 raw/SHOT.MOV
avconvert -p Preset1920x1080 -s raw/SHOT.MOV -o raw_sdr/SHOT.mov --replace
```

Agent prompt:

```text
read STACK.md and the selected branch README.
run the preflight checks.
build a public-safe EDL/branch recipe.
keep private media, names, paths, transcripts, and keys out of public artifacts.
```

---

## Branch: portable iphone kit

Use this branch when a creator wants a **portable, human-handoff-friendly iPhone Reels pipeline**. It is inspired by the public `Voronik1801/reel_pipline` repo, but stored here as generic mechanics.

**What it is strong at:**

- `pipeline_check` before creative work;
- Apple `avconvert` for iPhone HLG/HDR → SDR color that looks like QuickTime/iPhone;
- copy-and-fill shoot templates (`build_*`, `make_reel_*`) that an agent or human can complete;
- stutter review from long word durations, near-repeats, and micro-pauses;
- dead-air review from RMS drops when `silencedetect` is not enough;
- identity slots for cover/finale/music/stickers, so account style is replaceable.

**What we borrow into the OS:**

- preflight becomes a required first-run step;
- iPhone color is a pipeline layer with explicit SDR normalization;
- transcript timing must be checked against audio energy;
- template discipline makes the stack handoff-friendly.

**What stays different:**

The portable kit is a strong linear creator pipeline. AI Mindset Reels Pipeline OS adds EDL-as-SSOT, branch surface, overlay lanes, block-editor preview, Trial Reels strategy, and skill memory. In other words: the kit is a good **single-account skeleton**; the OS turns it into a forkable editing state machine.

Public branch files: `branches/portable-iphone-kit/README.md` and `branches/portable-iphone-kit/edit_decision_list.json`.

---

## Branch: creator-collab challenge

Use this branch when a collab reel is also a **public creator-growth challenge**. The private project may know the real creators and raw footage; the public branch stores only the reproducible mechanics.

**Commitment pattern:**

- goal: first creator to reach `10000` followers;
- rule: `paid_ads_allowed = false` / no targeted ads;
- stake: the loser publishes a short public post or reel marketing the winner's product;
- loop: publish collab reels, log public follower deltas, compare preparation time, and keep the challenge visible.

**Public metrics schema:**

| metric | meaning |
|--------|---------|
| `followers_start` | baseline follower count at challenge start |
| `followers_current` | latest visible follower count |
| `followers_delta` | current minus baseline |
| `content_count` | posts/reels published inside the challenge window |
| `prep_minutes` | creator-reported preparation time per artifact |
| `paid_ads_allowed` | boolean; default `false` |
| `publish_window` | dates included in the sprint |
| `trial_results` | optional Trial Reels result summary, aggregated only |

**Collab reel recipe:**

- preserve live handoffs, interruptions, and short acknowledgements as story beats;
- keep one audio spine unless a cross-source insert is explicitly marked;
- use motif objects or scene actions as transitions: plant/onion, chair, laptop, agent screen;
- visual language: full-screen real motif first, minimal direct typography, white text with one yellow keyword, tiny star accents if useful;
- avoid oversized cards/pills over faces; no production labels in frame;
- EDL keeps `collabBeats[]` with `preserve_reason` for every interruption/handoff.

**Privacy boundary:** public docs can say `host_creator`, `guest_creator`, `creator_a`, `creator_b`, `hook`, `turn`, `handoff`, and `motif_insert`. They must not include real names, handles, private screenshots, raw transcript text, local filenames, local paths, or provider keys.

---

## The skills (how the agent runs it)

| skill | owns | version |
|-------|------|---------|
| **`reel-edit`** | video/EDL state: transcription, smart cut, modes/styles, presets, audio-first render, collab handoff preservation, draft→final | 3.17 |
| **`reel-block-edit`** | portable block-editor contract `source blocks → branch surface → trim → overlay lanes → export`; EDL strip, overlay lanes as first-class tracks, `realself.timeline.json` manifest (`source_blocks`, `cut_maps`, `overlay_tracks`, `branches`, `interlocks`, `renderer_adapters`), dashboard export | 1.8 |
| **`stack-compare`** | legacy skill name for the Shaper public package: B&W dashboard/configurator, live EDL layer, challenge branches, research matrix, and public/private boundary (dashboard agent shapes/sanitizes/deploys, source agent owns the real EDL/project) | 1.8 |
| **`inside-insanity`** | Alex personal Instagram layer: visual DNA, hook/caption grammar, cover/story routing, and survivor patterns for @_inside_insanity | 1.0 |

Division of labor: `reel-edit` owns the video state; `stack-compare` owns the public Shaper package. The agent and human argue at the level of **blocks, branches, and overlay lanes** – a shared, reproducible editing state.

---

## Current research signals

EXA research on 2026-06-25 shows the open-source short-form video space converging on the same primitives:

- transcript-first editing with word-level timing (`claude-shorts`, ReelForge, OpenScript);
- EDL/multi-track timeline state (`OpenScript`, AI video editor, Remotion timeline docs);
- Remotion as the common JSON/React render adapter (`OpenCut`, ReelForge, X-Cut, Remotion docs);
- browser/live preview via WebCodecs/Mediabunny-style engines;
- agent-facing MCP/tool layers for timeline operations.

Sources used in the dashboard/research pass:

- `https://github.com/Voronik1801/reel_pipline`
- `https://github.com/AgriciDaniel/claude-shorts`
- `https://github.com/birchrust/openscript`
- `https://github.com/floomhq/opencut`
- `https://github.com/pedro199288/reelforge`
- `https://github.com/akira231097/reelforge`
- `https://github.com/MeiGen-AI/X-Cut`
- `https://www.remotion.dev/docs/building-a-timeline`
- `https://www.remotion.dev/docs/renderer/render-media`
- `https://www.remotion.dev/docs/timeline/render`

**Verdict:** the market is catching up to `transcribe → EDL → tracks → render`. Our moat should be the branch surface, viewer-first overlays, account-specific style memory, public/private split, and Trial Reels feedback loop.

---

## What we borrowed + roadmap

- **Borrowed:** montage plan as SSOT (→ OpenTimelineIO), draft→final two-tier render, ElevenLabs Scribe for word timestamps, source-range discipline.
- **Not ported:** long-form YouTube deterministic chain, heavy asset pipeline, YouTube API – Reels need branch surface and fast preview.
- **Roadmap:** `realself.timeline.v1` → OTIO `.otio`; WebCodecs + Mediabunny live preview in the block editor; ProRes-4444 alpha overlay lane; cloud render via Remotion Lambda or Hyperframes (HTML-native, agent-first).

Full technology survey (12 EXA queries: Remotion, OTIO, Hyperframes, X-Cut, CutAgent, libass vs HTML caption engines, multi-take selection, jump-cut engagement study, …) lives in the dashboard's **технологии** tab.

---

## How this stack is maintained – a two-agent loop

This stack is kept by **two agents talking over Murmur** (agent-to-agent):

- a **source agent** (Codex) owns the real EDL / project / renders – private, with real names and footage;
- a **dashboard agent** (Claude Code) owns this public package – it only ever publishes **generic, sanitized mechanics**.

The loop: the source agent cuts and assembles branches, then sends a **generic delta** (layers, EDL schema, contracts) over Murmur; the dashboard agent sanitizes it and folds it into the public stack. Personal content never crosses into the public repo (see [CONTRIBUTING](CONTRIBUTING.md): no real footage / names / on-screen text). The ownership boundary and the no-personal-data rule are encoded in the `stack-compare` skill.

It's a pattern worth stealing: **let the agent that holds the private state hand only reproducible structure to the agent that publishes.**

---

## Run it

This repo ships the **dashboard** (a single static `index.html`) and this stack doc. The editing skills (`reel-edit`, `reel-block-edit`, `stack-compare`) drive the actual rendering inside Claude Code / Codex. To reproduce the pipeline you need: ffmpeg (with libx264), Python (OpenCV, Pillow), a transcription provider (Scribe or local Whisper), and the skills.

To **fork the dashboard** as your own Pipeline OS artifact, copy `index.html`, edit the header/thesis and branch packs, keep the Shaper design tokens. See the dashboard README.

To **contribute your branch**, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Portability – why the skills work anywhere

The skills are portable because the public docs describe **contracts, not machine paths**: the EDL schema, manifest fields, overlay-lane contract, audio policy, QA gate, install requirements, and first-run sequence. Anyone can stand the stack up in their own Claude Code / Codex from these contracts. Absolute project examples stay private; **public examples are schematic EDL fragments or sanitized branch folders** – never real footage or local paths.

No provider secrets live here. If a renderer uses transcription, image generation, cloud render, or Instagram export helpers, keys stay in the contributor's local environment and the public repo documents only the pipeline contract.
