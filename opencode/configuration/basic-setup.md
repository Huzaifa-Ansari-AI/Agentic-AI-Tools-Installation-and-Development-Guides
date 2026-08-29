# OpenCode Basic Setup

After installing OpenCode, configure it for your first project.

## Initial Setup

### Step 1 — Run `/connect`

In the OpenCode TUI, run:

```
/connect
```

This command lets you add or change your LLM provider credentials.

**For beginners:** Choose **OpenCode Zen** — a curated list of tested models provided by the OpenCode team.

### Step 2 — Select Your Provider

OpenCode shows available providers. Navigate with arrow keys and select one:

```
Available Providers:
  ▸ OpenCode Zen
  ▸ Anthropic (Claude)
  ▸ OpenAI (ChatGPT)
  ▸ Google (Gemini)
  ▸ ... and more
```

**Recommended first choices:**
- **OpenCode Zen** — Simple, curated, good for learning
- **Anthropic** — Excellent coding models (Claude Sonnet)
- **OpenAI** — Popular ChatGPT models

### Step 3 — Get Your API Key

After selecting a provider, OpenCode displays a link:

```
Visit: https://opencode.ai/auth
Sign in, add your billing details, and copy your API key
```

1. Open the link in your browser
2. Create an account (if needed)
3. Add billing information
4. Generate an API key
5. Copy the key

### Step 4 — Paste Your API Key

Back in OpenCode, paste the key when prompted:

```
┌ API key
│ sk-...
└ (press Enter)
```

If successful, you'll see:
```
✓ Connected to [Provider Name]
```

## Authentication Storage

Your API keys are stored securely:

- **macOS/Linux:** `~/.local/share/opencode/auth.json`
- **Windows:** `%USERPROFILE%\.local\share\opencode\auth.json`

Keys are stored locally and never sent to OpenCode's servers.

## Select Your AI Model

Run this command in OpenCode:

```
/models
```

You'll see available models from your provider.

### Model Recommendations

**For General Coding:**
- Claude 3.5 Sonnet
- GPT-4
- Qwen Coder

**Fast & Affordable:**
- Claude 3.5 Haiku
- GPT-4 Mini
- Grok Beta

**Maximum Capability:**
- Claude Opus
- GPT-4 Turbo

Use arrow keys to select and press Enter.

## Initialize Your Project

Run this command:

```
/init
```

OpenCode analyzes your project and creates `AGENTS.md` in your project root. This file:
- Contains project analysis
- Helps OpenCode understand your structure
- Should be committed to Git

**Commit the file:**

```bash
git add AGENTS.md
git commit -m "chore: add OpenCode project configuration"
```

## Configuration Files

### Global Config

OpenCode reads a global config from:

- **macOS/Linux:** `~/.config/opencode/opencode.json`
- **Windows:** `%USERPROFILE%\.config\opencode\opencode.json`

### Project Config

You can add `opencode.json` in your project root for project-specific settings:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5"
}
```

This overrides global settings for the current project.

## Essential Environment Variables

### For Anthropic (Claude)

If not using `/connect`:

```bash
export ANTHROPIC_API_KEY="your-key-here"
```

### For OpenAI

```bash
export OPENAI_API_KEY="your-key-here"
```

### For Google Gemini

```bash
export GOOGLE_API_KEY="your-key-here"
```

On Windows PowerShell:

```powershell
$env:ANTHROPIC_API_KEY="your-key-here"
```

## Verify Your Setup

Test that everything works:

1. Open a project directory
2. Start OpenCode: `opencode`
3. Try a simple request:

```
Explain what this project does in one sentence
```

If OpenCode responds with an analysis, your setup is complete!

## Switch Between Models

You can change models anytime:

```
/models
```

This is useful for:
- Using cheaper models for simple tasks
- Trying different models for better results
- Testing new models as they're released

## Switch Providers

To add a new provider or change your current one:

```
/connect
```

You can have multiple providers configured and switch between them at any time.

## Common Configurations

### Use a Different Default Model

Edit `~/.config/opencode/opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5"
}
```

Available model format: `provider/model-name`

Example models:
- `anthropic/claude-sonnet-4-5`
- `openai/gpt-4-turbo`
- `google/gemini-2-flash`

### Use Multiple Providers

If you've set up multiple providers with `/connect`, you can use any in your commands.

Run `/models` to see all available models across all providers.

## Advanced Configuration

For more detailed configuration options (themes, keybinds, formatters, etc.), see the [official config documentation](https://opencode.ai/docs/config).

## Troubleshooting Configuration

### "API key not found"

**Solution:**
1. Run `/connect` to re-enter your API key
2. Ensure your API key is valid in the provider's dashboard
3. Check that the provider is active and has billing enabled

---

### "Model not available"

**Cause:** Model doesn't exist or you don't have access to it.

**Solution:**
1. Run `/models` to see available models
2. Verify model name format: `provider/model-name`
3. Check your provider's website for available models

---

### "Config file errors"

**Solution:**
1. Ensure your `opencode.json` uses valid JSON syntax
2. Use this schema reference: [opencode.ai/config.json](https://opencode.ai/config.json)
3. Most editors support schema validation (VS Code will show errors)

---

## Next Steps

✅ Basic setup is complete

**What's next?**

1. **Learn Common Issues** — [Troubleshooting Guide](../troubleshooting/common-issues.md)
2. **Explore Demos** — [Demos](../demos/README.md)
3. **Read Official Docs** — [opencode.ai/docs](https://opencode.ai/docs)

---

**Back to OpenCode Guide** — [← OpenCode README](../README.md)
