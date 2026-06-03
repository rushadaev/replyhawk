# ReplyHawk — what's where

The whole product is three repos under `~/Desktop/ALEX/`. This file lives in the
landing-page repo but documents all three, so there's one place to look.

## The three live pieces

### 1. `replyhawk/`  →  the landing page (this repo)
- Static HTML/CSS, no build step.
- Hosted on **GitHub Pages** at **https://reply-hawk.com** (CNAME in repo root).
- Files: `index.html` (landing), `privacy.html`, `terms.html`.
- Repo: github.com/rushadaev/replyhawk

### 2. `lead-bot-next/`  →  the cloud
- **Next.js 16** (App Router) + **Postgres** (Drizzle ORM).
- Hosted on **Railway**: https://lead-bot-next-production.up.railway.app
- Repo: github.com/rushadaev/lead-bot-next
- Responsibilities:
  - **Dashboard** (`/`) — kanban + table of leads, lead drawer, activity timeline, tags, notifications bell.
  - **Ingest API** (`POST /api/leads`) — every source pushes leads here.
  - **Call API** (`POST /api/calls/leads/:id`) — places an ElevenLabs outbound call.
  - **Webhook** (`POST /api/calls/webhook`) — ElevenLabs post-call transcript + outcome, HMAC-verified.
  - **MCP server** (`/api/mcp`) — 9 tools the voice agent calls live during a call
    (log_event, add_tag, update_lead, append_note, escalate_to_owner, get_lead,
    create_lead, book_appointment, remove_tag).
  - **Multi-tenant**: `businesses` + `agent_tokens` tables; every row scoped by `business_id`.
    A device authenticates with its agent token (Bearer).

### 3. `replyhawk-agent/`  →  the desktop app
- **Electron + React + TypeScript** (electron-vite).
- Runs on the **operator's own Mac** so all platform traffic comes from their real
  IP and real Chrome — looks like the pro using their own account (because it is).
- Repo: github.com/rushadaev/replyhawk-agent
- Responsibilities:
  - Pair with the cloud using an agent token (stored encrypted via Electron safeStorage).
  - Launch the operator's system **Chrome** with an isolated profile per platform
    (Yelp on CDP port 19222, Thumbtack on 19223). User logs in once.
  - After login, swap Chrome to **headless** so it runs invisibly in the background.
  - **Watchers** poll Yelp `leads_center` and Thumbtack `pro-inbox` every 30s, diff
    against a snapshot, and POST new leads / messages to the cloud's `/api/leads`.
  - Live-view screenshots + poll logs in the UI so the operator can see it working.

## The flow, end to end

```
Yelp / Thumbtack
      │  (operator's Chrome, on their Mac)
      ▼
replyhawk-agent  ──POST /api/leads──►  lead-bot-next (cloud)
                                            │
                          dashboard shows the lead live
                                            │
                       operator (or auto) clicks "Call"
                                            ▼
                            ElevenLabs voice agent "Alex"
                                            │ calls customer, books job
                                            │ uses MCP tools to log/tag/book
                                            ▼
                       webhook → cloud → dashboard timeline
```

## Voice agent

- Built in the **ElevenLabs** dashboard (agent id in `lead-bot-next/.env`).
- Connected to the cloud's MCP server for live tool calls.
- Twilio number bridged for inbound + outbound PSTN.
- System prompt lives in `lead-bot-next/agent-prompt.md`.

## Not yet built (roadmap)

- **Text reply-back** to Yelp/Thumbtack from the agent (currently reads only; replies happen via phone call).
- **Follow-up sequences** (10min / 1hr / 24hr re-engagement).
- **SMS-to-owner** alerts (notifications table is ready; Twilio SMS sender not wired).
- **Google LSA** source.
- **Windows** build of the desktop app.
- **Code-signing + notarization + auto-update** for the Mac app.

## Archived (not in use)

`~/Desktop/ALEX/_archive/` holds v1 (Python Gmail email-parser) and v2 (Hono + SQLite cloud).
Both fully superseded by the three repos above.
`~/Desktop/ALEX/YELP-devtools/` holds the original Playwright scratch scripts — kept for
debugging scrapers when a platform changes its HTML; not part of the product.
