# Reels Pipeline OS

> **Visual atlas and stack spec for agentic Reels editing.**
> Live: https://apowall.github.io/reels-pipeline-os/

This repo is a public **visual atlas + branch builder** for short-form montage stacks. It explains the shared editing state first: source audit, transcript, source blocks, EDL, overlay lanes, render route, QA and public/private boundary. The builder is the practical layer: it turns that map into a branch pack, JSON config and agent prompt.

It is built from our real Codex/Claude Code reel sessions, the `reel-edit` skill, the `stack-compare` skill, and a fresh audit of the public `Voronik1801/reel_pipline` repo.

![cover](assets/og-cover.png)

## Start Here

| file | what |
|------|------|
| **[index.html](index.html)** | public atlas, author map, technology map and live stack builder |
| **[STACK.md](STACK.md)** | stack spec: packs, contracts, EDL schema, QA gates, source research, agent prompt |
| **[PERSONAL-LAYER.md](PERSONAL-LAYER.md)** | public-safe analysis of the personal visual-board layer before `reel-edit` |
| **[CONTRIBUTING.md](CONTRIBUTING.md)** | how to contribute a branch without leaking private data |
| **[branches/](branches/)** | public-safe branch recipes and sample EDLs |

## What Changed

The current version leads with the **public montage atlas**:

1. see how different editing approaches become one `editing state`;
2. inspect how Alex's `reel-edit 3.17` skill is configured now;
3. review the author/source map: Dasha/Voronik, X-Cut, OpenScript, HyperFrames, ReelStack, OpenReels, OpenCut;
4. inspect the personal layer: Style Bible → inside-insanity visual board → `reel-edit`;
5. inspect the technology map: ffmpeg, ffprobe, avconvert, Whisper/Scribe, Pillow, OTIO, Remotion, HyperFrames, WebCodecs, MCP;
6. choose `source`, `goal` and `mode` in the builder;
7. copy the generated JSON config and agent prompt.

## Branch Packs

| pack | use when | key artifacts |
|------|----------|---------------|
| `portable-iphone-kit` | one creator has iPhone `.MOV` footage | `source_map.md`, `raw_sdr/*.mov`, transcript JSON, EDL, QA |
| `edl-block-editor` | the montage must become reusable state | `edit_decision_list.json`, `realself.timeline.json`, overlay manifest |
| `inside-insanity` | Alex personal Instagram reels | EDL, survivor notes, contact sheet, Saved Messages preview |
| `collab-spine` | live exchange between creators | turn map, `collabBeats[]`, preserved handoff notes |
| `creator-collab-challenge` | public growth challenge | sanitized metrics, challenge EDL, Trial results |
| `public-dashboard` | open-source package | `STACK.md`, `CONTRIBUTING.md`, branch README, static dashboard |

## Dasha/Voronik Audit

The public [`Voronik1801/reel_pipline`](https://github.com/Voronik1801/reel_pipline) repo is strongest as a **portable iPhone production kit**:

- Apple `avconvert` for iPhone HLG/bt2020 → SDR bt709 color;
- `pipeline_check.sh` preflight before creative work;
- copy-fill-run templates for build and render;
- RMS dead-air detector;
- stutter detector from long words, pauses and near-repeats;
- replaceable subtitle, cover, finale, music and sticker slots.

We use those mechanics as the `portable-iphone-kit` pack. The broader OS adds EDL-as-SSOT, branch surface, overlay lanes, block-editor direction, Trial loop, skill memory and public/private packaging.

## Technology Map

| layer | recommended tools |
|-------|-------------------|
| production core | `ffmpeg`, `ffprobe`, Python 3.10+, sequential render queue |
| iPhone ingest | Apple `avconvert` for HDR/HLG → SDR bt709 on macOS |
| transcript | Whisper or Scribe with word timings and pause map |
| overlays | Pillow, numpy, ASS/PNG subtitle lanes, contact sheets |
| state | JSON EDL now, OpenTimelineIO adapter later |
| render adapters | ffmpeg now, Remotion or HyperFrames for programmatic visual branches |
| browser/editor surface | reel-block-edit/Palmier board, WebCodecs/OpenCut direction |
| agent control | CLI, MCP tools, skills and public-safe branch packs |

## Builder Prompt

The live page generates a prompt from `source + goal + mode`. The prompt asks an agent to:

- read `STACK.md`, `README.md` and the selected branch README;
- run preflight;
- build `source_map.md`, transcript JSON, pause map, EDL and overlay tracks;
- render review/final MP4;
- run ffprobe, audio, first-3s, safe-zone and privacy QA;
- write `skill_patch_notes.md` when a survivor pattern should update `reel-edit`.

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
