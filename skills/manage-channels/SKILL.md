---
name: manage-channels
description: List connected social media channels and check their health via BulkPublish. Use when the user asks about their connected accounts.
---

# BulkPublish — Channels Reference

## Tools

| Tool | Use for | Key params |
|---|---|---|
| `list_channels` | All connected accounts with IDs and status | none |
| `get_channel_health` | Token validity and connection issues | `channelId` |
| `get_channel_options` | Platform-specific options | `channelId` |
| `search_mentions` | Find @mention usernames | `channelId`, `query` |

## list_channels response shape

Each channel: `id`, `platform`, `accountName`, `accountId`, `accountType`, `isActive`, `tokenStatus` ("valid"/"expired"/"error"), `tokenExpiresAt`.

## Platform names (used in channels and create_post)

`facebook`, `instagram`, `x`, `linkedin`, `tiktok`, `youtube`, `pinterest`, `threads`, `bluesky`, `gmb`, `mastodon`, `reddit`, `discord`, `telegram`

## Channel options by platform

| Platform | get_channel_options returns |
|---|---|
| Pinterest | Available boards |
| YouTube | Playlists, categories |
| Instagram | Eligible collaborators |

## Notes

- Channels can only be connected/reconnected via the web UI (OAuth) — not via API
- LinkedIn channels are either a personal profile or a company page (`accountType`: `personal` / `organization`); both are connected in the dashboard and posted to like any other channel
- `search_mentions` works on X/Twitter and Bluesky only
- Token "expired" means the user needs to reconnect at app.bulkpublish.com/channels
