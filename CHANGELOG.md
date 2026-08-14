# Changelog

## 1.4.3 (2026-08-15)

- **get-analytics skill: metric support matrix re-synced with the server's docs
  audit.** Reddit reports likes/comments/shares and Discord reports
  likes/comments — both were wrongly listed as reporting nothing; X now
  reports `saves` (bookmarks). Only Google Business, Telegram and Tumblr report
  no per-post metrics.

## 1.4.2 (2026-08-08)

- **schedule-post skill: `firstComment` is not a real parameter.** The field list documented `firstComment (string)` as a top-level post param. No such field exists in the API or the MCP tools — an agent following this would pass an unknown key and get no first comment, silently. The auto-reply is `_firstComment` at the **top level of `platformSpecific`**. Also noted that it is unsupported on Discord, Pinterest, TikTok, Google Business and Tumblr, where the main post still publishes and the comment is recorded as failed.

- **Corrects 1.4.1: `reddit.thumbnailUrl` is NOT optional.** The previous entry (and the skill) said Reddit video posts fall back to the video's auto-extracted poster frame. They do not — only the Pinterest half of that change shipped. `webapp/src/lib/platforms/reddit.ts` reads `thumbnailUrl` from `platformSpecific` and fails the publish when it is absent, so **Reddit video posts require it explicitly**. The skill now contrasts this with Pinterest's `coverImageUrl` so the two are not assumed to behave alike.
- **platform-reference skill: Reddit options corrected against the handler.** `title` is **not** required (it defaults to the first line of `content`, truncated to 300 chars); `flairText`, `nsfw` and `spoiler` **do not exist** and were removed; `subreddit` accepts `r/webdev` and `/r/webdev`, not just the bare name; `type` was missing. `platformSpecific.reddit` is keyed by **channel ID** (like Tumblr), which the skill never said. Added the derived post-kind rules, the exactly-one-media-file limit, and the two publish gotchas: Reddit returns HTTP 200 on a rule rejection, and media submissions confirm asynchronously with a 20-second timeout after which a post is failed but may still have appeared — verify before retrying.
- **platform-reference skill: Discord's required `channelId` documented.** The section listed no `platformSpecific` at all, so a post built from it would fail with "No Discord channel selected". Added that a connected Discord channel is an entire *server*, that the inner `channelId` is the Discord text-channel snowflake rather than the BulkPublish channel id, the 10 × 25MB attachment limit, and that publishing uses a global bot token — so failures are permission problems, never a reconnect issue. Dropped the mention of webhooks, which the server does not use.
- **platform-reference skill: Telegram documented as taking no `platformSpecific` options.** Added the URL-fetch media caps (5MB images / 20MB video, well below Telegram's upload limits), that content is sent with no parse mode so Markdown is not rendered, and that text over the 1,024-character caption limit arrives as a second message rather than being truncated.

## 1.4.1 (2026-08-08)

- **platform-reference skill: `pinterest.coverImageUrl` is now optional.** The server falls back to an attached image, then the video's auto-extracted poster frame; publishing fails only when all three are missing.
- **platform-reference skill: `reddit.thumbnailUrl` documented, optional.** Reddit video posts fall back to the video's auto-extracted poster frame when no thumbnail is set.

## 1.4.0 (2026-08-01)

- **Link tracking documented across the skills.** `schedule-post` gains `linkTrackingOverride` (tri-state, default `null`) with an explicit instruction to omit it unless the user asked for a behaviour — `false` is a real "post the links as written", not the same as unset. `get-analytics` gains `linkClicks` / `totalLinkClicks`, including the two rules that matter: never add them to the platform's own `clicks`, and read a 0 as "no tracked link clicks" rather than "nobody clicked", since it also covers tracking being off, the post having no links, or shortening being skipped for a channel's character limit.
- README summarises both, alongside the note that shortening can silently not apply on X and Bluesky.

## 1.3.1 (2026-07-29)

- **LinkedIn company pages are gated separately from personal profiles.** `list_platforms` entries can now carry a `variants` object keyed by channel `accountType`; LinkedIn reports `variants.organization` for company pages, which run on a separate LinkedIn app (Community Management API) with its own review. Pages can be paused for new connections while personal-profile posting is fully live — an agent reading only the platform-level `state` would wrongly tell a user LinkedIn is unavailable, or wrongly offer a company-page connect that can only 403. When a variant blocks a write, the `PLATFORM_DISABLED` error carries an `accountType` naming it.

## 1.3.0 (2026-07-28)

- **Per-metric platform support in `get-analytics`.** Every platform entry of `get_post_metrics` now carries `metricsSupported` and `supportedMetrics`, and the engagement response adds `metricSupport` / `supportedTotals` / `partialTotals` / `conditionalMetrics`. All metric columns are stored as integers defaulting to `0`, so a metric a platform never reports was indistinguishable from a real zero — an agent reading the raw response would confidently tell the user "0 impressions" for a Bluesky post, which has no impressions metric at all. The skill now carries the full per-platform table and an explicit rule: **never report a 0 without checking `supportedMetrics` first.**
- **`metricsDisabledChannels`** documented — X reads are billed, so its per-post sync is opt-in per channel and runs at most weekly. Until it is enabled on the Channels page every X figure stays 0 and refreshing cannot change it.
- Documented the two other legitimate reasons a figure is 0: the 6-hourly snapshot has not run since publish, and Facebook's impressions/reach/clicks need the `read_insights` permission.
- Server-side fixes reflected here: Pinterest now reports likes and comments (previously always 0), Threads post metrics now return at all, and Bluesky now reports saves via bookmarks.
- Plugin description corrected: 14 -> 15 platforms (Tumblr, shipped in 1.2.0).

## 1.2.0 (2026-07-25)

- **Tumblr support (15th platform).** `tumblr` added to the platform character-limit map (32,768) and display names. Tumblr posts accept up to 30 images **or** exactly one video — never both in the same post.
- **Platform availability.** Platforms can now be disabled server-side. Post creation targeting a disabled platform returns 403 `PLATFORM_DISABLED` (distinct from `FEATURE_DISABLED`, which means the plan doesn't include it). Posts already scheduled when a platform is disabled are **held, not failed**, and publish automatically once it is re-enabled — no need to delete and recreate them. `GET /api/platforms` reports the current state of every platform.
- Skills and README updated: platform count 14 -> 15, `tumblr` added to the channel platform-key list, and a full Tumblr entry in the platform reference (post type, media rules, and the channel-ID-keyed `platformSpecific.tumblr` options).

## 1.1.0 (2026-07-24)

- **Team approval** documented across the skills and README:
  - `requestApproval` (boolean, **default `false`**) on post create/update holds a scheduled post for team approval; **forced server-side** for API keys of members whose role lacks `post:publish` (contributors), regardless of the flag.
  - Post objects carry `approvalStatus` (**default `"none"`** | `pending` | `approved` | `rejected`), `approvedBy`, `approvedAt`, `rejectionReason`. `approvalStatus` is orthogonal to `status`: the scheduler skips `pending` and `rejected` posts even when scheduled and overdue.
  - New MCP tools `approve_post` and `reject_post` (mcp-server ≥1.7.0, `POST /api/posts/{id}/approve|reject`) — require a role with `post:approve` (owner, admin, approver); 400 if the post is not awaiting approval, 403 without the role, 404 if not found. Rejection takes an optional `reason` (max 2000 chars).
  - `list_posts` gained the `approvalStatus` filter (the approval queue is `approvalStatus: "pending"`).
  - `publish_post` / `retry_post` return **403 `APPROVAL_REQUIRED`** for roles without `post:publish` — agents must submit for approval instead of retrying.
- Tool count updated to 39 (Posts: 12).

## 1.0.5 (2026-07-18)

- **RSS custom-field tokens** in `schedule-post`: documented that a feed item's own extra leaf fields (namespaced or not) can be used as `{fieldName}` caption tokens beyond the standard set; the webapp editor surfaces a feed's real fields as pills after a preview.

## 1.0.4 (2026-07-18)

- **RSS field mapping** documented in `schedule-post`: the new `fieldMapping` field on `POST/PUT /api/rss-feeds` (caption template with item tokens, `mediaField` enclosure selection with media-required-platform skipping, `stripHtml`, `truncate` smart/hard/skip, `hashtags`, per-channel `channelOverrides`; `null` clears to the default).
- Corrected stale note: RSS autopost now HAS MCP tools (`list/create/update/delete_rss_feed`, mcp-server ≥1.5.0).

## 1.0.3 (2026-07-17)

- Documented three new REST endpoint groups (no MCP tools yet) across skills + README:
  - **Channel Sets** (`/api/channel-sets`) — saved channel groups, 50/org cap, names unique per org (409 `DUPLICATE_NAME`), in `manage-channels`.
  - **RSS Autopost** (`/api/rss-feeds`) — 20 feeds/org, `mode` defaults to `draft` (vs `publish` = auto-publish), changing `feedUrl` re-baselines the feed (no backlog flood), in `schedule-post`.
  - **Multipart media upload** (`/api/media/multipart/create|complete|abort`) — 10MB parts with per-part ETags and retry, videos up to 1GB, in `bulk-publish`.

## 1.0.2 (2026-07-16)

- Skills/docs: 14 platforms (added Reddit, Discord, Telegram); Google Business is `gmb`, not `google_business`.
- Corrected schedule/queue-slot guidance: `get_queue_slot` takes only an optional `timezone`.
- Completed the postFormat set (post/video/reel/story/carousel/thread) and the 7-value post status set.

