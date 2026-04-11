---
name: manage-channels
description: List connected social media channels, check their health, and troubleshoot connection issues. Use when the user asks about their connected accounts or channel status.
allowed-tools: Bash(npx *) Read
---

# Manage Channels

Help the user view and troubleshoot their connected social media accounts.

## Workflow

1. **List channels** — call `list_channels` to see all connected accounts
   - Returns: channel ID, platform, account name, token health status

2. **Check health** — for any channel the user is concerned about, call `get_channel_health`
   - Returns: token validity, connection status, expiry info, issues

3. **Get options** — call `get_channel_options` to see platform-specific capabilities:
   - Pinterest: available boards
   - YouTube: playlists, categories
   - LinkedIn: organization pages
   - Instagram: eligible collaborators

4. **Search mentions** — if the user needs to find accounts to tag, use `search_mentions`
   - Works on X/Twitter and Bluesky
   - Returns matching usernames for @mentions

## Common Issues

- **Token expired**: User needs to reconnect at app.bulkpublish.com/channels
- **Rate limited**: Platform is throttling — suggest waiting or reducing post frequency
- **Scope missing**: Channel was connected with limited permissions — reconnect with full scopes

## Notes

- Channels can only be connected/reconnected via the web UI (OAuth flow)
- The API can list, check health, and get options — but not create new connections
