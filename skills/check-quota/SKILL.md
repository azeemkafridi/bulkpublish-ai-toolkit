---
name: check-quota
description: Check BulkPublish plan limits, usage, and remaining quota. Use when the user asks about their plan, limits, remaining posts, or storage.
allowed-tools: Bash(npx *) Read
---

# Check Quota & Plan Usage

Help the user understand their current plan limits and usage.

## Workflow

1. **Get quota** — call `get_quota_usage`
   - Returns: plan name, daily/monthly post limits, scheduled post limit, channel limit, media storage used/total, API call limits

2. **Present clearly**:
   - Current plan tier (Free, Pro, Business)
   - Usage vs limits for each resource
   - Highlight anything near the limit (>80% used)

3. **Recommendations**:
   - If hitting post limits: suggest upgrading or optimizing schedule
   - If hitting storage: suggest cleaning up unused media with `list_media` + `delete_media`
   - If hitting channel limits: suggest upgrading plan

## Plan Tiers

| Resource | Free | Pro | Business |
|---|---|---|---|
| Posts/day | 5 | 50 | 200 |
| Channels | 3 | 15 | 50 |
| Storage | 500MB | 5GB | 25GB |
| Scheduled posts | 10 | 100 | 500 |
| API calls/day | 100 | 5,000 | 25,000 |

## Notes

- Quota resets daily at midnight UTC for daily limits
- Storage is cumulative — delete old media to free space
- Upgrade at app.bulkpublish.com/settings/billing
