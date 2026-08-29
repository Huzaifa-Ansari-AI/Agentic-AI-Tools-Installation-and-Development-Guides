# OpenCode Troubleshooting

Common issues and solutions for OpenCode.

## Installation Issues

### Problem: "Command not found: opencode"

OpenCode is not installed or not in your PATH.

**Solution:**

1. Verify installation completed:

```bash
opencode --version
```

2. If not found, reinstall using your package manager:

**WSL/Linux:**
```bash
curl -fsSL https://opencode.ai/install | bash
```

**Windows (Scoop):**
```powershell
scoop install opencode
```

**npm:**
```bash
npm install -g opencode-ai@latest
```

3. Close and reopen your terminal
4. Try again: `opencode --version`

---

### Problem: Permission Denied (Windows)

Installation failed due to permissions.

**Solution:**

1. Open PowerShell **as Administrator**
2. Try installation again:

```powershell
scoop install opencode
```

Or use npm:
```powershell
npm install -g opencode-ai
```

---

### Problem: Download Fails / Network Error

**Solution:**

1. Check your internet connection
2. Try again — the install script may retry automatically
3. If persistent, try npm installation:

```bash
npm install -g opencode-ai@latest
```

---

## Authentication Issues

### Problem: "API key not found" or "No provider configured"

OpenCode can't find your API credentials.

**Solution:**

1. Run the connect command:

```
/connect
```

2. Select your provider (OpenCode Zen recommended for beginners)
3. Follow the browser link to get your API key
4. Paste the key into OpenCode when prompted
5. Try your command again

---

### Problem: "Invalid API key" or "Authentication failed"

Your API key is rejected by the provider.

**Solution:**

1. Verify the key is correct:
   - Log into your provider's dashboard
   - Generate a new API key
   - Copy carefully (avoid extra spaces)

2. In OpenCode, run:

```
/connect
```

3. Re-enter your API key
4. Try again

**For WSL users:** Ensure you're using the correct provider. For example, if you're using Anthropic, your API key must be from `console.anthropic.com`, not another service.

---

### Problem: "Rate limited" or "Quota exceeded"

You've hit usage limits on your provider account.

**Solution:**

1. Check your provider's dashboard for:
   - Remaining API credits
   - Rate limits and usage
   - Billing status

2. For free tiers:
   - Upgrade to a paid plan
   - Or wait for the limit to reset

3. For OpenCode Zen:
   - Check your account at `opencode.ai/auth`
   - Add credits if needed

---

## Model & Provider Issues

### Problem: "Model not available" or "ProviderModelNotFoundError"

Model name is incorrect or you don't have access.

**Solution:**

1. Check available models:

```
/models
```

2. Verify model name format: `provider/model-name`

Example correct names:
- `anthropic/claude-sonnet-4-5`
- `openai/gpt-4-turbo`
- `google/gemini-2-flash`

3. If using a custom config, use the correct model ID from `/models`

---

### Problem: Model Response is Slow

Processing is taking longer than expected.

**Solution:**

1. **Use a smaller/faster model:**

```
/models
```

Select a smaller model like:
- Claude 3.5 Haiku
- GPT-4 Mini

2. **Check your internet connection** — slow connection = slow responses

3. **Try a different provider** — some providers are faster than others

4. **For WSL users:** Ensure you're not working on Windows files (`/mnt/c/`) — clone your repo to WSL instead:

```bash
cd ~
git clone <your-repo>
cd <your-repo>
opencode
```

---

### Problem: ProviderInitError

Configuration is invalid or corrupted.

**Solution:**

1. Clear your auth data:

**Linux/macOS:**
```bash
rm -rf ~/.local/share/opencode
```

**Windows:**
Press `WIN+R` and delete: `%USERPROFILE%\.local\share\opencode`

2. Run `/connect` again and re-enter your credentials

3. Run `/models` to verify it works

---

## Runtime & Execution Issues

### Problem: OpenCode Won't Start or Crashes

**Solution:**

1. Check logs for error details:

**Linux/macOS:**
```bash
cat ~/.local/share/opencode/log/*.log
```

**Windows:**
Check: `%USERPROFILE%\.local\share\opencode\log`

2. Try starting with verbose output:

```bash
opencode --log-level DEBUG
```

3. Clear the cache:

**Linux/macOS:**
```bash
rm -rf ~/.cache/opencode
```

**Windows:**
Delete: `%USERPROFILE%\.cache\opencode`

4. Reinstall OpenCode:

```bash
npm install -g opencode-ai@latest
```

Or use your package manager.

---

### Problem: "Cannot read files" or Permission Errors

OpenCode can't access your project files.

**Solution:**

1. Ensure your project directory is readable:

```bash
ls -la /path/to/project
```

2. If using a subdirectory:

```bash
cd /path/to/project
opencode
```

3. Ensure Git is initialized in your project:

```bash
git init
```

4. **For WSL users:** Ensure you're working in WSL, not on Windows drives if possible:

```bash
# Slow (Windows file system accessed through WSL)
cd /mnt/c/Users/YourName/project

# Fast (WSL native file system)
cd ~/project
```

---

## Configuration Issues

### Problem: Config File Not Found or Ignored

OpenCode doesn't use your `opencode.json`.

**Solution:**

1. Ensure file is in the right location:
   - **Global:** `~/.config/opencode/opencode.json`
   - **Project:** `opencode.json` in project root

2. Verify JSON syntax:

```bash
cat ~/.config/opencode/opencode.json | jq
```

If there's a syntax error, `jq` will show it.

3. For project config, ensure you're in the project root:

```bash
ls -la opencode.json
```

---

### Problem: "Config Schema Error"

Your configuration doesn't match the expected schema.

**Solution:**

1. Validate your `opencode.json` with the schema:

Reference: [opencode.ai/config.json](https://opencode.ai/config.json)

2. Common mistakes:
   - Using wrong quotes (use double quotes, not single)
   - Missing commas between fields
   - Trailing commas in arrays/objects

3. **Example valid config:**

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "autoupdate": true
}
```

---

## Platform-Specific Issues

### Windows Native Issues

**Problem: Slow Performance**

**Solution:**
- Use [WSL instead](../installation/windows-wsl.md) for better performance

**Problem: Copy/Paste Not Working**

**Solution:**
- This is a limitation of some terminals on Windows
- Use Windows Terminal instead of Command Prompt
- Upgrade to the latest version of Windows Terminal

---

### WSL/Linux Issues

**Problem: "Copy/paste not working"**

**Solution:**

Install clipboard utilities:

```bash
sudo apt update
sudo apt install -y xclip
```

Then restart OpenCode.

---

**Problem: "Connection refused" or port errors**

**Solution:**

1. Ensure WSL is running:

```powershell
wsl --status
```

2. Restart WSL:

```powershell
wsl --terminate Ubuntu
wsl
```

---

## Getting More Help

If your issue isn't listed here:

1. **Check the logs:**

**Linux/macOS:**
```bash
tail -f ~/.local/share/opencode/log/*.log
```

**Windows:**
Check the latest file in: `%USERPROFILE%\.local\share\opencode\log`

2. **Report on GitHub:**
   - [github.com/anomalyco/opencode/issues](https://github.com/anomalyco/opencode/issues)
   - Search existing issues first
   - Provide your OpenCode version and OS

3. **Join the Discord:**
   - [opencode.ai/discord](https://opencode.ai/discord)
   - Ask in the #help channel

4. **Read official troubleshooting:**
   - [opencode.ai/docs/troubleshooting](https://opencode.ai/docs/troubleshooting)

---

**Back to OpenCode Guide** — [← OpenCode README](../README.md)
