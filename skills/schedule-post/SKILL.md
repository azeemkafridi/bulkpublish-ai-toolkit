---
name: schedule-post
description: Create, schedule, or publish social media posts via BulkPublish MCP. Use when the user wants to post to social media.
---

# BulkPublish — Post Creation Reference

## create_post parameters

```
content        (string, required) — post text
channels       (array, required)  — [{channelId: number, platform: string}]
                                    Get these from list_channels first
status         ("draft"|"scheduled") — default "draft"
scheduledAt    (ISO 8601 string)  — required when status is "scheduled"
timezone       (string)           — e.g. "America/New_York", "Asia/Karachi"
mediaFileIds   (number[])         — IDs from upload_media
platformContent (object)          — per-platform text: {"x": "Short", "linkedin": "Longer version"}
postTypeOverrides (object)        — per-platform format: {"instagram": "reel", "facebook": "story"}
postFormat     ("post"|"thread")  — "thread" requires threadParts
threadParts    (array)            — [{content: string, mediaFileIds?: number[]}], min 2 parts
firstComment   (string)           — auto-reply after publishing
```

## Post type overrides

| Platform | Types |
|---|---|
| Instagram | `reel`, `story`, `carousel` |
| Facebook | `story`, `reel` |
| TikTok | `slideshow` |
| YouTube | `short` |
| X/Twitter | `thread` |
| Threads | `thread` |
| Bluesky | `thread` |
| Mastodon | `thread` |

## Publishing flow

- **Draft then publish**: `create_post` (status: "draft") → `publish_post` (postId)
- **Schedule for later**: `create_post` (status: "scheduled", scheduledAt: "2026-04-12T09:00:00Z")
- **Optimal timing**: call `get_queue_slot` with channelId to get the best next slot

## Character limits

| Platform | Limit |
|---|---|
| X/Twitter | 280 (25,000 long posts) |
| Instagram | 2,200 |
| Facebook | 63,206 |
| LinkedIn | 3,000 |
| TikTok | 2,200 |
| YouTube | 5,000 (description) |
| Threads | 500 |
| Bluesky | 300 |
| Pinterest | 500 |
| Google Business | 1,500 |
| Mastodon | 500 |

## Common mistakes

- `channels` takes objects `{channelId, platform}`, NOT just IDs
- Always call `list_channels` first to get valid channelId + platform pairs
- `scheduledAt` must be in the future and in ISO 8601 format
- To publish immediately: create as draft, then call `publish_post`
- `mediaFileIds` are numbers from `upload_media`, not file paths
