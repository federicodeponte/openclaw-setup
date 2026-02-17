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
2. Say: "Read the SETUP.md file in this repo and set up OpenClaw for me on a new Hetzner server"
3. Follow the prompts (it will ask for your API keys)
4. Done. Message your AI on WhatsApp.

## What Claude Code Will Do For You

1. Create a Hetzner server (~€5/month)
2. SSH into it and install dependencies
3. Clone and configure OpenClaw
4. Set up WhatsApp connection (you'll scan a QR code)
5. Connect your Gmail (optional)
6. Start the agent

## Manual Setup

If you prefer to do it yourself, see [SETUP.md](SETUP.md) for step-by-step instructions.

## Stack

| Component | What | Cost |
|-----------|------|------|
| Server | Hetzner CX22 | ~€5/mo |
| LLM | Gemini 3 Flash | Free tier |
| Interface | WhatsApp | Free |
| Memory | GitHub repo | Free |

## Links

- [OpenClaw](https://github.com/openclaw/openclaw) - The agent framework
- [Claude Code](https://claude.ai/code) - AI coding assistant
- [Hetzner Cloud](https://hetzner.cloud) - Cheap VPS hosting

## Questions?

Open an issue or DM me on [LinkedIn](https://linkedin.com/in/federicodeponte).
