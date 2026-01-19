# 🧠 Infinite Brainrot Backrooms

Autonomous AI-generated doomscroll feed + Twitter bot. Like Truth Terminal, but for brainrot.

## One Command to Run

```bash
# 1. Copy env file and add your Anthropic key
cp .env.example .env
# Edit .env and add ANTHROPIC_API_KEY

# 2. Run everything
docker compose up --build
```

That's it. No node, no pnpm, no manual migrations.

## What You Get

| URL | Description |
|-----|-------------|
| http://localhost:3000 | Doomscroll feed |
| http://localhost:3000/admin | Admin status page |
| http://localhost:3001/v1/feed | API feed endpoint |
| http://localhost:3001/v1/status | System health |

## How It Works

1. **Worker** generates 5 thoughts every 30 seconds via Claude
2. **Thoughts** are scored, moderated, and stored in Postgres
3. **API** serves the feed via REST + WebSocket for live updates
4. **Web** displays infinite scroll with real-time new thoughts
5. **Twitter** bot tweets top thoughts every 20-60 min (or DRY_RUN if no keys)

## Twitter Setup (Optional)

Add to `.env`:
```
TWITTER_API_KEY=your-key
TWITTER_API_SECRET=your-secret
TWITTER_ACCESS_TOKEN=your-token
TWITTER_ACCESS_SECRET=your-token-secret
```

Without Twitter keys, the system runs in DRY_RUN mode (marks candidates but doesn't post).

## Content Engine Features

- **Trend Spotlight**: 3 trends get boosted each hour
- **Ephemeral References**: Things everyone "knows" that disappear in 6-24h
- **Format Carousel**: No format >25% of recent thoughts
- **Trend Decay**: Trends cycle every hour, max 15% per trend in 6h
- **Retail-Safe Funny**: Edgy but no slurs/violence/real people

## Stopping

```bash
docker compose down        # Stop services
docker compose down -v     # Stop + delete data
```

## Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Next.js   │◄───►│   Fastify   │◄───►│   Worker    │
│   Web UI    │     │    API      │     │  (Claude)   │
└─────────────┘     └─────────────┘     └─────────────┘
                           │                   │
                    ┌──────┴──────┐     ┌──────┴──────┐
                    │  PostgreSQL │     │   Claude    │
                    │   + Redis   │     │    API      │
                    └─────────────┘     └─────────────┘
```

## License

MIT
