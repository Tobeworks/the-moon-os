# Promo Tool

Token-protected listening pages for DJs and journalists.
No account, no login — just a link with a token.

---

## Workflow

```
Label creates promo (PocketBase Admin)
  → generates link: the-moon-records.de/promo/pg-001?t=TOKEN
  → sends link by email to DJ
  → DJ opens link: listens to release, downloads tracks, leaves feedback
  → Label reads feedback in PocketBase Admin
```

---

## Admin UI

**Production:**
```
https://pb.the-moon-records.de/_/
```

**Local:**
```
http://localhost:8090/_/
```

Login with the superuser account (created on first start).
If password is lost:

```bash
kubectl exec -n the-moon-web deployment/pocketbase -- \
  /pb/pocketbase superuser upsert admin@the-moon-records.de NewPassword123!
```

---

## Creating a promo

1. Open PocketBase Admin → **Collections → promos → New record**
2. Fill in the fields:

| Field | Required | Description |
|---|---|---|
| `token` | ✅ | Unique access code, e.g. `PG001-DJ-KOOL-X7K` |
| `release_slug` | ✅ | Catalog ID lowercase, e.g. `pg-001` |
| `recipient_name` | ✅ | DJ name — shown on the page |
| `recipient_email` | — | Internal reference only |
| `notes` | — | Internal notes (not visible to DJ) |
| `expires_at` | — | Expiry date — page shows "EXPIRED" after this |

3. Save → build the link:

```
https://the-moon-records.de/promo/[release_slug]?t=[token]
```

**Example:**
```
https://the-moon-records.de/promo/pg-001?t=PG001-DJ-KOOL-X7K
```

---

## URL format

```
/promo/[release-slug]?t=[TOKEN]
```

- `release-slug` = `catalog` field from `releases.json`, lowercase (e.g. `pg-001`)
- `t` = token from the PocketBase promo record

---

## What the DJ sees

- Release info (cover, title, artist, format, genre)
- Recipient name: `FOR: DJ KOOL`
- Expiry date (if set)
- Audio player with 128k streaming
- Download buttons: individual tracks (128K / 320K) + ZIP of all tracks
- Feedback form (name, email, free text)
- All previous feedback for this release

---

## Collections

### `promos`

One promo = one DJ access for one release.

| Field | Type | Rule |
|---|---|---|
| `token` | Text, Unique | — |
| `release_slug` | Text | — |
| `recipient_name` | Text | — |
| `recipient_email` | Email | optional |
| `notes` | Text | optional, internal |
| `expires_at` | Date | optional |

**API Rules:** List/View public (empty string) — Astro reads without auth token.

### `feedback`

One entry per DJ feedback. Relation to `promos`.

| Field | Type |
|---|---|
| `promo` | Relation → promos |
| `name` | Text |
| `email` | Email |
| `comment` | Text |
| `user_agent` | Text (auto) |
| `ip` | Text (auto) |

**API Rules:** List/View/Create public — DJs write directly, label reads in Admin.

### `download_events`

Logged automatically on every download. Visible in Admin only.

| Field | Type |
|---|---|
| `promo` | Relation → promos |
| `quality` | Select: `128` / `320` |
| `user_agent` | Text |
| `ip` | Text |

---

## Download endpoints

### Single track

```
GET /api/promo/download?t=TOKEN&q=128&track=1
```

Returns MP3 file directly (Content-Disposition: attachment).

### ZIP all tracks

```
GET /api/promo/download?t=TOKEN&q=320
```

Streams ZIP archive of all tracks in the selected quality.
Filename format: `pg-001_input_null_320k.zip`

---

## Security

- Token validated server-side against PocketBase (never in client)
- `release_slug` in URL must match `release_slug` in promo record
- Expired promos return a 410 error page
- PocketBase is internal (ClusterIP) — only accessible via ingress `pb.the-moon-records.de`
- Download events are logged (IP + User-Agent)

---

## Migrations

Located in `tools/pocketbase/pb_migrations/`, applied automatically on PocketBase start:

| File | Description |
|---|---|
| `1_create_promos.js` | promos collection |
| `2_create_feedback.js` | feedback collection |
| `3_create_download_events.js` | download_events collection |
