# OpenClaw Setup

Set up your own personal AI assistant in 10 minutes. No coding required.

## What You Get

- A personal AI assistant running 24/7
- Message it on WhatsApp like a colleague
- It reads your emails, manages your calendar, remembers your projects
- Costs ~€5/month (Hetzner server)

## Prerequisites

1. **Claude Code** or **Codex** installed on your machine
2. A Hetzner Cloud account (or any VPS provider)
3. A Google account (for Gemini API - free tier)

## Quick Start

1. Open Claude Code or Codex
2. Say:

```
Read github.com/federicodeponte/openclaw-setup/SETUP.md and set up OpenClaw for me on a new Hetzner server
```

3. Follow the prompts (it will ask for your API keys)
4. Scan the WhatsApp QR code when prompted
5. Done. Message your AI on WhatsApp.

## What Claude Code Will Do For You

1. Create a Hetzner server (~€5/month)
2. SSH into it and install Node.js
3. Install OpenClaw via npm (`npm install -g openclaw`)
4. Run the setup wizard
5. Configure Gemini API (free tier)
6. Set up WhatsApp connection (you scan QR code)
7. Create systemd service for 24/7 operation

## Stack

| Component | What | Cost |
|-----------|------|------|
| Server | Hetzner CX22 | ~€5/mo |
| LLM | Gemini (via OpenClaw) | Free tier |
| Interface | WhatsApp | Free |
| Agent | OpenClaw | Free (npm) |

## Manual Setup

If you prefer to do it yourself, see [SETUP.md](SETUP.md) for step-by-step instructions.

## Links

- [OpenClaw](https://openclaw.dev) - The agent framework
- [Claude Code](https://claude.ai/code) - AI coding assistant
- [Hetzner Cloud](https://hetzner.cloud) - VPS hosting

## Questions?

Open an issue or DM me on [LinkedIn](https://linkedin.com/in/federicodeponte).

---

🦞 *Built with OpenClaw*
