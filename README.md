# 🦞 OpenClaw Setup

Set up your own personal AI assistant in 10 minutes. Guided by AI, minimal effort.

**Official OpenClaw docs:** https://docs.openclaw.ai

## What You Get

- A personal AI assistant running 24/7
- Message it on WhatsApp, Telegram, Discord, Slack, Signal, iMessage, etc.
- It remembers context across conversations
- Extensible with plugins (email, calendar, etc.)
- Costs ~€5/month (Hetzner server)

## Prerequisites

1. **Claude Code** or **Codex** installed
2. A Hetzner Cloud account → [Sign up](https://hetzner.cloud)
3. An LLM provider (Gemini free tier works, or Anthropic for best quality)

## Quick Start

### Option A: Full Automation (Recommended)

1. Get your Hetzner API token from console.hetzner.cloud > Security > API Tokens
2. Open Claude Code or Codex
3. Say:

```
Read github.com/federicodeponte/openclaw-setup/SETUP.md and set up OpenClaw for me. Here's my Hetzner API token: <paste token>
```

4. Claude creates the server, installs everything, guides you through config
5. Scan WhatsApp QR code when prompted
6. Done!

### Option B: Manual Server + Guided Install

1. Create a server manually at console.hetzner.cloud (Ubuntu 24.04, CPX22)
2. Open Claude Code and say:

```
Read github.com/federicodeponte/openclaw-setup/SETUP.md and help me install OpenClaw on my server at <IP>
```

## What Claude Does vs. What You Do

| Claude Does | You Do |
|-------------|--------|
| Creates Hetzner server | Provide Hetzner API token |
| SSHs in and installs OpenClaw | Provide LLM API key (Gemini/Anthropic) |
| Runs non-interactive onboard | Scan WhatsApp QR code |
| Configures everything | That's it |
| Verifies setup works | |

**Only manual step:** Scanning the WhatsApp QR code with your phone.

## What Happens

1. Creates a Hetzner server (~€5/month)
2. Installs OpenClaw via official script
3. Runs non-interactive onboard with your LLM API key
4. Installs daemon for 24/7 operation
5. You scan WhatsApp QR code
6. Done - message your assistant anytime

## Stack

| Component | What | Cost |
|-----------|------|------|
| Server | Hetzner CPX22 | ~€5/mo |
| LLM | Anthropic or Gemini | Varies (free tier available) |
| Channels | WhatsApp, Telegram, Discord, etc. | Free |
| Agent | OpenClaw | Free (MIT license) |

## Manual Setup

See [SETUP.md](SETUP.md) for step-by-step instructions.

## Links

- [OpenClaw Docs](https://docs.openclaw.ai)
- [Getting Started](https://docs.openclaw.ai/start/getting-started)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [Discord](https://discord.gg/clawd)
- [Hetzner Cloud](https://hetzner.cloud)

## Questions?

Open an issue or DM me on [LinkedIn](https://linkedin.com/in/federicodeponte).

---

🦞 *Built with [OpenClaw](https://openclaw.ai)*
