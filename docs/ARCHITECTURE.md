# Content Mate — Architecture

## System Overview

Content Mate is a **137-node n8n workflow** backed by an **Airtable database**. It automates the entire short-form content pipeline from idea sourcing to multi-platform publishing.

## The 4 Pipelines

### Pipeline 1: Scrape
```
Schedule Trigger → TwitterAPI.io → Deduplicate → Store in Ideas table
```
- Runs on a schedule (configurable)
- Pulls recent posts from monitored X accounts
- Checks for duplicates against existing Ideas
- Stores new ideas with engagement metrics

### Pipeline 2: Create
```
Schedule/Webhook → Pick Top Idea → OpenAI (Script) → ElevenLabs (Voice)
                → Replicate (Avatar) → FFmpeg (Captions + Music) → Final Video
```
1. Selects the top unprocessed idea from the Ideas table
2. GPT-4 writes a short-form video script optimized for vertical
3. ElevenLabs generates natural voiceover audio
4. Random avatar selected, lip-synced with voiceover via Replicate
5. Speech-to-text generates word timestamps for captions
6. FFmpeg creates ASS subtitle file and burns captions onto video
7. Random background music selected and mixed in
8. Final video uploaded to Google Drive and linked in Airtable

### Pipeline 3: Publish (Short-Form)
```
Schedule/Webhook → Get Ready Videos → Blotato → 9 Platforms
```
- Picks videos with "Ready" status
- Publishes simultaneously to: YouTube Shorts, TikTok, Instagram Reels, X, Threads, LinkedIn, Facebook, Pinterest, Bluesky
- Updates status to "Published"
- Sends Telegram notification

### Pipeline 4: Publish (Long-Form)
```
Webhook → Get Long-Form Record → Blotato → Selected Platforms
```
- Triggered manually via webhook
- Posts long-form content to selected platforms
- Separate from the automated short-form flow

## Key Design Decisions

### Config in Airtable (Setup Table)
All API keys, account IDs, and table references are stored in a single Airtable Setup record. The workflow reads this at the start of every run. Benefits:
- Change config without editing the workflow
- Non-technical users can update settings
- Easy to clone for multiple accounts

### Avatar System
Instead of generating AI videos from scratch (slow, expensive), Content Mate uses pre-recorded 4-second avatar loops that get lip-synced. Benefits:
- Consistent look across all videos
- Fast processing (lip-sync vs full generation)
- Multiple avatar options for variety

### Caption Pipeline
Captions are generated in 3 steps:
1. ElevenLabs Speech-to-Text → word-level timestamps
2. n8n Code node → ASS subtitle file with animation
3. FFmpeg → burns subtitles onto video

This gives animated, styled captions without needing a video editor.

## Node Categories

| Category | Count | Purpose |
|----------|-------|---------|
| Airtable | ~25 | Read/write database records |
| HTTP Request | ~15 | API calls (Twitter, ElevenLabs, Replicate) |
| File I/O | ~15 | Read/write temp files on server |
| Execute Command | ~8 | FFmpeg video processing |
| OpenAI | ~4 | Script and hook generation |
| Blotato | ~18 | Multi-platform publishing |
| Code | ~5 | Custom logic (dedup, captions, selection) |
| Triggers | ~5 | Schedules and webhooks |
| Other | ~42 | Switches, merges, waits, sticky notes |

## Data Flow

```
X/Twitter
    ↓ (TwitterAPI.io)
[Ideas Table]
    ↓ (Top pick)
[Create Table] ← Script (OpenAI)
    ↓
Voice (ElevenLabs) → Avatar (Replicate) → Captions (FFmpeg)
    ↓
[Google Drive] → Final Video
    ↓
[Blotato] → 9 Platforms
    ↓
[Telegram] → Notification
```
