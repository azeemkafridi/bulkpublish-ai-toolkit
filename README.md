# BulkPublish AI Toolkit

Connect your AI assistant to [BulkPublish](https://bulkpublish.com) — schedule posts, manage channels, upload media, and track analytics across 14 social media platforms, all from your AI tool of choice.

## What You Get

**51 MCP tools** covering the full BulkPublish API, including interactive MCP Apps widgets:

| Category | Tools | Examples |
|---|---|---|
| **Posts** | 12 | Create, schedule, publish, retry, delete, approve, reject, bulk actions, metrics |
| **Channels** | 4 | List accounts, health check, platform options, @mention search |
| **Media** | 4 | Upload (URL), list, get, delete |
| **Labels** | 4 | Create, list, update, delete |
| **Schedules** | 4 | Create recurring schedules, update, list, delete |
| **Analytics** | 1 | Summary by date range with per-platform breakdown |
| **Quota** | 1 | Plan usage and limits |
| **Queue** | 1 | Next optimal posting slot |

Plus three REST-only endpoint groups documented in the skills (no MCP tools yet): **Channel Sets** (saved channel groups for one-click targeting, up to 50 per org), **RSS Autopost** (up to 20 feeds per org, polled every 15 min — new items become draft or auto-published posts), and **multipart media upload** (chunked 10MB parts with per-part retry — videos up to 1GB).

**Team approval:** posts carry an `approvalStatus` (`none` default | `pending` | `approved` | `rejected`) that is separate from `status` — the scheduler never publishes a pending or rejected post. Pass `requestApproval: true` (default `false`) when creating or updating a post to hold it for review; it is forced on for API keys whose role lacks `post:publish` (contributors), who also get `403 APPROVAL_REQUIRED` from publish/retry. Owners, admins and approvers release posts with `approve_post` / `reject_post`.

**Link tracking:** BulkPublish can rewrite a post's links to `bulkpubli.sh` short URLs and count the clicks — the only way to measure outbound clicks, since platform APIs do not report them (X dropped the field entirely). It is off by default and opt-in per organization; `linkTrackingOverride` (`true` | `false` | `null`, default `null`) overrides it per post. It is **tri-state — omit it unless the user asked**, because `false` is an explicit "post the links as written", not the same as inheriting. Clicks come back as `linkClicks` / `totalLinkClicks`, which are measured by us and therefore available on every platform, and are deliberately **not** part of `clicks`/`totalClicks` — one visit can register in both, so never add them together. Shortening is skipped on any channel where the rewrite would exceed that platform's character limit (a short URL is 28 characters and can be longer than the link it replaces), so tracking can silently not apply on X or Bluesky; the post still publishes with its original links.

**Supported platforms:** Facebook, Instagram, X/Twitter, TikTok, YouTube, Threads, Bluesky, Pinterest, Google Business Profile, LinkedIn, Mastodon, Discord, Telegram, Tumblr, Snapchat

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

### Hosted server (ChatGPT, claude.ai, and other web hosts)

Web-based hosts can't spawn a local process, so they need a remote HTTP endpoint
instead of the `npx` stdio setups above. BulkPublish hosts one:

```
https://mcp.bulkpublish.com/mcp
```

It serves the same tool suite as the stdio server over Streamable HTTP, and
authenticates two ways:

- **OAuth 2.1 (recommended).** Add the URL as a custom connector and the host
  walks you through the consent screen, where you paste your `bp_` API key once.
  No key ever lives in the connector's URL.
- **Key in the URL,** for hosts that don't do OAuth:
  `https://mcp.bulkpublish.com/mcp?key=bp_your_key_here`. Treat that URL as a
  secret — anyone holding it holds your API key.

Get a key at [app.bulkpublish.com/developer](https://app.bulkpublish.com/developer).

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
| Platform Reference | `/bulkpublish:platform-reference` | All 14 platforms — post types, media rules, required fields, limits |
| Schedule Post | `/bulkpublish:schedule-post` | Create and schedule posts with optimal timing, incl. the team approval flow |
| Get Analytics | `/bulkpublish:get-analytics` | Pull performance reports and engagement data |
| Manage Channels | `/bulkpublish:manage-channels` | View connected accounts, channel sets, and troubleshoot issues |
| Bulk Publish | `/bulkpublish:bulk-publish` | Upload media (multipart for videos up to 1GB) and publish posts in batch |
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

"Set up my blog's RSS feed so new articles land as drafts for my LinkedIn and X channels"

"Schedule this for Friday 9am but hold it for my editor to approve"

"Show me everything waiting on approval, then approve the LinkedIn one"
```

---

## All 51 MCP Tools

<details>
<summary>Click to expand full tool list</summary>

**Posts:**
`create_post`, `update_post`, `delete_post`, `list_posts`, `get_post`, `publish_post`, `retry_post`, `approve_post`, `reject_post`, `get_post_metrics`, `publish_story`, `bulk_posts`

**Channels:**
`list_channels`, `get_channel_health`, `get_channel_options`, `search_mentions`

**Platforms:**
`list_platforms`

**Media:**
`upload_media`, `list_media`, `get_media`, `delete_media`, `create_media_upload`, `finalize_media_upload`, `create_multipart_upload`, `complete_multipart_upload`, `abort_multipart_upload`

**Labels:**
`list_labels`, `create_label`, `update_label`, `delete_label`

**Analytics:**
`get_analytics`

**Channel Sets:**
`list_channel_sets`, `create_channel_set`, `update_channel_set`, `delete_channel_set`

**RSS Autopost:**
`list_rss_feeds`, `create_rss_feed`, `update_rss_feed`, `delete_rss_feed`

**Schedules:**
`list_schedules`, `create_schedule`, `update_schedule`, `delete_schedule`

**Quota:**
`get_quota_usage`

**Queue:**
`get_queue_slot`

**Interactive UI (MCP Apps):**
`compose_post`, `view_analytics`, `view_posts`, `view_channels`, `view_media`, `view_quota`

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
