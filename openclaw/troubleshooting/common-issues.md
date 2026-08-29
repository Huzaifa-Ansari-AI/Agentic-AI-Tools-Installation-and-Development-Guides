# OpenClaw Troubleshooting Guide

Common problems and solutions for beginner OpenClaw users.

## Installation Issues

### "Command not found: openclaw"

**Cause:** Node.js/npm didn't install correctly, or you're using the wrong terminal.

**Solution:**
1. Verify you're in the **correct terminal:**
   - **WSL**: Open Ubuntu terminal (search for "Ubuntu" in Start menu)
   - **Native Windows**: Open PowerShell or Windows Terminal as Administrator
2. Verify Node.js is installed:
   ```
   node --version
   npm --version
   ```
   Both should show version numbers (e.g., `v26.0.0`).
3. If `node` isn't found, install Node.js:
   - Visit [nodejs.org](https://nodejs.org/)
   - Download LTS version 26
   - Run the installer, check "Add to PATH"
   - **Close and reopen your terminal**
4. Reinstall OpenClaw:
   ```
   npm install -g openclaw@latest --allow-scripts=openclaw
   ```

### "npm: Permission denied" or "npm ERR! EACCES"

**Cause:** npm permissions are misconfigured.

**Solution:**
- **On Windows**: Run your terminal as Administrator
- **On WSL/Linux**: Reinstall Node.js with proper permissions:
  ```
  curl -fsSL https://deb.nodesource.com/setup_26.x | sudo -E bash - && sudo apt install -y nodejs
  ```

### NPM installation takes too long or times out

**Cause:** Slow internet or npm registry issues.

**Solution:**
1. Make sure your internet is stable
2. Try using a different npm mirror:
   ```
   npm config set registry https://registry.npmjs.org/
   ```
3. Clear npm cache:
   ```
   npm cache clean --force
   ```
4. Retry installation:
   ```
   npm install -g openclaw@latest --allow-scripts=openclaw
   ```

## Authentication Issues

### "API key not found" or "Authentication failed"

**Cause:** You haven't authenticated with an AI provider yet, or the key is invalid.

**Solution:**
1. Run the onboarding wizard:
   ```
   openclaw onboard
   ```
2. When prompted, select your provider (Claude, GPT, Gemini, etc.)
3. Let the browser open and get a fresh API key from the provider's dashboard
4. Paste the key into the terminal when asked
5. Press Enter and complete onboarding

### "Invalid API key" or "Key rejected by provider"

**Cause:** Your API key is incorrect, expired, or you've used up your quota.

**Solution:**
1. Go to your provider's dashboard:
   - **Claude/Anthropic**: [console.anthropic.com](https://console.anthropic.com/)
   - **OpenAI/ChatGPT**: [platform.openai.com](https://platform.openai.com/)
   - **Google/Gemini**: [aistudio.google.com](https://aistudio.google.com/)
2. Create a new API key (or verify the old one is still active)
3. Re-run onboarding with the new key:
   ```
   openclaw onboard --reset-auth
   ```
4. Paste the fresh key

### "Rate limited" or "Quota exceeded"

**Cause:** You've sent too many requests, or your account quota is full.

**Solution:**
1. **Wait a while** — Rate limits reset after a few minutes/hours (depends on provider)
2. **Check your account** at the provider's dashboard for quota information
3. **Upgrade your plan** if you've hit a usage limit
4. **Switch to a cheaper model:**
   ```
   openclaw model set anthropic/claude-3-5-haiku
   ```
   (Haiku is faster and cheaper than Sonnet)

## Gateway / Startup Issues

### "Gateway won't start" or keeps crashing immediately

**Cause:** Configuration error, port already in use, or missing dependencies.

**Solution:**
1. Check logs for details:
   ```
   openclaw logs
   ```
2. Look for error messages. Common ones:
   - **"Port already in use"**: Another process is using port 18789. Restart or change the port.
   - **"Configuration error"**: Your config file is corrupted. Reset it:
     ```
     openclaw onboard --reset-config
     ```
3. Try stopping and starting:
   ```
   openclaw stop
   openclaw start
   ```
4. If still broken, re-run full onboarding:
   ```
   openclaw onboard --install-daemon
   ```

### "Port 18789 already in use"

**Cause:** Another application is using that port, or OpenClaw is running twice.

**Solution:**

**On Windows (PowerShell):**
```powershell
Get-NetTCPConnection -LocalPort 18789 | Stop-Process -Force
openclaw start
```

**On WSL/Linux:**
```
lsof -i :18789
kill -9 [PID from output]
openclaw start
```

Or change the port in `~/.openclaw/openclaw.json`:

```json
{
  "gateway": {
    "port": 18790
  }
}
```

Then access it at: `http://127.0.0.1:18790/`

### "Gateway is running but dashboard won't open"

**Cause:** Firewall is blocking, or incorrect URL.

**Solution:**
1. Verify Gateway is actually running:
   ```
   openclaw status
   ```
   Should say `Gateway is running`.
2. Try the correct URL: `http://127.0.0.1:18789/` (not `https://` and not a typo)
3. Try a different browser (Chrome, Firefox, etc.)
4. **On Windows:** Check Windows Firewall:
   - Press `Win + S`, search **"Firewall"**
   - Click **Windows Defender Firewall > Advanced Settings**
   - Click **Inbound Rules > New Rule**
   - Select **Port**, choose **TCP**, enter `18789`
   - Allow the connection and name it "OpenClaw"
5. Restart Gateway:
   ```
   openclaw stop
   openclaw start
   ```

## Model / Response Issues

### "Model not available" or "Model not found"

**Cause:** The model name is misspelled or you don't have access to it.

**Solution:**
1. See which models you can use:
   ```
   openclaw models list
   ```
2. Set a model from the list:
   ```
   openclaw model set anthropic/claude-3-5-sonnet
   ```
3. If the model still isn't available, check your provider account. You might not have access.

### "Model response is very slow"

**Cause:** You're using a large/slow model, your internet is slow, or your computer is overloaded.

**Solution:**
1. Try a faster model:
   ```
   openclaw model set anthropic/claude-3-5-haiku
   ```
   (Haiku is the fastest Claude, GPT-4o-mini is fast for OpenAI)
2. Close other heavy applications
3. **On Windows (native)**: Switch to [WSL](./windows-wsl.md) for better performance
4. Check your internet speed: [speedtest.net](https://speedtest.net/)

### "OpenClaw gives wrong answers" or "Doesn't understand code"

**Cause:** You're using a weaker model, or the agent doesn't have enough context.

**Solution:**
1. Try a more powerful model:
   ```
   openclaw model set anthropic/claude-opus
   ```
   or
   ```
   openclaw model set openai/gpt-4-turbo
   ```
2. Ask more clearly with full context
3. Use the web dashboard to see what information the agent has

## Channel Setup Issues

### "Telegram bot won't respond"

**Cause:** Bot token is wrong, or OpenClaw isn't connected to the Telegram channel.

**Solution:**
1. Verify the bot token in your config:
   ```
   cat ~/.openclaw/openclaw.json
   ```
   Look for `"telegram"` and check the token.
2. Get a fresh token from @BotFather:
   - Open Telegram
   - Search **@BotFather**
   - Type `/mybots` to see your bots
   - Select your bot, click **API Token**
   - Copy the token
3. Update OpenClaw config:
   - Open the web dashboard: `http://127.0.0.1:18789/`
   - Go to **Channels > Telegram**
   - Paste the new token and save
4. Restart Gateway:
   ```
   openclaw restart
   ```
5. Send a test message to your bot on Telegram

### "Discord bot won't respond"

**Cause:** Bot token is wrong, or the bot doesn't have permissions.

**Solution:**
1. Check your bot has the right permissions:
   - Go to [Discord Developer Portal](https://discord.com/developers/applications)
   - Select your application
   - Go to **Bot**
   - Check that **Message Content Intent** is enabled
2. Get a fresh bot token:
   - Click **Regenerate** under TOKEN
   - Copy it
3. Update OpenClaw:
   - Open dashboard: `http://127.0.0.1:18789/`
   - Go to **Channels > Discord**
   - Paste the token and save
4. Restart Gateway:
   ```
   openclaw restart
   ```

### "WhatsApp not connected" or "Channel setup is complicated"

**Cause:** WhatsApp/Twilio setup is more complex than other channels.

**Solution:**
1. **Start with Telegram instead** — it's much easier
2. Once Telegram works, try other channels
3. See full WhatsApp setup at [docs.openclaw.ai/channels/whatsapp](https://docs.openclaw.ai/channels/whatsapp)

## WSL-Specific Issues

### "WSL won't install" (Windows message)

**Cause:** Virtualization is disabled, or Windows isn't up to date.

**Solution:**
1. Update Windows completely:
   - Press `Win + I` to open Settings
   - Go to **Update & Security > Check for updates**
   - Install all updates and restart
2. Enable Virtualization in BIOS (if still failing):
   - Restart your computer
   - Press **F2, F10, Del, or Esc** during startup (depends on your PC)
   - Find **Virtualization** or **VT-x** setting
   - Enable it and save
   - Restart
3. Try installing WSL again:
   ```powershell
   wsl --install
   ```

### "WSL runs very slowly"

**Cause:** You're working on the Windows file system (`/mnt/c/`), which is slow.

**Solution:**
1. Work in the Ubuntu home directory instead:
   ```
   cd ~
   ```
2. Create a projects folder there:
   ```
   mkdir ~/projects
   cd ~/projects
   ```
3. Use Ubuntu directories for OpenClaw work, not `/mnt/c/Users/...`

### "WSL terminal won't copy/paste"

**Cause:** Copy/paste doesn't work the same in WSL as Linux.

**Solution:**
1. Use **right-click** to paste in Ubuntu terminal
2. Or use **Ctrl + Shift + V** in Windows Terminal
3. Install `xclip` if you need clipboard integration:
   ```
   sudo apt install xclip
   ```

## Performance Issues

### "Everything is running slowly"

**Cause:** Disk I/O, CPU, or memory issue.

**Solution:**
1. Check what's using resources:
   - **Windows**: Open Task Manager (`Ctrl + Shift + Esc`)
   - **WSL/Linux**: Run `top` or `htop`
2. Close heavy applications (Chrome, Discord, Visual Studio, etc.)
3. Restart your computer
4. **On native Windows:** Switch to [WSL](./windows-wsl.md) for better performance
5. Upgrade to a faster/cheaper model

## Getting Help

**Before asking for help:**

1. **Collect logs:**
   ```
   openclaw logs
   ```
2. **Check your config:**
   ```
   cat ~/.openclaw/openclaw.json
   ```
3. **Verify basics:**
   - Node.js is installed: `node --version`
   - Gateway is running: `openclaw status`
   - Dashboard loads: `http://127.0.0.1:18789/`

**Get help at:**

- **Official Docs**: [docs.openclaw.ai/help](https://docs.openclaw.ai/help)
- **GitHub Issues**: [github.com/openclaw/openclaw/issues](https://github.com/openclaw/openclaw/issues)
- **Discord Community**: [discord.com/invite/clawd](https://discord.com/invite/clawd)

## Back to OpenClaw

- [← Return to Overview](../README.md)
- [→ Configuration Guide](../configuration/basic-setup.md)
