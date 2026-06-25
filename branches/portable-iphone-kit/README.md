# portable iphone kit

Public-safe branch for a transferable iPhone Reels workflow inspired by the public
`Voronik1801/reel_pipline` repository.

## What It Captures

- macOS/iPhone color discipline: HDR/HLG footage is converted to SDR through Apple
  `avconvert`, then exported with bt709 tags;
- a first-run environment check before any edit starts;
- copy-and-fill templates for a new shoot, so the pipeline can be handed to a
  new creator without moving private footage;
- stutter and dead-air review as explicit QA artifacts;
- replaceable cover/finale/music slots, so account identity lives in config
  and private media stays outside the public branch.

## What We Borrow

- **preflight before creativity**: check ffmpeg, ffprobe, whisper, avconvert,
  fonts, and working folders first;
- **template discipline**: new shoot = copy templates, fill sources, cut ranges,
  cover config, and finale config;
- **audio truth beats transcript truth**: word timings are useful, but long words,
  RMS drops, and ZCR spikes must be reviewed by ear;
- **iPhone color is a pipeline layer**: HDR conversion is not an aesthetic grade;
- **handoff-friendly docs**: the README is written so a human can ask an agent to
  run the whole chain.

## What Stays Different In Our Stack

The portable kit is a strong **single-account production skeleton**. AI Mindset
Reels Pipeline OS keeps it as a branch, then adds EDL-as-SSOT, branch surfaces,
overlay lanes, block editor previews, Trial Reels strategy, and skill memory.

## Contribution Rule

Do not copy creator footage, motif assets, real captions, Telegram/Instagram
handles, private shoot names, local paths, or raw transcripts into this branch.
Contribute the reusable mechanics only.
