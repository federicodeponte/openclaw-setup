# OpenClaw Setup Instructions

This guide helps Claude Code/Codex set up OpenClaw on a Hetzner server automatically.

**Official docs:** https://docs.openclaw.ai/start/getting-started

---

## What You Need (Get These First)

1. **Hetzner API Token**
   - Go to https://console.hetzner.cloud
   - Create a project (or use existing)
   - Security > API Tokens > Generate API Token (Read & Write)
   - Copy it

2. **LLM API Key** (pick one)
   - **Gemini (free):** https://aistudio.google.com/apikey
   - **Anthropic (best):** https://console.anthropic.com/settings/keys

3. **SSH Key** (if you don't have one)
   ```bash
   ssh-keygen -t ed25519 -C "openclaw"
   cat ~/.ssh/id_ed25519.pub
   ```

---

## Setup Flow

### Step 1: Install hcloud CLI (Local Machine)

```bash
# macOS
brew install hcloud

# Linux
curl -sSLO https://github.com/hetznercloud/cli/releases/latest/download/hcloud-linux-amd64.tar.gz
sudo tar -C /usr/local/bin --no-same-owner -xzf hcloud-linux-amd64.tar.gz
rm hcloud-linux-amd64.tar.gz
```

### Step 2: Configure hcloud

```bash
# Create context (paste your API token when prompted)
hcloud context create openclaw
```

### Step 3: Upload SSH Key

```bash
# Check if key already exists
hcloud ssh-key list

# If not, create it:
hcloud ssh-key create --name openclaw-key --public-key-from-file ~/.ssh/id_ed25519.pub
```

### Step 4: Create Server

```bash
# Check if server already exists
hcloud server list

# If not, create it:
hcloud server create \
  --name openclaw \
  --type cpx22 \
  --image ubuntu-24.04 \
  --location nbg1 \
  --ssh-key openclaw-key
```

Save the IP address from the output.

### Step 5: SSH Into Server

```bash
# Wait 30s for boot, then:
ssh root@<SERVER_IP>
```

### Step 6: Install OpenClaw (On Server)

```bash
apt update && apt upgrade -y
curl -fsSL https://openclaw.ai/install.sh | bash
source ~/.bashrc
openclaw --version
```

### Step 7: Run Onboarding (Non-Interactive)

**With Gemini:**
```bash
openclaw onboard \
  --non-interactive \
  --accept-risk \
  --install-daemon \
  --auth-choice gemini-api-key \
  --gemini-api-key YOUR_KEY_HERE
```

**With Anthropic:**
```bash
openclaw onboard \
  --non-interactive \
  --accept-risk \
  --install-daemon \
  --auth-choice anthropic-api-key \
  --anthropic-api-key YOUR_KEY_HERE
```

### Step 8: Verify

```bash
openclaw gateway status
openclaw doctor
```

### Step 9: Connect WhatsApp (Only Manual Step)

```bash
openclaw channels add --channel whatsapp
openclaw channels login --channel whatsapp
```

1. QR code appears in terminal
2. WhatsApp > Settings > Linked Devices > Link a Device
3. Scan QR code
4. Done

### Step 10: Test

```bash
# Send yourself a WhatsApp message, or:
openclaw agent --message "Hello, what can you do?"
```

---

## Troubleshooting

**"SSH key already exists"**
```bash
hcloud ssh-key list  # Find existing key name
# Use that name in server create, or delete and recreate
```

**"Server name already exists"**
```bash
hcloud server list
hcloud server delete openclaw  # If you want to start fresh
```

**"Connection refused" on SSH**
- Wait 60 seconds for server to fully boot
- Check: `hcloud server describe openclaw`

**"openclaw: command not found"**
```bash
source ~/.bashrc
# Or add to PATH manually:
export PATH="$HOME/.openclaw/bin:$PATH"
```

**Gateway not starting**
```bash
journalctl -u openclaw -f  # Check logs
openclaw doctor            # Run diagnostics
```

---

## Cleanup (Delete Everything)

```bash
# Delete server (stops billing)
hcloud server delete openclaw

# Optional: remove SSH key from Hetzner
hcloud ssh-key delete openclaw-key

# Optional: remove hcloud context
hcloud context delete openclaw
```

---

## Useful Commands

```bash
openclaw doctor              # Health check
openclaw gateway status      # Gateway status
openclaw update              # Update to latest
openclaw channels list       # List connected channels
journalctl -u openclaw -f    # View logs
systemctl restart openclaw   # Restart daemon
```

---

## Notes for Claude Code

**Ask user for:**
1. Hetzner API token
2. LLM API key (Gemini or Anthropic)

**Automation flow:**
```bash
# 1. Setup hcloud (user pastes token when prompted)
hcloud context create openclaw

# 2. Upload SSH key (check first)
hcloud ssh-key list || hcloud ssh-key create --name openclaw-key --public-key-from-file ~/.ssh/id_ed25519.pub

# 3. Create server (check first)
hcloud server list | grep openclaw || hcloud server create --name openclaw --type cpx22 --image ubuntu-24.04 --location nbg1 --ssh-key openclaw-key

# 4. Get IP
SERVER_IP=$(hcloud server ip openclaw)

# 5. Wait and SSH
sleep 30
ssh root@$SERVER_IP

# 6. On server: install + onboard
apt update && apt upgrade -y
curl -fsSL https://openclaw.ai/install.sh | bash
source ~/.bashrc
openclaw onboard --non-interactive --accept-risk --install-daemon --auth-choice gemini-api-key --gemini-api-key <KEY>

# 7. Verify
openclaw gateway status
openclaw doctor

# 8. WhatsApp (user scans QR)
openclaw channels add --channel whatsapp
openclaw channels login --channel whatsapp
```

**Only manual step:** WhatsApp QR scan
