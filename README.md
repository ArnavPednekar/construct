# Construct — MeetingOS

> Turn raw meeting notes into action items, Google Calendar events, and Slack notifications — instantly.

![MeetingOS](https://img.shields.io/badge/status-active-brightgreen) ![React](https://img.shields.io/badge/built%20with-React-61DAFB) ![Cloudflare Workers](https://img.shields.io/badge/proxy-Cloudflare%20Workers-F38020)

---

## What it does

Construct is an AI-powered meeting action extractor. Paste your meeting notes (or load them directly from Google Drive), and the AI will:

- Extract a **meeting title**, **date**, and **summary**
- Pull out all **action items** with owners, deadlines, and priorities
- Let you **add tasks to Google Calendar** with one click
- Automatically **post tasks to your Slack #tasks channel** when added to Calendar

---

## Features

- **AI Pipeline** — Analyzes raw meeting notes and structures them into tasks
- **Google Drive integration** — Load notes directly from your Drive docs
- **Google Calendar sync** — Add/remove tasks as calendar events with priority labels
- **Slack integration** — Auto-posts to `#tasks` when a task is added to Calendar
- **Inline editing** — Edit task names, deadlines, and cycle priorities on the fly
- **Task management** — Remove and restore tasks, track what's been scheduled
- **Clean UI** — Color-coded owners, priority badges, and smooth animations

---

## Tech Stack

| Layer | Tech |
|---|---|
| Frontend | React (JSX) |
| AI | OpenRouter API |
| Auth & Files | Google OAuth + Drive API |
| Calendar | Google Calendar API |
| Slack | Slack Bot API via Cloudflare Worker proxy |
| Proxy | Cloudflare Workers |

---

## Setup

### 1. Clone the repo

```bash
git clone https://github.com/Anxietyop/construct.git
cd construct
npm install
```

### 2. Google OAuth

Create a project at [console.cloud.google.com](https://console.cloud.google.com) and enable:
- Google Drive API
- Google Calendar API

Add your domain to the OAuth consent screen and grab your **Client ID**.

Update the `CLIENT_ID` in `src/meeting-action-extractor.jsx`:
```js
const CLIENT_ID = "your-client-id.apps.googleusercontent.com";
```

### 3. OpenRouter API Key

Get a key from [openrouter.ai](https://openrouter.ai) and update:
```js
"Authorization": "Bearer your-openrouter-key"
```

### 4. Slack Bot

1. Create a Slack app at [api.slack.com/apps](https://api.slack.com/apps)
2. Add bot scopes: `chat:write`, `chat:write.public`, `channels:read`
3. Install to workspace and copy the `xoxb-...` token
4. Invite the bot to your channel: `/invite @your-app-name`

### 5. Cloudflare Worker (Slack Proxy)

Since browsers can't call the Slack API directly (CORS), a Cloudflare Worker acts as a proxy.

1. Create a new Worker at [dash.cloudflare.com](https://dash.cloudflare.com)
2. Paste the code from `workers/cloudflare-worker.js`
3. Deploy and copy the worker URL
4. Update in `src/meeting-action-extractor.jsx`:
```js
await fetch("https://your-worker.workers.dev", { ... })
```

### 6. Run

```bash
npm run dev
```

---

## How to use

1. Click **Connect Google** to authenticate
2. Pick a file from Google Drive or paste meeting notes directly
3. Hit **▶ Run Pipeline**
4. Review extracted tasks — edit names, deadlines, or cycle priorities
5. Click **+ Cal** on any task to add it to Google Calendar and notify Slack
6. Or hit **Add all to Calendar** to schedule everything at once

---

## Project Structure

```
construct/
├── src/
│   └── meeting-action-extractor.jsx   # Main app
├── workers/
│   └── cloudflare-worker.js           # Slack proxy worker
├── backend/
│   └── server.js                      # (optional backend)
└── README.md
```

---

## Contributing

PRs welcome. Built by [@Anxietyop](https://github.com/Anxietyop).
