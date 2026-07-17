# Changelog

## 1.0.3 (2026-07-17)

- Documented three new REST endpoint groups (no MCP tools yet) across skills + README:
  - **Channel Sets** (`/api/channel-sets`) — saved channel groups, 50/org cap, names unique per org (409 `DUPLICATE_NAME`), in `manage-channels`.
  - **RSS Autopost** (`/api/rss-feeds`) — 20 feeds/org, `mode` defaults to `draft` (vs `publish` = auto-publish), changing `feedUrl` re-baselines the feed (no backlog flood), in `schedule-post`.
  - **Multipart media upload** (`/api/media/multipart/create|complete|abort`) — 10MB parts with per-part ETags and retry, videos up to 1GB, in `bulk-publish`.

## 1.0.2 (2026-07-16)

- Skills/docs: 14 platforms (added Reddit, Discord, Telegram); Google Business is `gmb`, not `google_business`.
- Corrected schedule/queue-slot guidance: `get_queue_slot` takes only an optional `timezone`.
- Completed the postFormat set (post/video/reel/story/carousel/thread) and the 7-value post status set.

