---
name: platform-reference
description: Complete reference for all 11 BulkPublish platforms — post types, media requirements, required fields, character limits, and platformSpecific options. ALWAYS consult before creating posts.
---

# BulkPublish — Platform Reference

## Quick Rules

- **YouTube & TikTok** require video — NEVER include them for image-only posts
- **Pinterest** requires a title — set `platformSpecific.pinterest.title`
- **YouTube** requires a title — set `platformSpecific.youtube.title`
- **Instagram** defaults to `feed_photo` — set `postTypeOverrides.instagram` for video content
- Use `platformContent` for shorter text on platforms with low char limits (Bluesky 300, Pinterest/Threads/Mastodon 500)

---

## Facebook

| Field | Value |
|---|---|
| Platform key | `facebook` |
| Char limit | 63,206 |
| Title required | No |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `post` (default) | 0-10 images OR 0-1 video | Text-only OK. No mixing images+video |
| `reel` | 1 video required | Vertical short-form video |
| `story` | 1 image OR 1 video required | Disappears after 24h |

**Media specs:** Image max 10MB (jpg/png/gif/webp). Video max 2GB (mp4/mov).

**platformSpecific options:** `linkPreview.url`

---

## Instagram

| Field | Value |
|---|---|
| Platform key | `instagram` |
| Char limit | 2,200 |
| Title required | No |
| Account type | Business or Creator (personal rejected) |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `feed_photo` (default) | 1 image required | Single photo post |
| `feed_video` | 1 video required | Single video post |
| `reel` | 1 video required | Short-form video, 9:16 recommended |
| `story` | 1 image OR 1 video required | Disappears after 24h |
| `carousel` | 2-10 images/videos required | Mixed media OK |

**Media specs:** Image max 8MB (jpg/jpeg only). Video max 1GB (mp4/mov).

**platformSpecific options:**
- `collaborators` — string or array of collaborator usernames
- `trialReel` — boolean, trial reel (graduates based on performance)
- `thumbnailTimestamp` — number (seconds), video thumbnail offset

**IMPORTANT:** If you have video and don't set `postTypeOverrides.instagram`, it defaults to `feed_photo` and FAILS. Always set to `reel` or `feed_video` for video.

---

## X / Twitter

| Field | Value |
|---|---|
| Platform key | `x` |
| Char limit | 280 (25,000 for long posts) |
| Title required | No |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `tweet` (default) | 0-4 images OR 0-1 video | Text-only OK. No mixing images+video |
| `thread` | Per-part media | Use `postFormat: "thread"` + `threadParts` |

**Media specs:** Image max 5MB (jpg/png/gif/webp), max 4. Video max 512MB (mp4/mov), max 1, max 140s.

---

## YouTube

| Field | Value |
|---|---|
| Platform key | `youtube` |
| Char limit | 5,000 (description) |
| Title required | **YES** — `platformSpecific.youtube.title` (1-100 chars) |
| Video required | **YES** — always |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `video` (default) | 1 video required | Standard upload |
| `short` | 1 video required | Vertical, under 3 minutes |

**Media specs:** Video max 128GB (mp4/mov/avi/wmv/flv/webm), max 1.

**platformSpecific options:**
- `title` — **REQUIRED**, 1-100 chars (falls back to first line of content)
- `privacyStatus` — `public` (default), `private`, `unlisted`
- `categoryId` — YouTube category ID (default 22)
- `madeForKids` — boolean (default false)
- `tags` — string array
- `playlistId` — add to playlist after upload
- `thumbnailUrl` — custom thumbnail URL

**IMPORTANT:** YouTube ONLY accepts video. Never include YouTube channel for image-only posts.

---

## TikTok

| Field | Value |
|---|---|
| Platform key | `tiktok` |
| Char limit | 2,200 |
| Title required | No (uses content) |
| Video required | **YES** (or images for slideshow) |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `video` (default) | 1 video required | Standard video upload |
| `photo_slideshow` | 1-35 images required | Auto-creates slideshow with music |

**Media specs:** Video max 4GB (mp4/mov), max 1. Image max 20MB (jpg/jpeg/webp), max 35 for slideshow.

**platformSpecific options:**
- `privacyLevel` — `SELF_ONLY` (default), `PUBLIC`, `FRIENDS`
- `disableDuet` — boolean
- `disableStitch` — boolean
- `disableComment` — boolean
- `isAigc` — boolean (AI-generated content disclosure)
- `thumbnailTimestamp` — number (seconds)

**IMPORTANT:** TikTok ONLY accepts video (or images for slideshow). Default privacy is `SELF_ONLY` — set to `PUBLIC` for visibility.

---

## LinkedIn

| Field | Value |
|---|---|
| Platform key | `linkedin` |
| Char limit | 3,000 |
| Title required | No (optional for articles/videos) |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `post` (default) | 0-1 image OR 0-1 video | Text-only OK |
| `multi_image` | 2-20 images required | Image gallery |
| `pdf_carousel` | 2-20 images required | Converted to PDF carousel |
| `article` | 0-1 image (thumbnail) | Link share, requires `platformSpecific.url` |

**Media specs:** Image max 10MB (jpg/png/gif), max 20. Video max 500MB (mp4 only), max 1, 3s-30min.

**platformSpecific options:**
- `title` — article/video title
- `description` — article description
- `url` — **REQUIRED for article type**, link URL
- `carouselTitle` — PDF carousel title

---

## Pinterest

| Field | Value |
|---|---|
| Platform key | `pinterest` |
| Char limit | 500 |
| Title required | **YES** — `platformSpecific.pinterest.title` (1-100 chars) |
| Board required | **YES** — `platformSpecific.pinterest.boardId` (auto-creates "BulkPublish" board if missing) |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `pin` (default) | 1 image required | Standard pin, 2:3 ratio recommended |
| `video_pin` | 1 video required | Video pin |
| `carousel` | 2-5 images required | Multi-image pin |

**Media specs:** Image max 20MB (jpg/png/webp), max 5. 1000x1500 recommended. Video max 2GB (mp4/mov), max 1.

**platformSpecific options:**
- `title` — **REQUIRED**, 1-100 chars (falls back to first 100 chars of content)
- `description` — pin description, max 500 chars
- `link` — destination URL
- `boardId` — target board ID (get from `get_channel_options`)
- `dominantColor` — hex color
- `coverImageUrl` — video pin thumbnail

---

## Threads

| Field | Value |
|---|---|
| Platform key | `threads` |
| Char limit | 500 |
| Title required | No |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `text` (default) | None | Text-only post |
| `image` | 1 image required | Photo post |
| `video` | 1 video required | Video post |
| `carousel` | 2-20 images/videos | Mixed media OK |
| `thread` | Per-part | Use `postFormat: "thread"` + `threadParts` |

**Media specs:** Image max 8MB (jpg/png), max 20. Video max 500MB (mp4/mov), max 20.

**platformSpecific options:** `quotePostId` — quote another Threads post

---

## Bluesky

| Field | Value |
|---|---|
| Platform key | `bluesky` |
| Char limit | 300 |
| Title required | No |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `post` (default) | 0-4 images OR 0-1 video | Text-only OK. No mixing. |
| `thread` | Per-part | Use `postFormat: "thread"` + `threadParts` |

**Media specs:** Image max 10MB auto-resized to <976KB (jpg/png/webp), max 4. Video max 100MB (mp4 only), max 1, max 60s.

---

## Google Business Profile

| Field | Value |
|---|---|
| Platform key | `gmb` |
| Char limit | 1,500 |
| Title required | Only for events (`eventTitle`) |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `standard` (default) | 0-1 image | Business update |
| `event` | 0-1 image | Requires dates/times |
| `offer` | 0-1 image | Promotional offer |

**Media specs:** Image max 5MB (jpg/png), max 1. No video support.

**platformSpecific options:**
- `ctaType` — `BOOK`, `ORDER`, `SHOP`, `LEARN_MORE`, `SIGN_UP`, `CALL`
- `ctaUrl` — CTA destination URL (not needed for CALL)
- `eventTitle` — event title (required for event type)
- `startDate` / `endDate` — `YYYY-MM-DD` format
- `startTime` / `endTime` — `HH:MM` format
- `couponCode` — offer coupon code
- `redeemOnlineUrl` — offer redemption URL
- `termsConditions` — offer terms text

---

## Mastodon

| Field | Value |
|---|---|
| Platform key | `mastodon` |
| Char limit | 500 |
| Title required | No |

**Post types:**

| Type | Media | Notes |
|---|---|---|
| `post` (default) | 0-4 images OR 0-1 video | Text-only OK. No mixing. |
| `thread` | Per-part | Use `postFormat: "thread"` + `threadParts` |

**Media specs:** Image max 16MB (jpg/png/webp/avif/gif), max 4. Video max 99MB (mp4 only), max 1, max 300s.

**platformSpecific options:**
- `visibility` — `public` (default), `unlisted`, `private`, `direct`
- `spoilerText` — content warning text
- `language` — ISO 639-1 language code
