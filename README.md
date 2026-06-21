# WorldPulse — Crucix Intelligence Engine

**Version:** v1.0  
**Status:** Active Development  
**Repository:** https://github.com/OneByJorah/WorldPulse

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Features](#features)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Service Management](#service-management)
- [Project Structure](#project-structure)
- [Screenshots](#screenshots)
- [Contributing](#contributing)
- [License](#license)
- [Author](#author)

---

## Overview

WorldPulse is a live intelligence terminal dashboard that aggregates 27 OSINT sources with a Jarvis-style UI. It supports alerts, two-way Telegram/Discord bots, and local SSE updates for real-time situational awareness.

The engine sweeps sources, generates briefings, and synthesizes insights via an LLM provider, all served over Express.

---

## Architecture

Client browser → Express server (`server.mjs`) → sweep cycle → source modules (`apis/sources/*.mjs`) → LLM synthesis → SSE live pushes. Notifications flow out via Telegram/Discord alerters.

Data path:
- Runs stored in `./runs/` (Docker volume).
- Dashboard rendered from `dashboard/public/jarvis.html`.
- Briefing templates and prompts in `apis/`.

---

## Technology Stack

| Layer | Stack |
|---|---|
| Runtime | Linux (Ubuntu 22.04+) / Docker |
| Backend | Node.js / Express |
| Intelligence | 27 OSINT source modules (mjs) |
| LLM | Pluggable provider (`lib/llm/`) |
| Frontend | HTML5 Jarvis-style dashboard |
| Notifications | Telegram / Discord |
| VCS | Git + GitHub (`github.com/OneByJorah/WorldPulse`) |

---

## Features

- **27 OSINT sources**: ADSB, BLS, CISA KEV, Cloudflare Radar, FRED, GDELT, KiwiSDR, NOAA, OpenSky, Reddit, ReliefWeb, and more.
- **Jarvis UI**: dark-themed terminal dashboard with live SSE updates.
- **Briefing pipeline**: auto-generated briefings from source sweeps.
- **Two-way bots**: Telegram and Discord integration.
- **Dockerized**: single `docker-compose.yml` deploy.
- **Localization**: built-in i18n support (`lib/i18n.mjs`).

---

## Getting Started

```bash
# 1. Clone
git clone https://github.com/OneByJorah/WorldPulse.git
cd WorldPulse

# 2. Configure
cp .env.example .env   # if present
# Edit .env with Telegram/Discord/LLM credentials

# 3. Run
docker compose up -d

# 4. Verify
curl http://localhost:3117
```

Or run directly:

```bash
node server.mjs
```

---

## Environment Variables

Configure via `.env` (refer to `.env.example` if present):

| Variable | Purpose |
|---|---|
| `PORT` | HTTP port (default `3117`) |
| `LLM_*` | LLM provider credentials |
| `TELEGRAM_*` | Telegram bot token/chat |
| `DISCORD_*` | Discord webhook/token |

Keep `.env` out of VCS.

---

## Service Management

```bash
# Start
docker compose up -d

# Logs
docker compose logs -f WorldPulse

# Stop
docker compose down
```

Local SSE endpoint is available under `/` or the dashboard path.

---

## Project Structure

```
WorldPulse/
├── server.mjs                  # Express dev server + sweep cycle
├── docker-compose.yml
├── apis/
│   ├── briefing.mjs
│   ├── save-briefing.mjs
│   └── sources/*.mjs           # 27 OSINT modules
├── dashboard/
│   └── public/
│       └── jarvis.html         # Jarvis-style UI
├── docs/
│   ├── boot.png
│   ├── dashboard.png
│   ├── globe.png
│   └── map.png
└── runs/                       # Runtime output (Docker volume)
```

---

## Screenshots

All screenshots are live captures from the local deployment.

### Boot
![Boot](docs/boot.png)

### Dashboard
![Dashboard](docs/dashboard.png)

### Globe
![Globe](docs/globe.png)

### Map
![Map](docs/map.png)

---

## Contributing

1. Create a feature branch off `main`.
2. Add source modules under `apis/sources/` with clear licensing and data attribution.
3. Submit a PR with description and screenshots for UI changes.

---

## License

MIT

---

## Author

Built by **Jhonattan L. Jimenez**.
