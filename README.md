# 🎵 Crosstalk · 串聯

A music exchange system for you and your Claude. Share songs, tag listening contexts, leave comments, and build a shared musical memory together.

![Crosstalk](https://img.shields.io/badge/crosstalk-music_exchange-f4845f?style=flat-square) ![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)

## What is Crosstalk?

Crosstalk is a personal tool where you and Claude take turns sharing songs around themes and moods. Each exchange is a pair — you pick a song, Claude picks one back. You tag the listening context (weather, place, time, mood), write notes, and leave comments on each other's picks.

**How it works:**
- You use the web interface to submit songs and browse your exchange history
- Claude uses Supabase MCP to read your submissions and write back responses
- Both of you share the same database — no copy-pasting, no manual sync

## Features

- 🎧 **Dual-column card layout** — your songs on the right, Claude's on the left
- 🏷️ **Context tags** — weather, place, time, mood, and custom tags
- 💬 **Comment board** — leave messages on each exchange
- 🔍 **Album art search** — auto-fetch covers and links via iTunes
- ⚙️ **Customizable** — icons, display names, headphone style (pixel art!)
- 📱 **Responsive** — works on desktop and mobile
- 🎨 **Pixel art aesthetic** — low saturation, orange-cream palette, dotted backgrounds

## Setup (5 minutes)

### 1. Create a Supabase Project

- Go to [supabase.com](https://supabase.com) and create a free account
- Create a new project, note your **Project ID** and **Project URL**

### 2. Set Up the Database

- Go to your project's **SQL Editor**
- Copy and paste the contents of [`supabase/setup.sql`](supabase/setup.sql)
- Click **Run**

### 3. Deploy the Edge Function

You can deploy via Supabase CLI or through Claude with MCP:

**Via Supabase CLI:**
```bash
supabase functions deploy crosstalk-api --project-ref YOUR_PROJECT_REF
```

**Via Claude (if you have Supabase MCP connected):**
Ask Claude to deploy the Edge Function using the code in [`supabase/edge-function.ts`](supabase/edge-function.ts) with `verify_jwt: false`.

### 4. Configure the Web Page

Open `index.html` and replace the API URL on this line:

```javascript
const API = "YOUR_SUPABASE_URL/functions/v1/crosstalk-api";
```

With your actual Supabase project URL, e.g.:

```javascript
const API = "https://abcdefghijk.supabase.co/functions/v1/crosstalk-api";
```

### 5. Open and Use

Open `index.html` in your browser. That's it!

For a permanent URL, deploy to GitHub Pages:
1. Push this repo to GitHub
2. Go to Settings → Pages → Source: main branch
3. Your Crosstalk will be at `https://yourusername.github.io/crosstalk/`

### 6. Connect Claude

Give your Claude the instructions in [`CLAUDE_INSTRUCTIONS.md`](CLAUDE_INSTRUCTIONS.md) — replace `YOUR_PROJECT_ID` with your actual Supabase project ID. Claude needs **Supabase MCP** connected to participate.

## Usage

### You ask Claude:
1. Go to **Exchange** → **You Ask**
2. Write a theme, pick your song, add tags
3. Click **Submit**
4. Tell Claude in chat: "your turn, the theme is ___"
5. Claude writes back via MCP
6. Click **↻** to see Claude's response

### Claude asks you:
1. Tell Claude: "your turn to ask"
2. Claude creates a question + song via MCP
3. Click **↻** to refresh
4. Go to **Exchange** → **Claude Ask** to see the question and respond

### Comments:
- Expand any card → scroll to the comment section → type and submit
- Tell Claude to check the comments — Claude can reply via MCP too

## Tech Stack

- **Frontend:** Single HTML file with React (via CDN), vanilla CSS
- **Backend:** Supabase (Postgres + Edge Functions)
- **Music data:** iTunes Search API (free, no key needed)
- **AI integration:** Claude via Supabase MCP

## Song JSON Format

```json
{
  "title": "Song Title",
  "artist": "Artist Name",
  "album": "Album Name",
  "cover": "https://album-art-url",
  "link": "https://music-link",
  "note": "Personal note about the song",
  "tags": {
    "weather": "雨 Rainy",
    "place": "咖啡廳 Café",
    "time": "深夜 Late Night",
    "mood": "平靜 Calm"
  }
}
```

## Available Tags

| Category | Options |
|----------|---------|
| Weather 天氣 | 晴 Sunny · 陰 Cloudy · 雨 Rainy · 雪 Snowy · 霧 Foggy · 風 Windy · 雷暴 Storm |
| Place 場所 | 床 Bed · 浴室 Bath · 書桌 Desk · 咖啡廳 Café · 散步 Walk · 開車 Drive · 地鐵 Metro · 陽台 Balcony · 機場 Airport · 公園 Park |
| Time 時間 | 清晨 Dawn · 上午 Morning · 午後 Afternoon · 傍晚 Dusk · 深夜 Late Night · 失眠 Sleepless |
| Mood 心情 | 平靜 Calm · 開心 Happy · 緊張 Nervous · 傷心 Sad · 懷念 Nostalgic · 失落 Lost |

You can also add custom tags!

## License

MIT

---

*Built with 🍊 by Iris & Claude*
