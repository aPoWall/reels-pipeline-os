# Reels Pipeline OS

> **Public atlas, stack spec and route configurator for agentic Reels editing.**
> Live: https://apowall.github.io/reels-pipeline-os/

This repo is a public **Reels Pipeline OS** for short-form editing. It explains the shared editing state first: brief, source audit, transcript, source blocks, EDL, overlay lanes, render route, cover lane, QA and public/private boundary. The configurator is the practical layer: it turns that map into a route session, branch pack, JSON config and agent prompt.

It is built from public-safe editing mechanics, the `stack-compare` package pattern, and an audit of public video-pipeline repositories including `Voronik1801/reel_pipline`.

![cover](assets/og-cover.png)

## Start Here

| file | what |
|------|------|
| **[index.html](index.html)** | public atlas, protocol map, research sessions, skill route map and live configurator |
| **[PROTOCOLS.md](PROTOCOLS.md)** | P0-P7 editing protocols and community borrow map |
| **[STACK.md](STACK.md)** | stack spec: packs, contracts, EDL schema, QA gates, source research, agent prompt |
| **[ARTICLE.md](ARTICLE.md)** | short public description and citation notes for articles |
| **[CONTRIBUTING.md](CONTRIBUTING.md)** | how to contribute a branch without leaking private data |
| **[branches/](branches/)** | public-safe branch recipes and sample EDLs |

## What Changed

The current version leads with the **public montage atlas**:

1. see how different editing approaches become one `editing state`;
2. inspect the public pipeline contract: mode, audio, EDL, overlays, QA;
3. inspect editing protocols P0-P7: brief, source, timing, story, EDL, overlays, render, QA;
4. inspect the soft communication layer: hook, voice, proof, turn;
5. review the research map: Voronik1801/reel_pipline, FeezRM/AI-ShortForm-Editor, CarlAmine/AI_Editor, OpenScript, X-Cut, AI Video Editor, floomhq/OpenCut, remotion-captioneer, OpenTimelineIO, Remotion, Mediabunny, cover generators and platform QA guides;
6. inspect how the public artifact fits the AI Mindset skill system: inside-insanity -> reel-edit -> reel-block-edit -> reel-palmier-board -> shaper-reels -> stack-compare -> GitHub Pages;
7. inspect the library stack: ffmpeg, ffprobe, avconvert, Whisper/Scribe, PySceneDetect, librosa, EasyOCR/PaddleOCR, Pillow, numpy, ASS/SRT/WebVTT, OTIO, Remotion, HyperFrames, Mediabunny, WebCodecs, MCP;
8. choose `source`, `goal`, `mode` and `route` in the configurator;
9. copy the generated protocol stack, JSON config and agent prompt.

## Route Sessions

The current configurator creates `reels.stack.config.v7`. A route session tells the agent what kind of pass to run on the same material.

| route | skill/system | output |
|-------|--------------|--------|
| `reel_edit` | `reel-edit` | private render spine: source audit, transcript, EDL, overlay lanes, review/final MP4, text-free base and QA |
| `block_timeline` | `reel-block-edit` | source blocks, timeline board state, overlay events, contact sheet and manual trim notes |
| `palmier` | `reel-palmier-board` | editable Palmier board, frame shells, native text clips and winner cutlane |
| `shaper` | `shaper-reels` | restrained public proof/process overlay with thin rails, labels and safe-zone QA |
| `cover_lab` | `inside-insanity` + `reel-edit` | clean no-text cover bases, independent text layer, contact sheet, style tokens and public-safe protocol delta |
| `article_research` | `stack-compare` | public research sessions, source credits, route matrix and article-ready summary |

### Same-Source Test

To compare routes, use one source map, transcript baseline and audio policy. Save outputs under `test_runs/<route>/`, then compare:

- first 3 seconds;
- safe-zone readability;
- cover readability;
- editability;
- render QA;
- publish readiness;
- learning note quality.

## Branch Packs

| pack | use when | key artifacts |
|------|----------|---------------|
| `portable-iphone-kit` | one creator has iPhone `.MOV` footage | `source_map.md`, `raw_sdr/*.mov`, transcript JSON, EDL, QA |
| `edl-block-editor` | the montage must become reusable state | `edit_decision_list.json`, `realself.timeline.json`, overlay manifest |
| `caption-lane` | subtitles and social-safe text are the main layer | word timings, ASS/SRT/WebVTT, PNG captions, safe-zone QA |
| `collab-spine` | live exchange between creators | turn map, `collabBeats[]`, preserved handoff notes |
| `creator-collab-challenge` | public growth challenge | sanitized metrics, challenge EDL, Trial results |
| `cover-visual-board` | cover set or visual-board update | clean no-text bases, text layer, contact sheet, style tokens, public delta |
| `public-dashboard` | open-source package | `STACK.md`, `CONTRIBUTING.md`, branch README, static dashboard |
| `article-demo` | article, talk or public explanation | schematic EDL, library list, community credits, citation paragraph |

## Dasha/Voronik Audit

The public [`Voronik1801/reel_pipline`](https://github.com/Voronik1801/reel_pipline) repo is strongest as a **portable iPhone production kit**:

- Apple `avconvert` for iPhone HLG/bt2020 → SDR bt709 color;
- `pipeline_check.sh` preflight before creative work;
- copy-fill-run templates for build and render;
- RMS dead-air detector;
- stutter detector from long words, pauses and near-repeats;
- replaceable subtitle, cover, finale, music and sticker slots.

Those mechanics inform the `portable-iphone-kit` pack. The broader OS adds EDL-as-SSOT, branch surface, overlay lanes, block-editor direction, QA gates and public/private packaging.

## Research Additions

The latest EXA pass adds these public mechanics:

- [`FeezRM/AI-ShortForm-Editor`](https://github.com/FeezRM/AI-ShortForm-Editor) - transcript, scene/silence/audio rhythm, EDL, confidence gate and vertical render;
- [`CarlAmine/AI_Editor`](https://github.com/CarlAmine/AI_Editor) - OCR/SceneDetect, brief-to-edit-plan JSON, overlay planner and render timeline;
- [`floomhq/opencut`](https://github.com/floomhq/opencut) - TypeScript timeline-as-code, Whisper word captions, validate/render CLI and Remotion route;
- [`neutral-Stage/remotion-captioneer`](https://github.com/neutral-Stage/remotion-captioneer) - word-level caption components, presets and subtitle exports;
- OpenTimelineIO, Remotion and Mediabunny - timeline interchange, JSON render state and browser media route;
- cover/thumbnail tools - clean no-text base, separate title layer, 5-10 variants, contact sheet and small-size QA.

## Technology Map

| layer | recommended tools |
|-------|-------------------|
| production core | `ffmpeg`, `ffprobe`, Python 3.10+, sequential render queue |
| iPhone ingest | Apple `avconvert` for HDR/HLG → SDR bt709 on macOS |
| transcript | Whisper or Scribe with word timings and pause map |
| analysis | PySceneDetect, librosa, EasyOCR/PaddleOCR where scene/proof detection matters |
| overlays | Pillow, numpy, ASS/SRT/WebVTT or PNG subtitle lanes, contact sheets |
| covers | clean base, independent text layer, contact sheet, readability QA |
| state | JSON EDL now, OpenTimelineIO adapter later |
| render adapters | ffmpeg now, Remotion or HyperFrames for programmatic visual branches |
| browser/editor surface | tracks/items board, WebCodecs/OpenCut direction |
| agent control | CLI, MCP tools and public-safe branch packs |

## Editing Protocols

| id | protocol | short output |
|----|----------|--------------|
| P0 | brief | goal, audience, source, mode, privacy boundary |
| P1 | source | source map, ffprobe facts, ingest decision |
| P2 | timing | transcript, word timings, pauses |
| P3 | story | source blocks and public-safe summaries |
| P4 | EDL | cuts, overlay tracks, render outputs |
| P5 | overlays | lanes, captions, safe zones |
| P6 | render | review, text-free base, final MP4 |
| P7 | QA | diagnostics, privacy pass, monitor note |

## Soft Communication Layer

| layer | purpose |
|-------|---------|
| hook | why someone should watch now |
| voice | how the reel speaks: tone, tempo, pause density, caption density |
| proof | what makes the claim believable on screen |
| turn | how the reel lands: reversal, conclusion, CTA and next branch |

## Builder Prompt

The live page generates a prompt from `source + goal + mode + route`. The prompt asks an agent to:

- read `STACK.md`, `README.md` and the selected branch README;
- read `PROTOCOLS.md` with the P0-P7 contract;
- run preflight;
- build `source_map.md`, transcript JSON, pause map, EDL and overlay tracks;
- build cover set when selected: clean bases, title layer, contact sheet and style tokens;
- render review/final MP4;
- run ffprobe, audio, first-3s, safe-zone and privacy QA;
- write a short learning note with reusable branch decisions and public-safe protocol delta.

Each route has a different process and handoff: render spine, timeline board, Palmier board, Shaper overlay, cover lab or article research.

## Public Boundary

Public files store **mechanics only**:

- no raw footage;
- no private transcript text;
- no real handles, names, local filenames or local paths;
- no provider keys;
- public metrics are aggregated.

## Publish Your Fork

```bash
gh repo create <you>/<name> --public --source=. --push
gh api -X POST repos/<you>/<name>/pages -f 'source[branch]=main' -f 'source[path]=/'
```

Built with Codex + Claude Code + EXA MCP research. Design language: AI Mindset Shaper.
