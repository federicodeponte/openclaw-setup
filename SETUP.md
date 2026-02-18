# OpenClaw Setup Instructions

This file contains the actual steps to set up OpenClaw.
Claude Code or Codex can read and execute these steps automatically.

---

## Step 1: Create Hetzner Server

1. Go to https://console.hetzner.cloud
2. Create new project "openclaw"
3. Add server:
   - Location: Nuremberg (or closest)
   - Image: Ubuntu 24.04
   - Type: CX22 (2 vCPU, 4GB RAM) - ~€5/month
   - SSH key: Add your public key
4. Note the server IP address

## Step 2: SSH Into Server

```bash
ssh root@<SERVER_IP>
```

## Step 3: Install Dependencies

```bash
# Update system
apt update && apt upgrade -y

# Install Node.js 20+
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt install -y nodejs

# Install Python (for some plugins)
apt install -y python3 python3-pip python3-venv git curl
```

## Step 4: Install OpenClaw

```bash
npm install -g openclaw
```

## Step 5: Run OpenClaw Setup Wizard

```bash
openclaw
```

This will:
1. Create the `.openclaw` directory
2. Ask you to configure your LLM provider (use Google/Gemini - free tier)
3. Set up the WhatsApp plugin
4. Show a QR code to scan with your phone

## Step 6: Configure Google/Gemini API

When prompted, add your Gemini API key:

1. Go to https://aistudio.google.com/apikey
2. Create API key
3. Enter it when openclaw asks

## Step 7: Connect WhatsApp

1. A QR code will appear in the terminal
2. Open WhatsApp on your phone
3. Go to Settings > Linked Devices > Link a Device
4. Scan the QR code
5. Done!

## Step 8: Run as Background Service

```bash
# Create systemd service
cat > /etc/systemd/system/openclaw.service << 'EOF'
[Unit]
Description=OpenClaw AI Assistant
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root
ExecStart=/usr/bin/openclaw
Restart=always
RestartSec=10
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
EOF

# Enable and start
systemctl daemon-reload
systemctl enable openclaw
systemctl start openclaw
```

## Step 9: Check Status

```bash
systemctl status openclaw
```

## Configuration Files

After setup, your config lives at `/root/.openclaw/`:

```
.openclaw/
├── openclaw.json      # Main config
├── agents/            # Your agents
├── credentials/       # API keys
├── memory/            # Agent memory
├── workspace/         # Working files
└── venv/              # Python environment
```

## Verification

1. Send a message to yourself on WhatsApp
2. OpenClaw should respond
3. Try: "What can you do?"

## Useful Commands

```bash
# Check version
openclaw --version

# Run doctor (check health)
openclaw doctor

# View logs
journalctl -u openclaw -f
```

## Current Setup (Federico's)

- Server: Hetzner CX22
- OpenClaw: v2026.2.15
- LLM: Google Gemini (free tier)
- Plugins: WhatsApp enabled
- Agents: main, opendraft

---

## Notes for Claude Code

When executing these steps:
1. Ask the user for their Hetzner credentials or guide them to create an account
2. SSH into the server and run the install commands
3. Guide them through the openclaw wizard
4. Help them get their Gemini API key
5. Wait for them to scan the WhatsApp QR code
6. Set up the systemd service for 24/7 operation
7. Verify everything works
