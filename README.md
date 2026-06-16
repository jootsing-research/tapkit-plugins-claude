[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/jootsing-research-tapkit-plugins-claude-badge.png)](https://mseep.ai/app/jootsing-research-tapkit-plugins-claude)

# TapKit Plugin Marketplace for Claude

Control a physical iPhone from Claude. Take screenshots, tap, swipe, type, and navigate apps — all through natural language.

## Installation

You'll need the [TapKit Mac app](https://tapkit.ai) running with a connected iPhone (Switch Control enabled). Then follow the docs for whichever Claude you're using:

- **Claude Code (CLI):** [docs.tapkit.ai/integrations/claude-code](https://docs.tapkit.ai/integrations/claude-code)
- **Claude desktop app:** [docs.tapkit.ai/integrations/claude-app](https://docs.tapkit.ai/integrations/claude-app)

## What You Get

The **tapkit** plugin connects Claude to a real iPhone via [TapKit](https://tapkit.ai). Once installed, you can tell Claude things like:

- "Take a screenshot of my phone"
- "Open Settings and turn on Do Not Disturb"
- "Order my usual from Uber Eats"
- "Check the weather in Tokyo"
- "Send a message on Telegram"

The plugin ships with the [TapKit MCP server](https://github.com/Jootsing-Research/tapkit-mcp) (gestures, screenshots, app control) plus a set of [skills](plugins/tapkit/skills) that teach Claude how to navigate specific apps.

## How It Works

```
Claude  →  TapKit MCP Server  →  TapKit API  →  TapKit Mac App  →  iPhone
 (you)      (mcp.tapkit.ai)       (cloud)        (your Mac)      (your device)
```

Claude sees the phone screen through screenshots and interacts through tap/swipe/type commands. The core loop is always: **screenshot → look → act → screenshot to verify**.

## Requirements

- [Claude Code](https://claude.ai/code) or the Claude desktop app
- A TapKit account at [tapkit.ai](https://tapkit.ai)
- The TapKit Mac app and a connected iPhone

## License

MIT
