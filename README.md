# Content Mate

**Automated short-form content factory. Scrape viral ideas, generate videos, publish to 9 platforms — all on autopilot.**

Content Mate is an n8n workflow + Airtable system that turns trending X/Twitter posts into fully produced short-form videos with AI voiceovers, lip-synced avatars, captions, and music — then publishes them everywhere.

Built by [AI Andy](https://youtube.com/@AIAndyAutomation).

---

## What It Does

```
X/Twitter Viral Posts → Script → Voiceover → Avatar → Captions → Music → Video → 9 Platforms
```

| Step | What Happens | Powered By |
|------|-------------|------------|
| **1. Scrape** | Monitors X accounts, finds viral posts | TwitterAPI.io + Apify |
| **2. Ideate** | Deduplicates, scores, picks best content | Airtable + n8n |
| **3. Script** | AI writes a short-form video script + hook | OpenAI GPT-4 |
| **4. Voice** | Generates natural voiceover | ElevenLabs |
| **5. Avatar** | Creates lip-synced talking head video | Replicate |
| **6. Captions** | Adds animated captions (ASS subtitles) | ElevenLabs S2T + FFmpeg |
| **7. Music** | Overlays background music | Your music library |
| **8. Combine** | Merges avatar + captions + music into final video | FFmpeg |
| **9. Publish** | Posts to all 9 platforms simultaneously | Blotato |

### Platforms Supported
YouTube Shorts, TikTok, Instagram Reels, X/Twitter, Threads, LinkedIn, Facebook, Pinterest, Bluesky

---

## Architecture

```
┌─────────────────────────────────────────────────┐
│              AIRTABLE (Database)                 │
│                                                  │
│  X Handles → Ideas → Create → Published          │
│  Avatars    Music    Setup                       │
└──────────┬───────────────────────┬───────────────┘
           │                       │
     ┌─────▼───────────────────────▼─────┐
     │         n8n WORKFLOW              │
     │       (137 nodes)                  │
     │                                    │
     │  Triggers:                         │
     │  • Schedule (auto-scrape)          │
     │  • Schedule (auto-create)          │
     │  • Schedule (auto-publish)         │
     │  • Webhooks (manual triggers)      │
     └──────────┬─────────────────────────┘
                │
     ┌──────────▼──────────────────────────┐
     │          EXTERNAL APIs              │
     │                                      │
     │  TwitterAPI.io  •  OpenAI  •  Eleven │
     │  Replicate  •  Blotato  •  Google    │
     │  Drive  •  Telegram  •  FFmpeg       │
     └─────────────────────────────────────┘
```

### The 4 Triggers

| Trigger | What It Does | Default Schedule |
|---------|-------------|------------------|
| **Scrape** | Pulls new viral posts from monitored X accounts | Every few hours |
| **Create** | Picks top idea, writes script, generates video | Daily |
| **Publish** | Posts finished videos to all platforms | Scheduled times |
| **Webhooks** | Manual triggers for avatar, music, publish | On demand |

---

## Quick Start

### Prerequisites
- [n8n](https://n8n.io) instance (self-hosted with Docker recommended)
- [Airtable](https://airtable.com) account
- API keys for: OpenAI, ElevenLabs, TwitterAPI.io, Replicate, Blotato
- FFmpeg installed on your n8n server
- Google Drive (for file storage)

### Setup (8 Steps)

The full setup guide walks you through everything: **[Content Mate v2.0 — Complete Setup Guide](https://docs.google.com/document/d/1y3t_ZP4OdOrKWGF20ZXzkJkr5G_3WC4sAhxBuFAXt7A/edit)**

Quick overview:
1. **Duplicate the Airtable template** (link in the setup guide)
2. **Download the n8n workflow** from `workflow/content-mate-v2.json`
3. **Get self-hosted n8n** (Hostinger KVM 2 recommended)
4. **Install n8n with FFmpeg** (Docker setup with execute command support)
5. **Create video folder** and set permissions on your server
6. **Fill in the Setup table** with all your API keys
7. **Import the workflow** into n8n and configure triggers
8. **Set up avatars, voices, and music** — then activate

### Monthly Cost
| Tier | Cost | What You Get |
|------|------|-------------|
| **Minimal** | $35-45/mo | Basic usage, fewer videos |
| **Daily use** | $55-75/mo | 5 videos/day, all platforms |
| **Scaled** | $100+/mo | High volume, multiple accounts |

See [docs/SETUP.md](docs/SETUP.md) for a quick-start reference, or follow the [full setup guide](https://docs.google.com/document/d/1y3t_ZP4OdOrKWGF20ZXzkJkr5G_3WC4sAhxBuFAXt7A/edit) for step-by-step instructions with screenshots.

---

## Airtable Structure

| Table | Purpose |
|-------|---------|
| **X** | X/Twitter handles to monitor for viral content |
| **Ideas** | Scraped posts, scored and deduplicated |
| **Create** | Main production table — scripts, videos, captions |
| **Avatars** | AI lip-sync avatar videos (4-second loops) |
| **Music** | Background music library |
| **Setup** | All configuration: API keys, account IDs, table IDs |
| **Longs** | Long-form content (YouTube videos) |

The Setup table is the brain — the n8n workflow reads all config from there. Change your API keys or account IDs in one place.

---

## How Videos Are Made

### 1. Scraping
The workflow monitors X accounts you specify, pulls their recent posts via TwitterAPI.io, deduplicates against existing ideas, and stores new ones in the Ideas table.

### 2. Script Generation
OpenAI GPT-4 takes the top viral idea and writes a short-form video script optimized for vertical video. It also generates a hook for the caption.

### 3. Voice Generation
ElevenLabs converts the script to a natural-sounding voiceover. The audio is saved to Google Drive and linked in Airtable.

### 4. Avatar Creation
A random avatar is selected from your library. The voiceover audio is lip-synced onto the avatar video using Replicate's model. Result: a talking-head video that matches your voice.

### 5. Caption & Music
- ElevenLabs Speech-to-Text generates word-level timestamps
- n8n creates an ASS subtitle file with animated captions
- FFmpeg combines: avatar video + captions + background music = final video

### 6. Publishing
Blotato publishes the finished video to all 9 platforms simultaneously with the AI-generated hook as the caption.

---

## Community

**Want pre-built Airtable templates, avatar packs, and video walkthroughs?**

Join the [Skool community](https://www.skool.com/ai-mate) for:
- One-click Airtable base clone
- Avatar creation tutorial
- Music library starter pack
- Workflow customization help
- Weekly live Q&A

---

## File Structure

```
content-mate/
├── README.md                           # This file
├── LICENSE                             # MIT License
├── workflow/
│   └── content-mate-v2.json           # n8n workflow (import this)
└── docs/
    ├── SETUP.md                        # Full setup guide
    ├── AIRTABLE_SCHEMA.md              # Table structure + field details
    └── ARCHITECTURE.md                 # How the workflow works
```

---

## Contributing

PRs welcome! Ideas for improvements:
- Support for more video styles
- Additional platform integrations
- Better caption animations
- Scheduling optimization

---

## See Also

**[Clawdia](https://github.com/AndyHafell/clawdia)** — The long-form YouTube automation system. Uses Claude Code + Python to automate show docs, AI thumbnails, and video publishing. Content Mate handles short-form; Clawdia handles long-form.

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

Built with n8n, Airtable, OpenAI, ElevenLabs, Replicate, Blotato, and FFmpeg.
