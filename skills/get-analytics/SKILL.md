---
name: get-analytics
description: Pull analytics, engagement metrics, and performance data for social media posts. Use when the user asks about post performance, engagement, or wants a report.
allowed-tools: Bash(npx *) Read
---

# Get Analytics & Metrics

Help the user understand how their social media content is performing.

## Workflow

1. **Determine scope** — ask the user what they want:
   - Overall analytics for a date range? Use `get_analytics`
   - Metrics for a specific post? Use `get_post_metrics`
   - Quota/usage overview? Use `get_quota_usage`

2. **Get analytics summary** — call `get_analytics` with:
   - `startDate`: ISO date (e.g., "2026-04-01")
   - `endDate`: ISO date (e.g., "2026-04-11")
   - Returns: total posts, status breakdown, per-platform stats, daily counts

3. **Get post-level metrics** — call `get_post_metrics` with `postId`:
   - Returns: likes, comments, shares, impressions, reach, clicks
   - Metrics vary by platform

4. **Present results** clearly:
   - Summarize key numbers (total reach, engagement rate, top-performing posts)
   - Break down by platform if relevant
   - Highlight trends (growth, decline, best days/times)

## Common Queries

- "How did my posts do this week?" -> `get_analytics` with last 7 days
- "Which post got the most engagement?" -> `list_posts` then `get_post_metrics` for top ones
- "Am I hitting my limits?" -> `get_quota_usage`
- "Show me Instagram performance" -> `get_analytics` then filter by platform
