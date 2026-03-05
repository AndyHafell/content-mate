# Content Mate — Airtable Schema

Create a new Airtable base with these 7 tables.

---

## Table 1: X (Handles to Monitor)

| Field | Type | Notes |
|-------|------|-------|
| Handle | Single line text | X/Twitter username (without @) |

Add the X accounts you want to monitor for viral content ideas.

---

## Table 2: Ideas (Scraped Content)

| Field | Type | Notes |
|-------|------|-------|
| Name | Single line text | Post author |
| Link | Long text | URL to the original post |
| Text | Rich text | Full post content |
| Views | Number | View count |
| Video (sec) | Number | Video duration if applicable |
| Ratiox | Formula | Engagement ratio calculation |
| Action | Single select | Options: `Create`, `Skip`, `Review` |
| Date | Date/time | When the post was published |
| Status | Single select | Options: `New`, `Processing`, `Created`, `Skipped` |
| Ratio | Long text | Engagement details |
| IsRetweet | Long text | Whether it's a retweet |
| Retweets | Number | Retweet count |
| Retweet Text | Long text | Original tweet text if retweet |
| Retweet Views | Number | Original tweet views |
| Button | Button | Trigger action |

---

## Table 3: Create (Main Production)

| Field | Type | Notes |
|-------|------|-------|
| Name | Single line text | Video title/topic |
| Status | Single select | `Scripting`, `Voiceover`, `Avatar`, `Captions`, `Music`, `Ready`, `Published`, `Error` |
| Caption Video | Attachment | Final video with captions |
| YT Short Script | Long text | AI-generated script |
| Views | Number | Original post views |
| X Video Link | URL | Source video |
| Avatars | Link to Avatars | Which avatar was used |
| Avatar Link | Lookup | Avatar video URL |
| Voice ID Link | Lookup | ElevenLabs voice ID |
| Date Created | Date/time | When created |
| TopCrop | Lookup | Avatar crop setting |
| X Video (sec) | Number | Source video duration |
| Publish Time | Date/time | Scheduled publish time |
| Music | Link to Music | Which music track |
| Music Link | Lookup | Music file URL |
| Google Drive Link | URL | Final video in Drive |
| Hook | Long text | AI-generated caption/hook |
| Script | Long text | Full video script |

---

## Table 4: Avatars

| Field | Type | Notes |
|-------|------|-------|
| Avatar Name | Single line text | Name/description |
| 4s Vertical Vid | Attachment | 4-second vertical loop of the avatar |
| To Drive | Button | Upload to Google Drive |
| Avatar link | URL | Google Drive link |
| Voice ID | Single line text | ElevenLabs voice ID for this avatar |
| TopCrop | Number | Crop position (pixels from top) |

**How to create avatars**: Record yourself (or use an AI avatar) in a 4-second vertical video loop. Upload it here. The workflow will lip-sync your voiceover onto this video.

---

## Table 5: Music

| Field | Type | Notes |
|-------|------|-------|
| Name | Single line text | Track name |
| Music mp3 | Attachment | MP3 file |
| to Drive | Button | Upload to Google Drive |
| Music Link | URL | Google Drive link |
| Notes | Single line text | Genre, mood, etc. |

Upload background music tracks here. The workflow randomly selects one for each video.

---

## Table 6: Setup (Configuration)

This is the brain — the n8n workflow reads ALL config from this table.

| Field | Type | What to Put |
|-------|------|------------|
| Config Name | Long text | Your config name (e.g., "My Setup") |
| Active | Long text | `ON` or `OFF` |
| **Airtable** | | |
| Airtable Base ID | Long text | Your base ID (starts with `app...`) |
| Airtable Personal Token | Long text | Your Airtable PAT |
| Table Music ID | Long text | Music table ID (starts with `tbl...`) |
| Table Create ID | Long text | Create table ID |
| Table X ID | Long text | X table ID |
| Table Avatars ID | Long text | Avatars table ID |
| Table Ideas ID | Long text | Ideas table ID |
| **AI & Voice** | | |
| OpenAI API Key | Long text | Your OpenAI API key |
| ElevenLabs API Key | Long text | Your ElevenLabs API key |
| **Video** | | |
| Replicate API Token | Long text | For avatar lip-sync |
| **Scraping** | | |
| TwitterAPI Key | Long text | From TwitterAPI.io |
| **Publishing (Blotato)** | | |
| Blotato API Key | Long text | Your Blotato API key |
| YouTube Account ID | Long text | Blotato YouTube account ID |
| TikTok Account ID | Long text | Blotato TikTok account ID |
| Instagram Account ID | Long text | Blotato Instagram account ID |
| X Account ID | Long text | Blotato X account ID |
| Threads Account ID | Long text | Blotato Threads account ID |
| LinkedIn Account ID | Long text | Blotato LinkedIn account ID |
| Facebook Account ID | Long text | Blotato Facebook account ID |
| Pinterest Account ID | Long text | Blotato Pinterest account ID |
| Bluesky Account ID | Long text | Blotato Bluesky account ID |
| **Other** | | |
| Google Drive Folder ID | Long text | Folder for video storage |
| Telegram Access Token | Long text | Bot token for notifications |
| Telegram Chat ID | Long text | Chat ID for notifications |
| Timezone | Long text | e.g., `America/New_York` |
| n8n Folder | Long text | Server path for temp files (e.g., `/videos/`) |

---

## Table 7: Longs (Long-Form Content)

| Field | Type | Notes |
|-------|------|-------|
| Name | Single line text | Video title |
| Description | Long text | Video description |
| Links | URL | Video URL |
| Status | Single select | `Draft`, `Published` |
| Button | Button | Trigger action |

---

## Getting Your IDs

### Base ID
Open your base in Airtable → look at the URL: `https://airtable.com/appXXXXXXX/...`

### Table IDs
Click on a table → URL shows: `https://airtable.com/appXXX/tblYYYYYYY/...`

### Blotato Account IDs
After connecting platforms in Blotato, each platform gets a numeric account ID. Find these in your Blotato dashboard.
