---
name: schedule-post
description: Schedule social media posts across multiple platforms with optimal timing. Use when the user wants to create, schedule, or publish posts to social media.
allowed-tools: Bash(npx *) Read Grep
---

# Schedule a Social Media Post

Help the user create and schedule a post using the BulkPublish MCP tools.

## Workflow

1. **Gather content** from the user — text, images/videos (URLs), and target platforms
2. **Check channels** — call `list_channels` to see which platforms are connected and healthy
3. **Validate content** — check character limits per platform:
   - X/Twitter: 280 chars (25,000 for long posts)
   - Instagram: 2,200 chars
   - Facebook: 63,206 chars
   - LinkedIn: 3,000 chars
   - TikTok: 2,200 chars
   - YouTube: 5,000 chars (description)
   - Threads: 500 chars
   - Bluesky: 300 chars
   - Pinterest: 500 chars
   - Google Business: 1,500 chars
   - Mastodon: 500 chars
4. **Upload media** if needed — call `upload_media` with the file URL
5. **Create the post** — call `create_post` with:
   - `channelIds`: array of channel IDs to publish to
   - `content`: the post text
   - `scheduledAt`: ISO 8601 datetime (or omit for draft)
   - `mediaFileIds`: array of uploaded media IDs
   - `platformContent`: platform-specific overrides if content differs per platform
   - `postTypeOverrides`: for stories, reels, threads, carousels, etc.
6. **Confirm** — show the user what was scheduled, when, and to which platforms

## Platform-Specific Post Types

Use `postTypeOverrides` for special formats:
- Instagram: `reel`, `story`, `carousel`
- Facebook: `story`, `reel`
- TikTok: `slideshow`
- YouTube: `short`
- X/Twitter: `thread`
- Threads: `thread`
- Bluesky: `thread`
- Mastodon: `thread`

## Tips

- If the user doesn't specify a time, use `get_queue_slot` to find the optimal slot
- For multi-platform posts with different content per platform, use `platformContent`
- Always show a preview/summary before creating the post
