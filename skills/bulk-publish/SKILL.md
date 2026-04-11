---
name: bulk-publish
description: Upload media files (local or URL), manage media library, and batch-create posts via BulkPublish. Use when the user wants to upload files or publish content in bulk.
---

# BulkPublish — Media & Bulk Publishing Reference

## upload_media parameters

```
url       (string, optional) — public URL to download from
filePath  (string, optional) — absolute local file path (e.g. /Users/me/photo.png)
filename  (string, optional) — override filename, otherwise derived from url/path
```

Provide either `url` OR `filePath`, not both.

## Supported formats

| Type | Formats | Max size |
|---|---|---|
| Image | JPEG, PNG, WebP, GIF | 100MB |
| Video | MP4, MOV, WebM | 100MB |

## Response

Returns: `id` (use this in `mediaFileIds` when creating posts), `fileName`, `mimeType`, `sizeBytes`, `width`, `height`, `duration` (video), `originalUrl`, `thumbnailUrl`, `previewUrl`.

## Other media tools

| Tool | Use for | Key params |
|---|---|---|
| `list_media` | Browse uploaded files | `search`, `page`, `limit` |
| `get_media` | Single file details | `mediaId` |
| `delete_media` | Remove a file | `mediaId` |

## Media requirements by platform

| Platform | Image | Video | Notes |
|---|---|---|---|
| Instagram Reels | — | MP4, 9:16 | 3-90 seconds |
| Instagram Stories | JPEG/PNG, 9:16 | MP4, 9:16 | 1080x1920 recommended |
| Instagram Carousel | JPEG/PNG | — | 2-10 images |
| TikTok | — | MP4 | 1-10 minutes |
| YouTube | — | MP4 | Requires title in platformSpecific |
| Pinterest | JPEG/PNG, 2:3 | MP4 | 1000x1500 recommended |
| Facebook/X/LinkedIn | JPEG/PNG | MP4 | Most formats accepted |

## Bulk pattern

For multiple files: call `upload_media` for each file, collect the returned IDs, then pass all IDs in `mediaFileIds` when creating the post.
