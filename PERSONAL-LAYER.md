# Personal Reels Layer

> Public-safe analysis of the personal layer behind AI Mindset `reel-edit`.

The public atlas revealed a missing distinction:

```text
visual references -> personal visual board -> reel-edit pipeline -> public branch pack
```

`reel-edit` should keep owning the render-grade spine: source audit, transcript,
word-safe cuts, EDL, overlay lanes, render outputs and QA.

The personal account needs an upstream board before that spine:

| layer | role |
|-------|------|
| `style-bible` | stores personal taste: references, light, texture, color, typography, recurring clusters |
| `visual-pipeline` | turns references into Design DNA, Design Code and briefs |
| `inside-insanity` | selects the account voice, visual mode, motif, caption grammar and survivor preset |
| `reel-edit` | renders and stores the edit state as EDL plus review/final outputs |
| `stack-compare` | packages only public-safe mechanics, source links and branch prompts |

## What Changed

Before this pass, the public dashboard explained the montage stack but treated
visual style as one more config field. For Alex's personal reels, style is a
separate upstream decision. The editor needs to know the account's current visual
state before choosing overlays.

The practical sequence becomes:

1. collect visual references from the day: photo, object, screenshot, raw clip,
   external reference, mood phrase;
2. map them into personal aesthetic DNA: light, texture, palette, typography,
   materiality and energy;
3. choose an `inside-insanity` visual mode: `shaper_color_return`,
   `kitchen_operating_system`, `dual_persona_no_overlap`,
   `collab_interruption_spine`, `cover_set` or a new candidate;
4. pass that board into `reel-edit` as metadata;
5. render branch variants from the same EDL;
6. update the visual board when a branch survives review.

## EDL Metadata Contract

Personal branches can carry this public-safe shape:

```json
{
  "visualPipeline": {
    "account": "_inside_insanity",
    "styleBoard": "inside-insanity",
    "visualMode": "shaper_color_return",
    "tone": "dry personal lower-case",
    "captionGrammar": "viewer-facing, sparse, no hashtags",
    "motifObject": "lamp | kitchen object | terminal-face | balcony",
    "survivorPreset": "color_return | kitchen_os | dual_persona",
    "publicSummary": "generic visual mechanics only"
  }
}
```

Private fields stay out of public branches: raw footage, transcript text, handles,
local paths, real filenames, private screenshots and provider keys.

## Skill Update Recommendation

Update `inside-insanity` with a visual-board contract:

- read Style Bible / visual DNA before routing to `reel-edit`;
- decide visual premise, tone, aesthetic mode and motif;
- pass `visualPipeline` metadata to the EDL;
- write survivor/failure notes back into the board.

Update `reel-edit` with a personal-board input contract:

- if `inside-insanity` is active, treat its style board as the visual source of truth;
- do not collapse personal taste into a generic `--style` flag;
- keep the rendered branch rebuildable: EDL, overlay tracks, text-free base, QA.

## Public Artifact Rule

The public page should show the mechanics:

- author/source map;
- technology map;
- personal visual-board layer;
- branch builder prompt.

It should not expose private day notes, raw media, transcripts or unpublished
account strategy.
