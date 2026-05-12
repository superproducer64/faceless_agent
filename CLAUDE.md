# Faceless YouTube Agent System

## What This Project Is
A fully orchestrated, multi-agent pipeline for producing faceless YouTube videos — narration-led, stock-footage or AI-imagery driven — from channel onboarding through to published video and monthly performance review.

## Agent Architecture
This project uses Claude Code subagents. Each agent is defined in `.claude/agents/`. The `/video-manager` agent is the entry point and orchestrator.

```
/video-manager          ← start here
├── /channel-onboarding   Layer 1: run once
├── /content-calendar     Layer 1: run monthly
├── /research-agent       Layer 2: per video
├── /script-writer        Layer 2: per video
├── /title-thumbnail      Layer 2: per video (parallel with script)
├── /voiceover-director   Layer 3: per video (after script)
├── /visual-brief         Layer 3: per video (after script)
├── /edit-brief           Layer 3: per video (after script)
├── /metadata-writer      Layer 4: per video
├── /clip-agent           Layer 4: per video (parallel with metadata)
├── /publisher            Layer 4: optional
└── /performance-review   Layer 5: run monthly
```

## File Conventions

### Slug format
All output files use a URL-safe slug as the filename:
`{topic-keyword}-{YYYY-MM}`
Example: `compound-interest-explained-2025-06`

### Context files (shared inputs, read by all agents)
- `context/channel-brief.md` — set by /channel-onboarding, read by everything
- `context/video-calendar.md` — set by /content-calendar, read by content agents
- `context/best-performers.md` — set by /performance-review, read by /content-calendar
- `context/research/{slug}.md` — set by /research-agent, read by script/title agents

### Output files (per-video deliverables)
- `outputs/scripts/{slug}.md`
- `outputs/briefs/{slug}.md`
- `outputs/voiceover/{slug}.md`
- `outputs/visual-briefs/{slug}.md`
- `outputs/edit-briefs/{slug}.md`
- `outputs/metadata/{slug}.md`
- `outputs/clips/{slug}.md`
- `outputs/publish-log/{slug}.md`

## Standard Run Order (single video)
1. `/channel-onboarding` (first run only)
2. `/content-calendar` (monthly)
3. `/research-agent` (per video)
4. `/script-writer` + `/title-thumbnail` (parallel, both depend on research)
5. `/voiceover-director` + `/visual-brief` + `/edit-brief` (parallel, depend on locked script)
6. `/metadata-writer` + `/clip-agent` (parallel, depend on locked script)
7. `/publisher` (optional, when assets are ready)
8. `/performance-review` (end of month)

## Rules for All Agents
- Always read `context/channel-brief.md` before generating any content
- Never invent data — if a required context file is missing, report the blocker and stop
- Write output files to the correct path using the slug from `context/video-calendar.md`
- Use second-person voice ("you") unless the channel brief specifies otherwise
- Keep all content within the niche and tone boundaries defined in the channel brief

---

# Book-to-Streaming Agent System

## What This Pipeline Is
A multi-agent pipeline for adapting classic and public domain literary works into streaming content — limited series, anthologies, or features. Takes a source book → produces a treatment, character bible, episode map, scene breakdowns, and pitch documents.

## Entry Point
Use `adaptation-director` to start any new adaptation project or run the full pipeline.

## Agent Architecture

```
/adaptation-director    ← start here (orchestrator)

LAYER 1 (parallel)
├── /book-analyst         Deep structural + thematic analysis of source
└── /platform-strategist  Format, platform fit, audience targeting

LAYER 2 (parallel, after Layer 1)
├── /character-architect  Character bible — rebuilt for screen
└── /world-builder        Visual world, tone, period, setting

LAYER 3 (after Layer 2)
├── /story-reconstructor  Episode map or three-act structure
└── /writers-room         Scene breakdowns + sample dialogue

LAYER 4 (parallel, after Layer 3)
├── /showrunner-brief     Full treatment document
└── /pitch-packager       Loglines, one-pager, platform pitches
```

## Slug Convention
`{title-slug}-{format}` — e.g. `count-of-monte-cristo-limited` or `dorian-gray-feature`

## Context Files (per project)
- `context/{slug}.md` — project status and producer notes
- `context/source-texts/` — drop source material here before running
- `context/research/{slug}-analysis.md` — book-analyst output
- `context/research/{slug}-platform.md` — platform-strategist output
- `context/research/{slug}-world.md` — world-builder output

## Output Files (per project)
- `outputs/character-bibles/{slug}-characters.md`
- `outputs/episode-breakdowns/{slug}-structure.md`
- `outputs/episode-breakdowns/{slug}-ep[N]-scenes.md`
- `outputs/treatments/{slug}-treatment.md`
- `outputs/pitch-docs/{slug}-pitch.md`
- `outputs/pilots/{slug}-pilot.md` (future)

## Quick Command Reference
| Say this | What happens |
|---|---|
| `"adapt [title] as a [format]"` | Intake + Layer 1 |
| `"full pipeline for [slug]"` | All layers sequentially |
| `"character bible for [slug]"` | character-architect only |
| `"episode map for [slug]"` | story-reconstructor only |
| `"treatment for [slug]"` | showrunner-brief only |
| `"pitch doc for [slug]"` | pitch-packager only |
| `"what's next for [slug]"` | Status check |

## Producer Decision Points
Every agent flags `⚡ PRODUCER DECISION:` moments. The pipeline pauses for your direction at each one.

## Rules for Book Adaptation Agents
- Never skip a layer — each agent's output feeds the next
- Flag `⚡ PRODUCER DECISION:` explicitly before proceeding past any creative choice
- Public domain only (US: pre-1928 publication, or author died 70+ years ago) unless producer confirms otherwise
- Write treatments in present tense, active voice, produced prose — no bullet points
