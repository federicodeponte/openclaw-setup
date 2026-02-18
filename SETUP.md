# OpenClaw Setup Instructions

This file contains the steps to set up OpenClaw on a Hetzner server.
Claude Code or Codex can read and execute these steps automatically.

**Official docs:** https://docs.openclaw.ai/start/getting-started

---

## Prerequisites (User Must Do First)

Before Claude can automate the setup, you need:

1. **Hetzner API Token**
   - Go to https://console.hetzner.cloud
   - Create a project (or use existing)
   - Go to Security > API Tokens > Generate API Token
   - Copy the token (you'll paste it when prompted)

2. **SSH Key** (if you don't have one)
   ```bash
   ssh-keygen -t ed25519 -C "openclaw"
   # Press Enter to accept defaults
   cat ~/.ssh/id_ed25519.pub
   # Copy this public key
   ```

---

## Fully Automated Setup (Recommended)

Claude Code can create the server and install everything. Just provide your Hetzner API token when asked.

### Step 1: Install hcloud CLI (on your local machine)

```bash
# macOS
brew install hcloud

# Linux
curl -sSLO https://github.com/hetznercloud/cli/releases/latest/download/hcloud-linux-amd64.tar.gz
sudo tar -C /usr/local/bin --no-same-owner -xzf hcloud-linux-amd64.tar.gz
rm hcloud-linux-amd64.tar.gz

# Verify
hcloud version
```

### Step 2: Authenticate hcloud

```bash
hcloud context create openclaw
# Paste your Hetzner API token when prompted
```

### Step 3: Upload SSH Key to Hetzner

```bash
hcloud ssh-key create --name openclaw-key --public-key-from-file ~/.ssh/id_ed25519.pub
```

### Step 4: Create Server (One Command)

```bash
hcloud server create \
  --name openclaw \
  --type cx22 \
  --image ubuntu-24.04 \
  --location nbg1 \
  --ssh-key openclaw-key
```

This outputs the server IP. Save it.

### Step 5: Wait for Server + SSH In

```bash
# Wait 30 seconds for server to boot, then:
ssh root@<SERVER_IP>
```

---

## Manual Server Creation (Alternative)

If you prefer the web console:

1. Go to https://console.hetzner.cloud
2. Create new project "openclaw"
3. Add server:
   - Location: Nuremberg (nbg1)
   - Image: Ubuntu 24.04
   - Type: CX22 (2 vCPU, 4GB RAM) - ~€5/month
   - SSH key: Add your public key
4. Note the server IP address

## Step 2: SSH Into Server

```bash
ssh root@<SERVER_IP>
```

## Step 3: Install OpenClaw

Use the official install script (handles Node.js dependency automatically):

```bash
# Update system first
apt update && apt upgrade -y

# Install OpenClaw (official method from docs.openclaw.ai)
curl -fsSL https://openclaw.ai/install.sh | bash
```

This script:
- Installs Node.js 22+ if needed
- Installs OpenClaw globally
- Sets up PATH

**Alternative (manual):** If you prefer manual installation:
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
apt install -y nodejs
npm install -g openclaw@latest
```

## Step 4: Verify Installation

```bash
openclaw --version
# Should show: 🦞 OpenClaw 2026.x.x
```

## Step 5: Run Onboarding Wizard (from official docs)

```bash
openclaw onboard --install-daemon
```

The wizard will:
1. Set up the Gateway daemon (systemd service)
2. Ask you to configure your LLM provider
3. Set up channels (WhatsApp, Telegram, etc.)
4. Guide you through authentication

## Step 6: Configure LLM Provider

**Recommended:** Anthropic Pro/Max subscription (best quality)

**Free option:** Google Gemini API
1. Go to https://aistudio.google.com/apikey
2. Create API key
3. Enter when prompted by wizard

## Step 7: Connect WhatsApp

During the wizard, when you enable WhatsApp:

1. A QR code will appear in the terminal
2. Open WhatsApp on your phone
3. Go to Settings > Linked Devices > Link a Device
4. Scan the QR code
5. Done!

## Step 8: Verify Setup

```bash
# Gateway status (from official docs)
openclaw gateway status

# Health check
openclaw doctor

# Check version
openclaw --version

# View logs
journalctl -u openclaw -f

# Open dashboard (optional)
openclaw dashboard
```

## Step 9: Send Test Message

```bash
# Via CLI
openclaw agent --message "Hello, what can you do?"

# Or just message yourself on WhatsApp
```

## Useful Commands

```bash
# Run onboarding again
openclaw onboard

# Health check
openclaw doctor

# Update to latest
openclaw update

# Gateway status
systemctl status openclaw
```

## Configuration

Config lives at `~/.openclaw/`:

```
.openclaw/
├── openclaw.json      # Main config
├── agents/            # Your agents
├── credentials/       # API keys (encrypted)
├── memory/            # Agent memory
├── workspace/         # Working files
└── logs/              # Logs
```

## Resources

- **Docs:** https://docs.openclaw.ai
- **Getting Started:** https://docs.openclaw.ai/start/getting-started
- **Channels:** https://docs.openclaw.ai/channels
- **WhatsApp:** https://docs.openclaw.ai/channels/whatsapp
- **Discord:** https://discord.gg/clawd

---

## Notes for Claude Code

When executing these steps:

### If user has hcloud CLI set up:
1. Run `hcloud server create --name openclaw --type cx22 --image ubuntu-24.04 --location nbg1 --ssh-key openclaw-key`
2. Wait 30s, then SSH into the new server
3. Run `apt update && apt upgrade -y`
4. Run `curl -fsSL https://openclaw.ai/install.sh | bash`
5. Run `openclaw onboard --install-daemon`
6. Guide user through wizard prompts (they need to interact)
7. Help them get API keys if needed (Gemini: https://aistudio.google.com/apikey)
8. Wait for WhatsApp QR scan (user must do this)
9. Run `openclaw gateway status` and `openclaw doctor` to verify

### If user doesn't have hcloud:
1. Ask them to create server manually via console.hetzner.cloud
2. Get the server IP from them
3. Continue from step 2 above

### Interactive steps (cannot be automated):
- Hetzner API token input
- `openclaw onboard` wizard prompts
- WhatsApp QR code scanning
- LLM API key entry
