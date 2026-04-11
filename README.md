# BulkPublish AI Toolkit

Connect your AI assistant to [BulkPublish](https://bulkpublish.com) — schedule posts, manage channels, upload media, and track analytics across 11 social media platforms, all from your AI tool of choice.

## What You Get

**29 MCP tools** covering the full BulkPublish API:

| Category | Tools | Examples |
|---|---|---|
| **Posts** | 10 | Create, schedule, publish, retry, delete, bulk actions, metrics |
| **Channels** | 4 | List accounts, health check, platform options, @mention search |
| **Media** | 4 | Upload (URL), list, get, delete |
| **Labels** | 4 | Create, list, update, delete |
| **Schedules** | 4 | Create recurring schedules, update, list, delete |
| **Analytics** | 1 | Summary by date range with per-platform breakdown |
| **Quota** | 1 | Plan usage and limits |
| **Queue** | 1 | Next optimal posting slot |

**Supported platforms:** Facebook, Instagram, X/Twitter, TikTok, YouTube, Threads, Bluesky, Pinterest, Google Business Profile, LinkedIn, Mastodon

## Prerequisites

- **Node.js 18+** (for running the MCP server via `npx`)
- **BulkPublish API key** — get one at [app.bulkpublish.com/developer](https://app.bulkpublish.com/developer)

---

## Quick Install

Three ways to get started — pick what fits your workflow:

```bash
# 1. Claude Code Plugin (recommended — auto-updates, includes skills)
/plugin marketplace add azeemkafridi/bulkpublish-ai-toolkit
/plugin install bulkpublish@bulkpublish-ai-toolkit

# 2. Agent Skills (works with 41+ agents — Claude Code, Cursor, Codex, Copilot, etc.)
npx skills add azeemkafridi/bulkpublish-ai-toolkit

# 3. MCP Server (any MCP-compatible tool)
claude mcp add bulkpublish -- npx -y @bulkpublish/mcp-server
```

---

## Setup by Tool

### Claude Code

**Option A — Plugin (recommended, auto-updates):**

```bash
/plugin marketplace add azeemkafridi/bulkpublish-ai-toolkit
/plugin install bulkpublish@bulkpublish-ai-toolkit
```

You'll be prompted for your API key during installation.

**Option B — Agent Skills:**

```bash
npx skills add azeemkafridi/bulkpublish-ai-toolkit
```

Or install a single skill:

```bash
npx skills add azeemkafridi/bulkpublish-ai-toolkit --skill schedule-post
```

**Option C — MCP only:**

```bash
claude mcp add bulkpublish -- npx -y @bulkpublish/mcp-server
```

Then set your API key:

```bash
export BULKPUBLISH_API_KEY=bp_your_key_here
```

---

### Claude Desktop

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "bulkpublish": {
      "command": "npx",
      "args": ["-y", "@bulkpublish/mcp-server"],
      "env": {
        "BULKPUBLISH_API_KEY": "bp_your_key_here"
      }
    }
  }
}
```

Restart Claude Desktop to activate.

---

### Cursor

**Option A — Agent Skills:**

```bash
npx skills add azeemkafridi/bulkpublish-ai-toolkit -a cursor
```

**Option B — MCP config.** Create or edit `.cursor/mcp.json` in your project root (or globally at `~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "bulkpublish": {
      "command": "npx",
      "args": ["-y", "@bulkpublish/mcp-server"],
      "env": {
        "BULKPUBLISH_API_KEY": "bp_your_key_here"
      }
    }
  }
}
```

---

### Windsurf

**Option A — Agent Skills:**

```bash
npx skills add azeemkafridi/bulkpublish-ai-toolkit -a windsurf
```

**Option B — MCP config.** Edit `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "bulkpublish": {
      "command": "npx",
      "args": ["-y", "@bulkpublish/mcp-server"],
      "env": {
        "BULKPUBLISH_API_KEY": "${env:BULKPUBLISH_API_KEY}"
      }
    }
  }
}
```

Set your key in your shell environment:

```bash
export BULKPUBLISH_API_KEY=bp_your_key_here
```

Or hardcode it directly in the `env` block above.

---

### VS Code (GitHub Copilot)

**Option A — Agent Skills:**

```bash
npx skills add azeemkafridi/bulkpublish-ai-toolkit -a github-copilot
```

**Option B — MCP config.** Add to your VS Code `settings.json` (Cmd+Shift+P > "Preferences: Open User Settings (JSON)"):

```json
{
  "mcp": {
    "servers": {
      "bulkpublish": {
        "command": "npx",
        "args": ["-y", "@bulkpublish/mcp-server"],
        "env": {
          "BULKPUBLISH_API_KEY": "bp_your_key_here"
        }
      }
    }
  }
}
```

---

### Gemini CLI

**Option A — CLI command:**

```bash
gemini mcp add bulkpublish -- npx -y @bulkpublish/mcp-server
```

**Option B — Edit `~/.gemini/settings.json`:**

```json
{
  "mcpServers": {
    "bulkpublish": {
      "command": "npx",
      "args": ["-y", "@bulkpublish/mcp-server"],
      "env": {
        "BULKPUBLISH_API_KEY": "$BULKPUBLISH_API_KEY"
      }
    }
  }
}
```

Set your key:

```bash
export BULKPUBLISH_API_KEY=bp_your_key_here
```

---

### OpenAI Codex CLI

**Option A — Agent Skills:**

```bash
npx skills add azeemkafridi/bulkpublish-ai-toolkit -a codex
```

**Option B — CLI command:**

```bash
codex mcp add bulkpublish -- npx -y @bulkpublish/mcp-server
```

**Option C — Edit `~/.codex/config.toml`:**

```toml
[mcp_servers.bulkpublish]
command = "npx"
args = ["-y", "@bulkpublish/mcp-server"]

[mcp_servers.bulkpublish.env]
BULKPUBLISH_API_KEY = "bp_your_key_here"
```

---

### ChatGPT (Remote MCP)

ChatGPT requires a remote HTTP MCP endpoint (no local stdio). BulkPublish does not currently host a remote MCP server. For ChatGPT integration, use the [BulkPublish Zapier integration](https://zapier.com/apps/bulkpublish) or the [REST API](https://app.bulkpublish.com/docs) directly.

---

### Other MCP-Compatible Tools

Any tool that supports the [Model Context Protocol](https://modelcontextprotocol.io) can connect to BulkPublish. The general pattern:

**Stdio transport:**
```
command: npx
args: -y @bulkpublish/mcp-server
env: BULKPUBLISH_API_KEY=bp_your_key_here
```

---

## Skills (Claude Code Plugin)

If you installed via the Claude Code plugin, you get these skills:

| Skill | Command | Description |
|---|---|---|
| Schedule Post | `/bulkpublish:schedule-post` | Create and schedule posts with optimal timing |
| Get Analytics | `/bulkpublish:get-analytics` | Pull performance reports and engagement data |
| Manage Channels | `/bulkpublish:manage-channels` | View connected accounts and troubleshoot issues |
| Bulk Upload | `/bulkpublish:bulk-upload` | Upload media and schedule posts in batch |
| Check Quota | `/bulkpublish:check-quota` | View plan limits and current usage |

---

## Examples

Once connected, just ask your AI assistant in natural language:

```
"Schedule a post about our new feature to Twitter and LinkedIn for tomorrow at 9am EST"

"How did my posts perform this week?"

"Upload these 5 images and schedule them as daily Instagram posts"

"Which of my channels have connection issues?"

"How many posts do I have left on my plan today?"
```

---

## All 29 MCP Tools

<details>
<summary>Click to expand full tool list</summary>

**Posts:**
`create_post`, `update_post`, `delete_post`, `list_posts`, `get_post`, `publish_post`, `retry_post`, `get_post_metrics`, `publish_story`, `bulk_posts`

**Channels:**
`list_channels`, `get_channel_health`, `get_channel_options`, `search_mentions`

**Media:**
`upload_media`, `list_media`, `get_media`, `delete_media`

**Labels:**
`list_labels`, `create_label`, `update_label`, `delete_label`

**Analytics:**
`get_analytics`

**Schedules:**
`list_schedules`, `create_schedule`, `update_schedule`, `delete_schedule`

**Quota:**
`get_quota_usage`

**Queue:**
`get_queue_slot`

</details>

---

## Other Integrations

BulkPublish is available across many platforms:

| Integration | Install |
|---|---|
| **Node.js SDK** | `npm install bulkpublish` |
| **Python SDK** | `pip install bulkpublish` |
| **Homebrew CLI** | `brew tap azeemkafridi/bulkpublish && brew install bulkpublish` |
| **Zapier** | [Search "BulkPublish" in Zapier](https://zapier.com/apps/bulkpublish) |
| **n8n** | `n8n-nodes-bulkpublish` (Community Nodes) |
| **Make.com** | Search "BulkPublish" in Make |
| **IFTTT** | [ifttt.com/services/bulkpublish](https://ifttt.com/services/bulkpublish) |
| **REST API** | [app.bulkpublish.com/docs](https://app.bulkpublish.com/docs) |
| **Postman** | [Import collection](https://github.com/azeemkafridi/bulkpublish-api/blob/main/postman-collection.json) |

---

## Links

- [BulkPublish](https://bulkpublish.com) — Main site
- [API Docs](https://app.bulkpublish.com/docs) — Interactive API reference
- [Developer Dashboard](https://app.bulkpublish.com/developer) — API keys & usage
- [MCP Server on npm](https://www.npmjs.com/package/@bulkpublish/mcp-server) — `@bulkpublish/mcp-server`
- [GitHub](https://github.com/azeemkafridi/bulkpublish-api) — SDKs, examples, guides

## License

MIT
