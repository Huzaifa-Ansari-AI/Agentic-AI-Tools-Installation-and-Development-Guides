# OpenClaw Configuration Guide

After installing OpenClaw, you need to complete two essential tasks:

1. **Authenticate** with your AI provider (to get responses from Claude, GPT, Gemini, etc.)
2. **Configure a channel** (to message OpenClaw from Telegram, WhatsApp, Discord, etc.)

This guide covers both.

## Initial Configuration

Your OpenClaw configuration lives at:

```
~/.openclaw/openclaw.json
```

On Windows (WSL or native), this is typically:

```
C:\Users\YourName\.openclaw\openclaw.json
```

You don't need to manually edit this file. The `openclaw onboard` command creates and updates it for you.

## Authentication (API Keys)

OpenClaw needs an API key from your chosen AI provider. Think of it as the password that lets OpenClaw request responses from Claude, GPT, Gemini, etc.

### Step 1 — Run Onboarding

If you haven't already, run:

```
openclaw onboard
```

Or if you want to update your authentication:

```
openclaw onboard --reset-auth
```

### Step 2 — Select a Provider

The wizard shows available providers. Choose one:

**For beginners:**
- **Anthropic (Claude)** — Best overall quality, great reasoning
- **OpenAI (GPT)** — If you already have a ChatGPT subscription
- **Google (Gemini)** — Good alternative, often faster

**Other options:**
- DeepSeek, Groq, Grok, Local models (advanced)

### Step 3 — Get Your API Key

After selecting a provider, the wizard:

1. Opens your browser to the provider's dashboard
2. Asks you to create or copy an API key
3. Prompts you to paste it back into the terminal

**You won't see the text as you type** — this is normal and secure. Just paste and press Enter.

### Step 4 — Choose Your Model

After authentication, you'll see a list of available models:

```
? Which model would you like to use?
❯ gpt-4o
  gpt-4-turbo
  gpt-4
  gpt-4o-mini
```

**For first-time users:**
- **Claude 3.5 Sonnet** (Anthropic) — Excellent all-around performance
- **GPT-4o** (OpenAI) — Powerful, good for coding
- **Gemini 2.0** (Google) — Fast and capable

Select one and press Enter. You can change models later.

## Channel Configuration

After authentication, you're ready to add messaging channels. A channel is how you talk to OpenClaw—WhatsApp, Telegram, Discord, etc.

### Quick Start: Telegram (Fastest)

Telegram is the easiest channel to set up. Once your Gateway is running:

1. **On your phone**, open Telegram
2. **Search for** `@BotFather` (the official Telegram bot)
3. **Start a chat** and type `/newbot`
4. **Follow the prompts:**
   - Give your bot a name (e.g., "My OpenClaw")
   - Give it a unique username (e.g., "my_openclaw_bot")
5. **Copy the API token** that BotFather gives you (long string of numbers and letters)
6. **Open OpenClaw's web dashboard**: `http://127.0.0.1:18789/`
7. **Go to Channels or Settings**
8. **Find Telegram** and click **Connect**
9. **Paste the API token**
10. **Save and restart the Gateway**

After that:
1. **Search for your bot** on Telegram (using the username you created)
2. **Send a test message**
3. Your OpenClaw responds

### Other Channels

Setting up other channels follows the same pattern:

**Discord:**
- Create a bot in Discord Developer Portal
- Copy the bot token
- Add to your Discord server
- Paste token in OpenClaw settings

**WhatsApp:**
- Connect via Twilio or Meta's WhatsApp Business API
- Get credentials from the provider
- Add to OpenClaw settings

**Slack:**
- Create an app in Slack API Portal
- Get a bot token
- Add to OpenClaw settings

Full channel setup guides are at [docs.openclaw.ai/channels](https://docs.openclaw.ai/channels).

## Important Configuration Files

### Main Config: `~/.openclaw/openclaw.json`

This file contains all your settings. Example:

```json
{
  "modelId": "anthropic/claude-3-5-sonnet",
  "channels": {
    "telegram": {
      "botToken": "123456:ABC..."
    }
  },
  "gateway": {
    "port": 18789
  }
}
```

**Do not edit by hand unless you know what you're doing.** Use `openclaw onboard` instead.

### Auth Credentials: `~/.openclaw/auth.json`

Stores your API keys securely. OpenClaw manages this automatically.

**Never commit these files to Git.** They contain sensitive credentials.

### Environment Variables

You can also set API keys via environment variables. This is useful for:
- Running OpenClaw on a server
- Keeping secrets out of config files
- Switching credentials without editing files

**To set environment variables (Windows PowerShell):**

```powershell
$env:ANTHROPIC_API_KEY="sk-ant-..."
$env:OPENAI_API_KEY="sk-..."
```

**To set environment variables (WSL/Linux):**

```
export ANTHROPIC_API_KEY="sk-ant-..."
export OPENAI_API_KEY="sk-..."
```

Supported variables:
- `ANTHROPIC_API_KEY` — For Claude models
- `OPENAI_API_KEY` — For GPT models
- `GOOGLE_API_KEY` — For Gemini models
- `DEEPSEEK_API_KEY` — For DeepSeek models

## Common Configurations

### Change Your Model

To switch from GPT to Claude without re-running full onboarding:

```
openclaw model set anthropic/claude-3-5-sonnet
```

Or re-run onboarding:

```
openclaw onboard
```

### Use Multiple Providers

Store multiple API keys so you can switch between providers. OpenClaw supports this through environment variables or manual config editing.

### Limit Channel Access

Restrict who can message your OpenClaw. Edit `~/.openclaw/openclaw.json`:

```json
{
  "channels": {
    "telegram": {
      "allowFrom": ["+1234567890", "+9876543210"]
    }
  }
}
```

This allows only those phone numbers to message your bot.

### Require Mentions in Groups

If your bot is in a group chat, make it respond only to mentions:

```json
{
  "channels": {
    "discord": {
      "requireMention": true
    }
  },
  "messages": {
    "groupChat": {
      "mentionPatterns": ["@openclaw", "openclaw:"]
    }
  }
}
```

## Verification

To verify your setup is complete:

1. **Run this command:**
   ```
   openclaw status
   ```
   You should see `Gateway is running`.

2. **Open the dashboard:**
   ```
   http://127.0.0.1:18789/
   ```

3. **Send a test message:**
   - Type `"Hello"` or `"What is 2 + 2?"`
   - You should get a response within 5 seconds

4. **If using a channel** (Telegram, Discord, etc.):
   - Message your bot from that platform
   - It should respond

If all three work, you're configured and ready to use OpenClaw.

## Next Steps

1. **[Troubleshooting Guide](../troubleshooting/common-issues.md)** — If something doesn't work
2. **[OpenClaw Full Docs](https://docs.openclaw.ai/gateway/configuration)** — For advanced configuration
3. **Explore skills and integrations** — Extend OpenClaw with custom capabilities
4. **[Back to OpenClaw Overview](../README.md)**

## Quick Reference

| Task | Command |
|------|---------|
| Run setup wizard | `openclaw onboard` |
| Reset authentication | `openclaw onboard --reset-auth` |
| View status | `openclaw status` |
| Open dashboard | `openclaw dashboard` |
| Check logs | `openclaw logs` |
| Restart Gateway | `openclaw restart` |
| Stop Gateway | `openclaw stop` |
| Start Gateway | `openclaw start` |
| View config | `cat ~/.openclaw/openclaw.json` |
| Update OpenClaw | `npm update -g openclaw` |
