# OpenClaw Demonstrations & Examples

This directory is reserved for **practical, tested, real-world OpenClaw workflows**.

## What Belongs Here

Demonstrations that show:

✓ **Real problems solved** — Actual workflows that do something useful  
✓ **Working code** — Examples that run without modification  
✓ **Full documentation** — Clear steps anyone can follow  
✓ **Verified results** — Tested and confirmed to work as described

## What Does NOT Belong Here

✗ Placeholder or template examples  
✗ Broken or untested code  
✗ Copied tutorials from elsewhere  
✗ Marketing or conceptual material  
✗ Incomplete workflows  

## Demo Structure

When submitting a demo, use this organization:

```
demos/
├── [demo-name]/
│   ├── README.md           # What this demo does and how to run it
│   ├── openclaw.json       # Config file (if custom setup needed)
│   ├── script.sh           # or .ps1 or .py — the actual workflow
│   └── RESULTS.md          # What to expect as output
```

## Example Demo: Email Cleanup

**Directory:** `demos/email-cleanup/`

**README.md includes:**
- One sentence: "Automatically unsubscribe from bulk emails"
- Prerequisites (e.g., Gmail account, OpenClaw with email integration)
- 5 clear steps to run it
- What the output looks like
- Expected run time

**script.sh** (or script.ps1):
- Actual commands that work
- Commented to explain each step
- No placeholders or `[INSERT X HERE]` markers

**RESULTS.md:**
- Example output showing success
- Screenshots (optional but helpful)
- Common issues and how to fix them

## Quality Requirements

**Practical:**
- Solves a real problem
- Not a toy example
- Useful for more than one person

**Documented:**
- Anyone can understand what it does
- Written for beginners
- Includes "why" not just "how"

**Reproducible:**
- Takes 5–30 minutes to run
- Minimal setup required
- Works on Windows, WSL, or both

**Verified:**
- Actually tested by the author
- Works as described
- Error messages explained

## Suggested Demo Ideas

**Beginner Level:**
- Summarize emails from a specific sender
- Daily task reminder from a calendar
- Auto-respond to certain message types
- Organize incoming emails by topic

**Intermediate Level:**
- Monitor GitHub issues and create summaries
- Auto-draft responses to common questions
- Generate weekly reports from Slack messages
- Backup important documents automatically

**Advanced Level:**
- Multi-agent coordination across projects
- Real-time market data analysis workflow
- Automated code review and testing
- Cross-channel task synchronization

## How to Create a Demo

1. **Build your workflow** in OpenClaw using the terminal or web dashboard
2. **Test it thoroughly** — run it 3+ times to ensure it's stable
3. **Document every step:**
   - Write the README first (describe what you did)
   - Include exact commands (copy/paste ready)
   - Add a screenshot of the output
4. **Save your configuration:**
   - Export your `~/.openclaw/openclaw.json` (remove API keys)
   - Save any custom scripts
5. **Create the demo folder** with all files
6. **Test again from scratch** — follow your own README and make sure it works

## Before You Submit

- ✓ Have you run this demo 3+ times successfully?
- ✓ Can someone else follow your README without guessing?
- ✓ Are all commands copy-paste ready (no placeholders)?
- ✓ Did you include expected output or a screenshot?
- ✓ Is the demo useful beyond just showing off features?

## Sharing Your Demo

Once your demo is ready:

1. Test the instructions one final time
2. Add your demo folder to this directory
3. Update the list below with a brief description
4. Commit and push to GitHub

## Current Demos

*(Currently empty — waiting for community contributions)*

[Demos will be listed here as they're added]

## Resources

- **Full OpenClaw Docs**: [docs.openclaw.ai](https://docs.openclaw.ai/)
- **Channel Integration**: [docs.openclaw.ai/channels](https://docs.openclaw.ai/channels/)
- **Skills & Automation**: [docs.openclaw.ai/skills](https://docs.openclaw.ai/skills)
- **Community Discord**: [discord.com/invite/clawd](https://discord.com/invite/clawd)

## Back to OpenClaw

- [← Return to Overview](../README.md)
- [← Troubleshooting](../troubleshooting/common-issues.md)
