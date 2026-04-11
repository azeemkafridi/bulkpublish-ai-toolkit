---
name: bulk-upload
description: Upload multiple media files and schedule bulk posts across platforms. Use when the user has a batch of content to publish or a content calendar to execute.
allowed-tools: Bash(npx *) Read Grep
---

# Bulk Upload & Schedule

Help the user upload media in bulk and schedule multiple posts efficiently.

## Workflow

1. **Gather content** — the user may provide:
   - A list of posts with text + media URLs
   - A CSV/file with content to schedule
   - A folder of images/videos to upload

2. **Upload media** — for each file, call `upload_media` with:
   - `url`: public URL of the image/video
   - `label`: optional label for organization
   - Supported: JPEG, PNG, WebP, GIF, MP4, MOV, WebM (max 100MB)
   - Track returned `mediaFileId` for each upload

3. **Check quota** — call `get_quota_usage` to verify the user has enough:
   - Daily post limit
   - Storage space
   - Scheduled post slots

4. **Schedule posts** — for each post, call `create_post` with:
   - Stagger scheduling times (use `get_queue_slot` for optimal slots)
   - Attach the uploaded media via `mediaFileIds`
   - Apply labels for organization

5. **Summary** — present a table of all scheduled posts:
   - Post content (truncated), platforms, scheduled time, media count, status

## Tips

- Use `list_labels` first and create labels with `create_label` if needed for batch organization
- For recurring content, consider `create_schedule` with a cron expression instead
- If the user provides a CSV, parse it and map columns to post fields
- Always confirm the schedule before creating all posts
