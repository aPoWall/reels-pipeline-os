# reel stack compare

Shaper-style single-file dashboard comparing two video-editing stacks:

- **their stack** — a deterministic long-form YouTube cut-chain (ElevenLabs Scribe → FFmpeg → монтажный план as single source of truth → Pillow/ProRes overlays → videotoolbox 1080p → YouTube API)
- **our stack** — a short-form Instagram Reels system (Whisper → visual block editor + `realself.timeline` manifest → mode/style library → audio-first lip-sync → Trial Reels)

Five tabs:

1. **сравнение** — the two pipelines side by side + layer-by-layer verdict table
2. **короткий формат** — their pipeline ported to short-form vs our short-form answer + 2026 short-form KPIs
3. **заимствуем** — six borrows ranked by leverage + our moat
4. **развитие** — target merged architecture + three roadmap horizons
5. **технологии** — EXA-researched technology matrix (Remotion, OpenTimelineIO, Hyperframes, X-Cut, Diffusion Studio + Mediabunny, WebCodecs, ElevenLabs Scribe, videotoolbox, RIFE, Revideo/Motion Canvas …)

Keyboard: `1`–`5` switch tabs · `d` toggle dark/light.

Single static file (`index.html`), no build step. Researched via EXA MCP, 2026-06-15.
