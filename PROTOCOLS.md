# Reels Editing Protocols

Public-safe operating protocol for Reels Pipeline OS.

```text
brief -> source -> timing -> story -> EDL -> overlays -> cover -> render -> QA -> monitor package
```

## Protocol Stack

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

## Soft Communication Layer

| layer | question | editing signal |
|-------|----------|----------------|
| hook | why watch | conflict, question, useful promise or visible proof in the first seconds |
| voice | how it speaks | tone, tempo, pauses, caption density, persona lane and visible process level |
| proof | why believe | screen, object, timeline, before/after, quote-safe source or action inside the frame |
| turn | why finish | reversal, conclusion, CTA, next branch and learning note |

## Community Borrow Map

| source | what to borrow | where it lands |
|--------|----------------|----------------|
| [Voronik1801/reel_pipline](https://github.com/Voronik1801/reel_pipline) | portable iPhone kit, `avconvert`, preflight, templates, stutter/dead-air checks | P1 source, P2 timing, P7 QA |
| [FeezRM/AI-ShortForm-Editor](https://github.com/FeezRM/AI-ShortForm-Editor) | transcript, scene/silence/audio rhythm, EDL, confidence gate and vertical render | P2 timing, P4 EDL, P7 QA |
| [CarlAmine/AI_Editor](https://github.com/CarlAmine/AI_Editor) | OCR/SceneDetect, brief-to-edit-plan JSON and overlay planner | P1 source, P3 story, P5 overlay |
| [MeiGen-AI/X-Cut](https://github.com/MeiGen-AI/X-Cut) | chat intent, dynamic skill selection, asset analysis, real-time Remotion timeline | P0 brief, P3 story, future editor surface |
| [birchrust/openscript](https://github.com/birchrust/openscript) | MCP tools, EDL v2, multi-track timeline, verification layer | P4 EDL, P7 QA, agent control |
| [floomhq/opencut](https://github.com/floomhq/opencut) | TypeScript timeline-as-code, Whisper word captions, validate/render CLI | P4 EDL, P5 caption lane, P6 render |
| [neutral-Stage/remotion-captioneer](https://github.com/neutral-Stage/remotion-captioneer) | word-level caption components, presets and subtitle exports | P2 timing, P5 caption lane |
| [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) | HTML/CSS/media render loop, CLI lint/preview/render, agent skills | P5 overlays, P6 render |
| [jurczykpawel/reelstack](https://github.com/jurczykpawel/reelstack) | CLI/API production stages, provider registry, prompt templates, self-hosting | P0 brief, P6 render, package automation |
| [tsensei/OpenReels](https://github.com/tsensei/OpenReels) | topic-to-short flow: research, script, voiceover, visuals, music, captions, assembly | article-demo pack, idea-to-reel branch |
| [OpenTimelineIO](https://github.com/AcademySoftwareFoundation/OpenTimelineIO) | editorial timeline interchange format | EDL export and NLE bridge |
| [Mediabunny](https://mediabunny.dev/guide/output-formats) / [WebCodecs](https://www.w3.org/TR/webcodecs/) | browser media containers and decode/encode primitives | future browser preview/export |
| [thumbnail-guru](https://github.com/sebastianhardy/thumbnail-guru), [Adobe Express](https://www.adobe.com/express/create/ai/thumbnail), [Canva](https://www.canva.com/ai-thumbnail-maker/) | clean base, separate text layer, variant contact sheet and small-size QA | cover-visual-board |

## Public Boundary

Public protocol files may include:

- generic timing ranges;
- source labels like `source_take_a`;
- `cuts[]`, `overlay_tracks[]`, render routes and QA status;
- cover lane mechanics, style tokens and generated or sanitized contact sheets;
- branch intent and learning notes;
- source links and community attribution.

Public protocol files exclude:

- raw footage;
- private transcript text;
- faces, handles, names and private screenshots;
- contact sheets made from private frames or faces;
- local paths and source filenames;
- provider keys or internal agent/session IDs.

## Configurator Output

The configurator should return:

- selected `source`, `goal`, `mode`;
- selected `route`;
- recommended branch pack;
- cover lane policy when selected;
- full protocol stack P0-P7;
- artifacts to create;
- public-safe agent prompt;
- learning note instruction.

## Test Session Routes

| route | owner | expected output |
|-------|-------|-----------------|
| `reel_edit` | `reel-edit` | render-grade spine, review/final MP4, text-free base, QA and learning note |
| `block_timeline` | `reel-block-edit` | source blocks, cut map, overlay events, board state and manual review notes |
| `palmier` | `reel-palmier-board` | editable Palmier board, frame shells, native text clips and winner cutlane |
| `shaper` | `shaper-reels` | monochrome proof/process overlay, safe-zone contact sheet and style reuse note |
| `cover_lab` | `inside-insanity` + `reel-edit` | clean cover bases, text layer, contact sheet, style tokens and public-safe protocol delta |
| `article_research` | `stack-compare` | public research sessions, source credits, route matrix and article-ready summary |

## Same Source Comparison Protocol

Use one source package for all routes:

- `source_map.md`;
- transcript JSON and pause map;
- baseline audio policy;
- privacy boundary;
- public-safe source labels.

Run each route into `test_runs/<route>/`. Compare:

- first 3 seconds;
- safe-zone readability;
- cover readability;
- editability;
- render or board QA;
- public boundary;
- publish readiness;
- learning note quality.

## Cover / Visual Board Protocol

When a reel produces a useful visual survivor, convert it in two steps:

1. Private skill note: store exact local observations, raw references and project context inside the private video project or skill memory.
2. Public protocol delta: publish only generic mechanics such as card grammar, caption density, cover base/text split, audio gate and QA rule.

Public cover artifacts use generated or sanitized images only. Never publish raw frames, faces, private locations, local filenames or transcript text.
