# OpenClaw on Windows Using WSL

This is the recommended way to install OpenClaw on Windows. WSL (Windows Subsystem for Linux) gives you a Linux environment running inside Windows, which means better performance and compatibility compared to native Windows.

## Overview

WSL lets you run Linux directly on Windows without a virtual machine. OpenClaw runs best in this Linux environment because:

- Better file system performance when working with code
- Access to standard Linux tools your AI agent will use
- Smoother command execution and shell integration
- Full Node.js compatibility

By the end of this guide, you'll have OpenClaw running as a background service, reachable from WhatsApp, Telegram, Discord, or your browser.

## Prerequisites

Before you start, make sure you have:

- **Windows 10 (Build 2004 or later) or Windows 11**
- **Administrator access** on your computer
- **At least 2 GB of free disk space** for WSL and OpenClaw
- **Internet connection** for downloading and installation
- **A text editor** (Windows Notepad is fine)
- **An API key** from an AI provider (Claude, GPT, Gemini, etc.) — you'll get this during setup

No Linux experience needed. This guide walks through every step.

## Step 1 — Install WSL

Open PowerShell as Administrator:

1. Press `Win + X`, then select **Windows PowerShell (Admin)** or **Windows Terminal (Admin)**
2. You'll see a terminal prompt

In the PowerShell window, run:

```powershell
wsl --install
```

This command:
- Installs WSL 2 (the latest version)
- Installs Ubuntu (the Linux distribution)
- Sets up the integration between Windows and Linux

**This takes 2–5 minutes.** After it finishes, you'll see a message to restart your computer.

Restart your computer when prompted.

## Step 2 — Complete Ubuntu Setup

After restart, Ubuntu starts automatically. You'll see a terminal asking for:

1. **Username**: Create a username (e.g., `lobster`, `user`, `alex`). Use lowercase, no spaces.
2. **Password**: Create a password. You won't see characters as you type—this is normal. Press Enter after typing.
3. **Confirm password**: Type the same password again.

Save your username and password somewhere safe. You might need them later.

Once setup is complete, you'll see a prompt like:

```
username@computer-name:~$
```

**Congratulations!** WSL is now ready. Don't close this window—you're in the Ubuntu terminal.

## Step 3 — Update Ubuntu

Your Linux environment needs the latest packages. In the Ubuntu terminal, run:

```
sudo apt update && sudo apt upgrade -y
```

This command:
- `sudo` — run as administrator
- `apt update` — check for updates
- `apt upgrade -y` — install updates automatically

This takes 1–3 minutes. Let it finish.

After it completes, you're still in the Ubuntu terminal, ready for the next step.

## Step 4 — Check Node.js

OpenClaw requires Node.js 26 (recommended) or Node.js 22.22.3+, Node 24.15+, or Node 25.9+.

Check if Node is installed:

```
node --version
```

If you see a version number like `v26.0.0`, you're good.

**If you see "command not found"**, you need to install Node.js:

```
curl -fsSL https://deb.nodesource.com/setup_26.x | sudo -E bash - && sudo apt install -y nodejs
```

Verify installation:

```
node --version
npm --version
```

Both commands should show version numbers. You're now ready for OpenClaw.

## Step 5 — Install OpenClaw

In the Ubuntu terminal, run:

```
npm install -g openclaw@latest --allow-scripts=openclaw
```

This command:
- `npm install -g` — install globally (available from anywhere)
- `openclaw@latest` — get the newest version
- `--allow-scripts=openclaw` — allow OpenClaw scripts to run (safe)

**Installation takes 1–3 minutes.** You'll see lots of text as packages download. This is normal.

After completion, you'll see a success message. Verify:

```
openclaw --version
```

If you see a version number (e.g., `1.2.3`), OpenClaw is installed.

## Step 6 — Run Onboarding

OpenClaw has a guided setup. Run:

```
openclaw onboard --install-daemon
```

This starts the onboarding wizard:

1. **Read the welcome message**, then press **Enter**
2. **Choose your model provider** (e.g., OpenAI, Anthropic/Claude, Google Gemini)
   - First-timers: Choose **OpenAI** if you have a ChatGPT account, or **Anthropic** if you have Claude
   - The wizard will open your browser and ask for an API key
3. **Follow the browser prompts** to get your API key
   - Copy the key from the provider's dashboard
   - Paste it into the terminal when asked
4. **Choose a channel** (e.g., Telegram, WhatsApp, Discord, or skip for now)
   - You can add channels later
5. **Review the summary**, then press **Enter** to confirm

The `--install-daemon` flag sets OpenClaw to run in the background automatically.

After onboarding completes, you'll see:

```
Gateway started on http://127.0.0.1:18789/
```

**Congratulations!** Your OpenClaw Gateway is running.

## Step 7 — Access OpenClaw

Your Gateway is now running. Access it three ways:

**Option A: Web Dashboard (easiest for testing)**

Open your browser and go to:

```
http://127.0.0.1:18789/
```

You'll see a chat interface. Send a test message like `"Say hello"`. Your AI should respond.

**Option B: Command Line**

To interact with OpenClaw from the terminal:

```
openclaw dashboard
```

This also opens the web dashboard.

**Option C: Add a Channel** (Telegram, WhatsApp, Discord, etc.)

You can message your OpenClaw from any supported app. To set up a channel:

1. Go to the web dashboard: `http://127.0.0.1:18789/`
2. Click **Channels** or **Settings**
3. Select a platform (e.g., Telegram)
4. Follow the setup instructions

For **Telegram** (fastest to set up):
- Search for `@BotFather` on Telegram
- Type `/newbot` and follow prompts to create a bot
- Copy the API token
- Paste it into OpenClaw's Telegram channel settings
- Message your bot from Telegram, and it forwards to OpenClaw

## Step 8 — Verify Installation

Test that everything works:

**In the Ubuntu terminal**, run:

```
openclaw status
```

You should see something like:

```
Gateway is running at http://127.0.0.1:18789/
```

**In your browser**, visit `http://127.0.0.1:18789/` and send a message. You should get a response.

If both work, OpenClaw is fully installed and running.

## Step 9 — Stop and Start OpenClaw

**To stop** the Gateway:

```
openclaw stop
```

**To start** it again:

```
openclaw start
```

Since you installed with `--install-daemon`, OpenClaw starts automatically when Windows boots.

## Step 10 — Update OpenClaw

To get the latest version:

```
npm update -g openclaw
```

Then restart the Gateway:

```
openclaw stop
openclaw start
```

## Useful Commands

Once OpenClaw is running, you can use these commands in the Ubuntu terminal:

```
openclaw --help             # Show all commands
openclaw status             # Check if Gateway is running
openclaw logs               # View Gateway logs
openclaw dashboard          # Open the Control UI in browser
openclaw config             # View current configuration
openclaw onboard            # Re-run the onboarding wizard
openclaw stop               # Stop the Gateway
openclaw start              # Start the Gateway
```

## Accessing from Other Devices

To message OpenClaw from your phone or another computer, you need to expose the Gateway to your network.

**Warning**: Exposing your Gateway to the internet requires careful security setup. Only do this if you understand the risks.

For local network access only:

1. **Find your Windows PC's IP address** in PowerShell:
   ```powershell
   ipconfig
   ```
   Look for "IPv4 Address" (e.g., `192.168.1.100`)

2. **Edit OpenClaw config** in Ubuntu:
   ```
   nano ~/.openclaw/openclaw.json
   ```

3. **Add this line** inside the config:
   ```json
   "gateway": {
     "host": "0.0.0.0"
   }
   ```

4. **Restart OpenClaw**:
   ```
   openclaw restart
   ```

Now devices on your WiFi network can access OpenClaw at:

```
http://YOUR-PC-IP:18789/
```

(Replace `YOUR-PC-IP` with your Windows PC's actual IP.)

For internet access beyond your home network, see [OpenClaw remote access docs](https://docs.openclaw.ai/gateway/remote).

## Common Problems

### "Command not found: openclaw"

**Cause**: Node.js or OpenClaw didn't install correctly, or you're in the wrong terminal.

**Solution**:
1. Make sure you're in the **Ubuntu terminal** (not Windows PowerShell)
2. Verify Node.js is installed:
   ```
   node --version
   ```
3. Reinstall OpenClaw:
   ```
   npm install -g openclaw@latest --allow-scripts=openclaw
   ```

### "Permission denied" or "sudo required"

**Cause**: Node.js/npm permissions are not configured correctly.

**Solution**: Reinstall Node.js with proper permissions:
```
curl -fsSL https://deb.nodesource.com/setup_26.x | sudo -E bash - && sudo apt install -y nodejs
```

### "Gateway won't start" or keeps crashing

**Cause**: Port 18789 is already in use, or there's a configuration error.

**Solution**:
1. Check logs:
   ```
   openclaw logs
   ```
2. Look for error messages
3. Try restarting:
   ```
   openclaw stop
   openclaw start
   ```
4. If that doesn't work, re-run onboarding:
   ```
   openclaw onboard --install-daemon
   ```

### "Can't connect to model" or "API key rejected"

**Cause**: Your API key is invalid or expired.

**Solution**:
1. Get a fresh API key from your provider's dashboard
2. Re-run onboarding:
   ```
   openclaw onboard
   ```
3. Select your provider and paste the new key

### "WSL won't install" (Windows message)

**Cause**: Virtualization is disabled in BIOS, or you're on an older Windows version.

**Solution**:
1. Make sure Windows is fully updated: `Settings > Update & Security > Check for updates`
2. If still blocked, enable Virtualization in BIOS (requires reboot)—search "Enable Virtualization Windows 10" for your specific PC model
3. Try the install again:
   ```powershell
   wsl --install
   ```

### WSL terminal is very slow

**Cause**: You're working on the Windows file system (`/mnt/c/`), which is slower.

**Solution**: Work directly in the Ubuntu home directory:
```
cd ~
```

Instead of `/mnt/c/Users/YourName/...`, work in `~/projects` or another Ubuntu directory.

### "Dashboard not opening" in browser

**Cause**: Firewall is blocking the port, or the Gateway didn't start.

**Solution**:
1. Verify the Gateway is running:
   ```
   openclaw status
   ```
2. Try restarting:
   ```
   openclaw stop
   openclaw start
   ```
3. Try a different browser
4. Make sure you're using the correct URL: `http://127.0.0.1:18789/` (not `https://`)

## Next Steps

1. **Set up a channel** (Telegram, WhatsApp, Discord) to message OpenClaw from your phone
2. **Add your first skill** — extend OpenClaw with custom workflows
3. **Configure multi-agent routing** if you want separate agents for different projects
4. **Read the full docs** at [docs.openclaw.ai](https://docs.openclaw.ai/)

## Back to OpenClaw

- [← Return to OpenClaw Overview](../README.md)
- [→ Continue to Configuration](../configuration/basic-setup.md)
