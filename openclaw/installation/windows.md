# OpenClaw on Native Windows

This guide covers installing OpenClaw directly on Windows without WSL. While WSL (Windows Subsystem for Linux) is recommended for better performance, OpenClaw works on native Windows with a straightforward installation.

## Prerequisites

- **Windows 10 or Windows 11**
- **Administrator access**
- **Node.js 26 (recommended)** or Node.js 22.22.3+, Node 24.15+, or Node 25.9+
- **2 GB free disk space**
- **API key from an AI provider** (Claude, GPT, Gemini, etc.)

## Step 1 — Install or Verify Node.js

OpenClaw requires Node.js. Check if it's installed:

**Open PowerShell (Admin):**
1. Press `Win + X`
2. Select **Windows PowerShell (Admin)**

Run:

```powershell
node --version
npm --version
```

**If both commands show version numbers** (e.g., `v26.0.0` and `10.0.0`), skip to Step 2.

**If you see "command not found"**, download and install Node.js:

1. Go to [nodejs.org](https://nodejs.org/)
2. Download **LTS version 26** (or latest recommended)
3. Run the installer and follow the prompts
4. When asked, check **"Add to PATH"**
5. Finish the installation
6. **Close and reopen PowerShell (Admin)**
7. Verify:
   ```powershell
   node --version
   npm --version
   ```

## Step 2 — Install OpenClaw

In PowerShell (Admin), run:

```powershell
npm install -g openclaw@latest --allow-scripts=openclaw
```

**Installation takes 1–2 minutes.** You'll see many lines of text as packages download. This is normal.

After completion, verify:

```powershell
openclaw --version
```

You should see a version number (e.g., `1.2.3`).

## Step 3 — Run Onboarding

Start the guided setup:

```powershell
openclaw onboard --install-daemon
```

The wizard will:

1. Ask for your AI provider (OpenAI, Anthropic Claude, Google Gemini, etc.)
2. Open your browser to get an API key
3. Ask for a messaging channel (Telegram, WhatsApp, Discord, or skip)
4. Configure and start the Gateway

**Follow the prompts** in your terminal and browser. After completion, you'll see:

```
Gateway started on http://127.0.0.1:18789/
```

## Step 4 — Access OpenClaw

Your Gateway is running. Test it:

**Option A: Web Dashboard** (easiest)

Open your browser and go to:

```
http://127.0.0.1:18789/
```

Send a test message like `"Hello"`. Your AI should respond.

**Option B: Command Line**

In PowerShell, run:

```powershell
openclaw dashboard
```

This also opens the web dashboard.

**Option C: Add a Channel**

From the web dashboard, set up Telegram, WhatsApp, Discord, or another channel to message OpenClaw from your phone.

## Step 5 — Verify Installation

In PowerShell, run:

```powershell
openclaw status
```

You should see:

```
Gateway is running at http://127.0.0.1:18789/
```

## Step 6 — Stop and Manage OpenClaw

**To stop the Gateway:**

```powershell
openclaw stop
```

**To start it again:**

```powershell
openclaw start
```

**To view logs:**

```powershell
openclaw logs
```

**To update to the latest version:**

```powershell
npm update -g openclaw
openclaw stop
openclaw start
```

## Windows-Specific Notes

### Performance

Native Windows is slower than WSL for file operations and command execution. If you notice slow responses:

- **Switch to WSL** for better performance (see [Windows + WSL guide](./windows-wsl.md))
- Use a faster model (e.g., Claude 3.5 Haiku instead of a larger model)
- Close other heavy applications

### Firewall

If the dashboard doesn't open, Windows Firewall might be blocking port 18789.

**To allow it:**

1. Press `Win + S` and search for "Firewall"
2. Open **Windows Defender Firewall > Advanced Settings**
3. Click **Inbound Rules > New Rule**
4. Select **Port > TCP > 18789**
5. Name it "OpenClaw" and finish

Then try again: `http://127.0.0.1:18789/`

### Copy/Paste in Terminal

Windows PowerShell copy/paste works differently than Linux. Use:

- **Right-click** to paste into PowerShell
- **Ctrl + Shift + V** in Windows Terminal

## Common Problems

### "Command not found: openclaw"

**Solution:**
1. Close and reopen PowerShell (Admin)
2. Verify Node.js is installed: `node --version`
3. Reinstall OpenClaw:
   ```powershell
   npm install -g openclaw@latest --allow-scripts=openclaw
   ```

### "Permission denied" or "Access denied"

**Solution:**
- Run PowerShell as Administrator (right-click > Run as administrator)
- Uninstall and reinstall:
  ```powershell
  npm uninstall -g openclaw
  npm install -g openclaw@latest --allow-scripts=openclaw
  ```

### Gateway won't start or crashes

**Solution:**
1. Check logs:
   ```powershell
   openclaw logs
   ```
2. Port 18789 might be in use. Kill it and restart:
   ```powershell
   Get-NetTCPConnection -LocalPort 18789 | Stop-Process -Force
   openclaw start
   ```
3. Re-run onboarding:
   ```powershell
   openclaw onboard --install-daemon
   ```

### "API key rejected" or "Can't connect to model"

**Solution:**
1. Get a fresh API key from your provider
2. Re-run onboarding:
   ```powershell
   openclaw onboard
   ```
3. Select your provider and paste the new key

### Dashboard won't open in browser

**Solution:**
1. Verify Gateway is running:
   ```powershell
   openclaw status
   ```
2. Try a different browser
3. Use the exact URL: `http://127.0.0.1:18789/` (not `https://`)
4. Check Windows Firewall isn't blocking it

## When to Use WSL Instead

Consider switching to [WSL + Windows guide](./windows-wsl.md) if you:

- Notice slow command execution
- Need to run Bash scripts or Linux tools
- Experience frequent performance issues
- Want the full Linux environment for advanced workflows

WSL provides significantly better performance with minimal extra setup.

## Next Steps

1. **[Complete basic setup](../configuration/basic-setup.md)** — Configure your preferred model and channels
2. **Add a messaging channel** (Telegram, WhatsApp, Discord)
3. **Explore troubleshooting** if you hit any issues

## Recommended Reading

- [OpenClaw Full Docs](https://docs.openclaw.ai/)
- [Back to OpenClaw Overview](../README.md)
