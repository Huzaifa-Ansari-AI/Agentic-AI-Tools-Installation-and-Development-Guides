# OpenCode on Native Windows

## Overview

OpenCode can run directly on Windows without WSL. However, the OpenCode team recommends WSL for the best experience.

This guide covers native Windows installation if you prefer to avoid WSL or your system doesn't support it.

## Important Note

**Windows Support Status:** OpenCode officially supports Windows, but WSL provides better performance and fuller feature support. If possible, use [Windows + WSL](./windows-wsl.md) instead.

## Prerequisites

**System Requirements:**
- Windows 10 or later
- Administrator access (for some installation methods)
- At least 1GB free disk space
- PowerShell or Command Prompt
- API key from an LLM provider

## Installation Methods

Choose one installation method:

### Method 1 — Scoop (Recommended for Windows)

Scoop is a package manager for Windows that handles installation and updates.

**Install Scoop** (if not already installed):

Open PowerShell and run:

```powershell
iwr -useb get.scoop.sh | iex
```

**Install OpenCode:**

```powershell
scoop install opencode
```

Verify:

```powershell
opencode --version
```

### Method 2 — Chocolatey

If you use Chocolatey:

```powershell
choco install opencode
```

Verify:

```powershell
opencode --version
```

### Method 3 — NPM (Requires Node.js)

If you have Node.js installed:

```powershell
npm install -g opencode-ai@latest
```

Verify:

```powershell
opencode --version
```

### Method 4 — GitHub Releases (Manual)

1. Go to [github.com/anomalyco/opencode/releases](https://github.com/anomalyco/opencode/releases)
2. Download the Windows executable
3. Add the executable's folder to your system PATH
4. Restart PowerShell
5. Verify: `opencode --version`

## Launch OpenCode

Open PowerShell or Command Prompt and navigate to your project:

```powershell
cd C:\path\to\your\project
```

Start OpenCode:

```powershell
opencode
```

## Initial Setup

### Connect to an AI Provider

Run this command in the OpenCode terminal:

```
/connect
```

Select your provider (OpenCode Zen recommended for beginners) and enter your API key.

### Select a Model

```
/models
```

Choose your AI model and press Enter.

### Initialize Your Project

```
/init
```

This creates an `AGENTS.md` file for your project.

## Performance Considerations

**Known Limitations:**
- Slightly slower performance compared to WSL
- Some terminal features may work differently
- File watching may be less reliable

**Recommendation:** If you experience performance issues, consider switching to [WSL](./windows-wsl.md).

## Troubleshooting

### **"Command not found: opencode"**

**Solution:**
1. Ensure OpenCode installation completed successfully
2. Restart PowerShell
3. Check that the installation folder is in your PATH

---

### **"Permission denied" or installation failures**

**Solution:**
- Run PowerShell as Administrator
- Try a different installation method (Scoop, Chocolatey, or NPM)

---

### **Poor performance or file access issues**

**Solution:**
- Switch to [Windows + WSL](./windows-wsl.md) for better performance
- Ensure your project is on a local drive (not network share)

---

## Upgrading OpenCode

Depending on your installation method:

**Scoop:**
```powershell
scoop update opencode
```

**Chocolatey:**
```powershell
choco upgrade opencode
```

**NPM:**
```powershell
npm install -g opencode-ai@latest
```

---

## Next Steps

✅ OpenCode is installed on Windows

**What's next?**

1. **Learn Configuration** — [Basic Setup Guide](../configuration/basic-setup.md)
2. **Solve Problems** — [Troubleshooting Guide](../troubleshooting/common-issues.md)
3. **Consider WSL** — [Windows + WSL Installation](./windows-wsl.md) for better experience
4. **Read Official Docs** — [opencode.ai/docs](https://opencode.ai/docs)

---

**Back to OpenCode Guide** — [← OpenCode README](../README.md)
