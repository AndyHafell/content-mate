# Content Mate — Setup Guide

## Prerequisites

| Service | Required | Free Tier? | What For |
|---------|----------|------------|----------|
| [n8n](https://n8n.io) | Yes | Yes (self-hosted) | Workflow engine |
| [Airtable](https://airtable.com) | Yes | Yes | Database |
| [OpenAI](https://platform.openai.com) | Yes | No (~$5/mo) | Script writing |
| [ElevenLabs](https://elevenlabs.io) | Yes | Yes (limited) | Voice generation |
| [Replicate](https://replicate.com) | Yes | No (pay-per-use) | Avatar lip-sync |
| [TwitterAPI.io](https://twitterapi.io) | Yes | No (~$10/mo) | X/Twitter scraping |
| [Blotato](https://blotato.com) | Yes | No (~$20/mo) | Multi-platform publishing |
| [Google Drive](https://drive.google.com) | Yes | Yes | File storage |
| [Telegram](https://telegram.org) | Optional | Yes | Notifications |
| FFmpeg | Yes | Yes | Video processing (install on n8n server) |

---

## Step 1: Set Up Airtable

1. Create a new Airtable base
2. Create all 7 tables following the schema in [AIRTABLE_SCHEMA.md](AIRTABLE_SCHEMA.md)
3. Fill in the **Setup** table with all your API keys and configuration
4. Get your base ID and all table IDs (see schema doc for how)

---

## Step 2: Get API Keys

### OpenAI
1. Go to [platform.openai.com](https://platform.openai.com)
2. Create an API key
3. Add to Setup table → `OpenAI API Key`

### ElevenLabs
1. Go to [elevenlabs.io](https://elevenlabs.io)
2. Create an API key in Settings
3. Add to Setup table → `ElevenLabs API Key`
4. Create or choose a voice → copy the Voice ID → add to your Avatar records

### Replicate
1. Go to [replicate.com](https://replicate.com)
2. Get your API token from account settings
3. Add to Setup table → `Replicate API Token`

### TwitterAPI.io
1. Sign up at [twitterapi.io](https://twitterapi.io)
2. Get your API key
3. Add to Setup table → `TwitterAPI Key`

### Blotato
1. Sign up at [blotato.com](https://blotato.com)
2. Connect all your social media accounts
3. Get your API key from settings
4. Note the account ID for each connected platform
5. Add all to Setup table

### Telegram (Optional)
1. Create a bot via [@BotFather](https://t.me/BotFather) on Telegram
2. Get the bot token → Setup table → `Telegram Access Token`
3. Get your chat ID → Setup table → `Telegram Chat ID`

---

## Step 3: Set Up n8n

### Install n8n
If self-hosting:
```bash
# Docker
docker run -d --name n8n -p 5678:5678 n8nio/n8n

# Or npm
npm install -g n8n
n8n start
```

### Install FFmpeg on your n8n server
```bash
# Ubuntu/Debian
sudo apt install ffmpeg

# macOS
brew install ffmpeg
```

### Import the Workflow
1. Open n8n (usually at `http://localhost:5678`)
2. Go to **Workflows** → **Import from file**
3. Select `workflow/content-mate-v2.json`
4. You'll see 137 nodes — don't panic, it's organized with sticky notes

### Connect Credentials
After importing, you need to set up credentials in n8n for:
1. **Airtable** — Personal Access Token
2. **Google Drive** — OAuth2 connection
3. **OpenAI** — API key
4. **Telegram** — Bot token (optional)
5. **Replicate** — API token (via HTTP Header Auth)
6. **Blotato** — API key

For each node that shows a credential error, click it and select/create the correct credential.

---

## Step 4: Prepare Content

### Add X Handles
In the **X** table, add the Twitter/X handles you want to monitor:
- Add handles of creators in your niche
- The scraper will pull their viral posts as content ideas

### Create Avatars
1. Record yourself in a 4-second vertical video loop
2. Upload to the **Avatars** table
3. Click "To Drive" to upload to Google Drive
4. Add your ElevenLabs Voice ID

### Add Music
1. Find royalty-free background music
2. Upload MP3s to the **Music** table
3. Click "to Drive" to upload to Google Drive

---

## Step 5: Activate

1. In n8n, toggle the workflow to **Active**
2. The scheduled triggers will start running automatically
3. Monitor progress via the Airtable Create table and Telegram notifications

---

## How It Runs

### Automatic Flow
1. **Scrape trigger** fires → pulls new posts from X → stores in Ideas
2. **Create trigger** fires → picks top idea → writes script → generates voice → creates avatar video → adds captions + music → saves final video
3. **Publish trigger** fires → posts finished videos to all platforms

### Manual Triggers
Use the webhook URLs to manually trigger specific steps:
- **Create webhook**: Force-create a video from the top idea
- **Avatar webhook**: Re-generate avatar for a specific record
- **Music webhook**: Re-add music to a specific record
- **Publish webhook**: Force-publish a specific video

---

## Troubleshooting

### Videos not generating
- Check FFmpeg is installed: `ffmpeg -version`
- Check the n8n server has write access to the `n8n Folder` path
- Check ElevenLabs and Replicate API keys are valid

### Captions not appearing
- ElevenLabs Speech-to-Text needs a valid API key
- The ASS subtitle generator expects specific audio format — check logs

### Publishing fails
- Verify Blotato account IDs are correct
- Check Blotato API key has access to all connected platforms
- Some platforms have upload size limits

### Scraping returns no results
- Verify TwitterAPI.io key is active
- Check X handles are correct (no @ prefix)
- Rate limits may apply — check TwitterAPI.io dashboard
