# OpenCode on Windows Using WSL

## Overview

Windows Subsystem for Linux (WSL) provides the best experience for running OpenCode on Windows. It delivers better file system performance, full terminal support, and compatibility with all OpenCode features that may be limited on native Windows.

This guide walks you through installing and setting up OpenCode in WSL on Windows.

## What is WSL?

Windows Subsystem for Linux (WSL) is a compatibility layer that lets you run a Linux environment directly on Windows without a virtual machine. This means:

- **Better Performance** — Direct file system access without virtualization overhead
- **Native Linux Tools** — Full Linux terminal environment with familiar commands
- **Seamless Access** — Access Windows files from Linux and vice versa
- **No Dual Boot** — Linux runs alongside Windows, always available

**Why use WSL for OpenCode?** The official OpenCode documentation recommends WSL for Windows users because it provides full compatibility with all features.

## Prerequisites

**System Requirements:**
- Windows 10 (Build 2004+) or Windows 11
- Administrator access to your computer
- At least 2GB free disk space
- A modern terminal emulator (PowerShell, Terminal, or similar)

**What You'll Need:**
- OpenCode API key from an LLM provider (Anthropic, OpenAI, etc.)
- Basic command-line familiarity

## Step 1 — Install WSL

### Check if WSL is Already Installed

Open PowerShell and check your WSL version:

```powershell
wsl --version
```

If you see a version number, WSL is already installed. Skip to **Step 2**.

If the command is not recognized, follow the installation steps below.

### Install WSL 2

Open PowerShell **as Administrator** and run:

```powershell
wsl --install
```

This command:
- Installs WSL 2 (the latest version)
- Installs Ubuntu as the default Linux distribution
- Enables required Windows features automatically

After the installation completes, restart your computer when prompted.

## Step 2 — Verify WSL and Ubuntu

After restarting, open PowerShell and verify the installation:

```powershell
wsl --list --verbose
```

You should see Ubuntu listed with version 2. Example output:

```
NAME      STATE           VERSION
Ubuntu    Running         2
```

### Update Ubuntu

Open WSL by typing in PowerShell:

```powershell
wsl
```

Inside the Ubuntu terminal, update the package manager:

```bash
sudo apt update && sudo apt upgrade -y
```

This ensures you have the latest packages before installing OpenCode.

Exit the WSL terminal:

```bash
exit
```

## Step 3 — Install OpenCode in WSL

Open PowerShell and start WSL:

```powershell
wsl
```

Now you're inside the Ubuntu terminal. Install OpenCode using the official script:

```bash
curl -fsSL https://opencode.ai/install | bash
```

The script will:
- Download the latest OpenCode binary
- Place it in your PATH (typically `~/.local/bin`)
- Make it immediately available as the `opencode` command

Verify the installation:

```bash
opencode --version
```

You should see the version number. Example:

```
OpenCode 1.18.25
```

## Step 4 — Launch OpenCode

Navigate to a project directory (or create one for testing):

```bash
cd /path/to/your/project
```

**Accessing Windows Files from WSL:**

Windows drives are accessible via `/mnt/`:
- `C:` drive → `/mnt/c/`
- `D:` drive → `/mnt/d/`
- etc.

Example — navigate to a project on your Windows desktop:

```bash
cd /mnt/c/Users/YourName/project
```

Start OpenCode:

```bash
opencode
```

You'll see the OpenCode terminal interface (TUI) start up. This is where you interact with the AI agent.

## Step 5 — Initial Setup & Authentication

When OpenCode starts, you need to authenticate with an AI provider.

### Connect to an AI Provider

In the OpenCode TUI, run:

```
/connect
```

OpenCode will show you a list of supported providers:
- **Anthropic** (Claude)
- **OpenAI** (ChatGPT Plus/Pro)
- **Google** (Gemini)
- **OpenCode Zen** (Recommended for beginners)
- And many more

**Recommended for Beginners:** Choose **OpenCode Zen** for a curated list of tested models.

### Get Your API Key

After selecting a provider:

1. OpenCode provides a link (e.g., `opencode.ai/auth` for Zen)
2. Open that link in your browser
3. Sign in and create an API key
4. Copy the API key and paste it into the OpenCode terminal

Example prompt in OpenCode:

```
┌ API key
│
└ enter
```

Paste your key and press Enter.

## Step 6 — Select Your AI Model

In the OpenCode TUI, run:

```
/models
```

OpenCode displays available models from your provider. Select one:

- **Recommended for Coding** — Claude 3.5 Sonnet or GPT-4
- **Fast & Cost-Effective** — Claude 3.5 Haiku or GPT-4 Mini
- **Most Capable** — Claude Opus or GPT-4 Turbo

Use arrow keys to select, then press Enter.

## Step 7 — Initialize Your Project

Still in the OpenCode TUI, initialize your current project:

```
/init
```

OpenCode analyzes your project structure and creates an `AGENTS.md` file in your project root. This helps OpenCode understand your codebase.

**Commit this file to Git:**

```bash
git add AGENTS.md
git commit -m "chore: add OpenCode project configuration"
```

## Step 8 — Verify Installation

Try your first OpenCode command:

```
Write a brief comment explaining the main purpose of the main.py file
```

OpenCode should:
1. Process your request
2. Scan the codebase
3. Provide an analysis or make changes

If you see a response, everything is working!

## Useful Commands

### Core Commands

- `/init` — Initialize OpenCode for your project
- `/models` — View and switch AI models
- `/connect` — Add or switch API providers
- `/undo` — Undo the last change
- `/redo` — Redo an undone change
- `/share` — Share your conversation

### Navigation

- `Tab` — Switch between **Build** mode (full access) and **Plan** mode (read-only analysis)
- Arrow Keys — Navigate menus and suggestions
- `Ctrl+C` — Exit OpenCode

## Common Problems

### **Problem: "Command not found: opencode"**

**Cause:** OpenCode is installed but not in your PATH.

**Solution:**
1. Ensure you ran the installation script completely
2. Try reopening your WSL terminal
3. If still not working, install manually:

```bash
npm install -g opencode-ai@latest
```

---

### **Problem: "API key rejected" or "Authentication failed"**

**Cause:** Invalid API key or provider configuration.

**Solution:**
1. Verify your API key is correct (copy directly from provider dashboard)
2. Ensure the provider is active and has billing set up
3. Run `/connect` again and re-enter your credentials

---

### **Problem: OpenCode is slow or files not updating**

**Cause:** Accessing Windows files through `/mnt/` can be slower than native Linux files.

**Solution:** Clone your repository directly into WSL:

```bash
cd ~
git clone https://github.com/yourname/yourrepo
cd yourrepo
opencode
```

Working within WSL improves performance significantly.

---

### **Problem: Copy/paste not working in OpenCode**

**Cause:** Missing clipboard utilities on Linux.

**Solution:** Install clipboard tools:

```bash
sudo apt install -y xclip
```

Then restart OpenCode.

---

## Desktop App + WSL Server (Optional)

If you prefer the OpenCode Desktop application but want to run the backend in WSL:

**In WSL terminal:**

```bash
opencode serve --hostname 0.0.0.0
```

**On Windows, in Desktop App:**
- Connect to: `http://localhost:4096`

This hybrid setup gives you a GUI on Windows while leveraging WSL's better performance for the backend.

---

## Next Steps

✅ OpenCode is now installed and authenticated

**What's next?**

1. **Learn Configuration** — [Basic Setup Guide](../configuration/basic-setup.md)
2. **Solve Problems** — [Troubleshooting Guide](../troubleshooting/common-issues.md)
3. **Read Official Docs** — [opencode.ai/docs](https://opencode.ai/docs)

---

**Back to OpenCode Guide** — [← OpenCode README](../README.md)
