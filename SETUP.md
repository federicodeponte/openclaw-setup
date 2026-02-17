# OpenClaw Setup Instructions

This file contains step-by-step instructions for setting up OpenClaw.
Claude Code or Codex can read and execute these steps automatically.

---

## Step 1: Create Hetzner Server

1. Go to https://console.hetzner.cloud
2. Create new project "openclaw"
3. Add server:
   - Location: Nuremberg or closest
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

# Install Python, pip, git
apt install -y python3 python3-pip python3-venv git curl

# Install Node.js (for some plugins)
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt install -y nodejs
```

## Step 4: Clone OpenClaw

```bash
cd /root
git clone https://github.com/openclaw/openclaw.git
cd openclaw
```

## Step 5: Create Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Step 6: Configure Environment

Create `.env` file:

```bash
cat > .env << 'EOF'
# LLM Configuration
GOOGLE_API_KEY=your_gemini_api_key_here

# WhatsApp (will be configured automatically)
WHATSAPP_ENABLED=true

# Gmail (optional)
GMAIL_ENABLED=false
GMAIL_ADDRESS=your_email@gmail.com
GMAIL_APP_PASSWORD=your_app_password

# GitHub (for memory/context)
GITHUB_TOKEN=your_github_token
GITHUB_REPO=your_username/openclaw-memory
EOF
```

## Step 7: Get API Keys

### Gemini API Key (Free)
1. Go to https://aistudio.google.com/apikey
2. Create API key
3. Add to `.env` as `GOOGLE_API_KEY`

### GitHub Token (Free)
1. Go to https://github.com/settings/tokens
2. Generate new token (classic)
3. Select scopes: `repo`
4. Add to `.env` as `GITHUB_TOKEN`

### Gmail App Password (Optional)
1. Go to https://myaccount.google.com/apppasswords
2. Create app password for "Mail"
3. Add to `.env` as `GMAIL_APP_PASSWORD`

## Step 8: Create Memory Repo

```bash
# Create a private repo for OpenClaw's memory
gh repo create openclaw-memory --private --clone
```

Or manually create a private repo on GitHub called `openclaw-memory`.

## Step 9: Start OpenClaw

```bash
cd /root/openclaw
source venv/bin/activate
python main.py
```

## Step 10: Connect WhatsApp

1. A QR code will appear in the terminal
2. Open WhatsApp on your phone
3. Go to Settings > Linked Devices > Link a Device
4. Scan the QR code
5. Done! Message yourself to test

## Step 11: Run as Service (Optional)

Create systemd service to run on boot:

```bash
cat > /etc/systemd/system/openclaw.service << 'EOF'
[Unit]
Description=OpenClaw AI Assistant
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/openclaw
Environment=PATH=/root/openclaw/venv/bin
ExecStart=/root/openclaw/venv/bin/python main.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable openclaw
systemctl start openclaw
```

## Verification

1. Send a message to yourself on WhatsApp
2. OpenClaw should respond
3. Try: "What can you do?"

## Troubleshooting

### QR Code Not Showing
- Make sure port 3000 is not blocked
- Try: `python main.py --show-qr`

### WhatsApp Disconnects
- Re-scan QR code
- Check internet connection on server

### Gmail Not Working
- Make sure 2FA is enabled on Google account
- Use App Password, not regular password

---

## Notes for Claude Code

When executing these steps:
1. Ask the user for their Hetzner credentials or guide them to create an account
2. Ask for their Gemini API key (or guide them to create one)
3. Ask if they want Gmail integration
4. Create the server and run all commands via SSH
5. Show the QR code and wait for them to scan
6. Verify the setup works before finishing
