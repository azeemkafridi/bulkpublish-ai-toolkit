---
name: get-analytics
description: Pull analytics, engagement metrics, and performance data from BulkPublish. Use when the user asks about post performance or engagement.
---

# BulkPublish — Analytics Reference

## Tools

| Tool | Use for | Key params |
|---|---|---|
| `get_analytics` | Overall summary for a date range | `startDate`, `endDate` (ISO dates) |
| `get_post_metrics` | Single post engagement | `postId` |
| `get_quota_usage` | Plan limits and current usage | none |
| `list_posts` | Find posts to analyze | `status`, `from`, `to`, `channelId`, `search` |

## get_analytics response shape

Returns: total posts, status breakdown (published/failed/scheduled/draft), per-platform counts, daily post counts for the range.

## get_post_metrics response shape

Returns per-platform: likes, comments, shares, impressions, reach, clicks, saves. Metrics vary by platform — not all platforms report all fields.

## get_quota_usage response shape

Returns: plan name, limits (channels, posts/day, storage, API calls), current usage for each, subscription status.

## Patterns

- "How did my posts do this week?" → `get_analytics` with last 7 days
- "Which post got the most likes?" → `list_posts` (status: "published") then `get_post_metrics` for each
- "Am I near my limits?" → `get_quota_usage`
- Date params are ISO date strings: "2026-04-01", not datetime
