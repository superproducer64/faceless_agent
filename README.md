# Faceless YouTube Agent System

A fully orchestrated multi-agent pipeline for producing faceless YouTube videos — built for Claude Code.

## Quick Start

```bash
# 1. Install Claude Code (if not already installed)
npm install -g @anthropic-ai/claude-code

# 2. Navigate to this project
cd faceless-youtube-agent

# 3. Launch Claude Code
claude

# 4. Start the orchestrator
> "set up the channel"
```

---

## Project Structure

```
faceless-youtube-agent/
├── CLAUDE.md                          ← Project memory (auto-read by Claude Code)
├── README.md                          ← This file
│
├── .claude/
│   └── agents/                        ← All subagents live here
│       ├── video-manager.md           ← Orchestrator — start here
│       ├── channel-onboarding.md      ← Layer 1
│       ├── content-calendar.md        ← Layer 1
│       ├── research-agent.md          ← Layer 2
│       ├── script-writer.md           ← Layer 2
│       ├── title-thumbnail.md         ← Layer 2
│       ├── voiceover-director.md      ← Layer 3
│       ├── visual-brief.md            ← Layer 3
│       ├── edit-brief.md              ← Layer 3
│       ├── metadata-writer.md         ← Layer 4
│       ├── clip-agent.md              ← Layer 4
│       ├── publisher.md               ← Layer 4 (optional)
│       └── performance-review.md      ← Layer 5
│
├── context/                           ← Shared inputs (read by all agents)
│   ├── channel-brief.md               ← Set by /channel-onboarding
│   ├── video-calendar.md              ← Set by /content-calendar
│   ├── best-performers.md             ← Set by /performance-review
│   └── research/
│       └── {slug}.md                  ← Set by /research-agent
│
└── outputs/                           ← Per-video deliverables
    ├── scripts/       {slug}.md
    ├── briefs/        {slug}.md
    ├── voiceover/     {slug}.md
    ├── visual-briefs/ {slug}.md
    ├── edit-briefs/   {slug}.md
    ├── metadata/      {slug}.md
    ├── clips/         {slug}.md
    └── publish-log/   {slug}.md
```

---

## How to Use

### First time — channel setup
```
> "set up the channel"
```
This runs `/channel-onboarding` and creates `context/channel-brief.md`.

### Monthly — plan content
```
> "plan this month's content"
```
Runs `/content-calendar` and creates `context/video-calendar.md`.

### Per video — full pipeline
```
> "full pipeline for compound-interest-explained-2025-06"
```
Runs all layers in sequence. Or run them step by step:

```
> "research compound-interest-explained-2025-06"
> "write script for compound-interest-explained-2025-06"
> "production files for compound-interest-explained-2025-06"
> "metadata and clips for compound-interest-explained-2025-06"
```

### Status check
```
> "what's next for compound-interest-explained-2025-06"
```

### End of month
```
> "performance review"
```

---

## Slug Convention

All files use a URL-safe slug:
```
{topic-keyword}-{YYYY-MM}
```
Examples:
- `compound-interest-explained-2025-06`
- `best-index-funds-uk-2025-07`
- `how-to-start-investing-2025-07`

The slug is set in `context/video-calendar.md` and used consistently across all agents.

---

## Parallelisable Agent Groups

Claude Code can run these agent groups concurrently:

| Group | Agents | Shared dependency |
|-------|--------|-------------------|
| Pre-production | script-writer + title-thumbnail | research complete |
| Production | voiceover + visual-brief + edit-brief | script locked |
| Distribution | metadata-writer + clip-agent | script locked |

To trigger parallel runs, tell video-manager explicitly:
```
> "run script and title brief in parallel for {slug}"
```

---

## Tool Integrations

| Agent | Recommended tool |
|-------|-----------------|
| voiceover-director | ElevenLabs, PlayHT, Murf |
| visual-brief | Midjourney, Flux, Runway, Pexels, Artgrid |
| edit-brief | CapCut, Descript, DaVinci Resolve |
| clip-agent | Opus Clip, CapCut |
| publisher | TubeBuddy, Publer, Later |
| research-agent | VidIQ, TubeBuddy, Ahrefs, Google Trends |
| performance-review | YouTube Studio Analytics |
