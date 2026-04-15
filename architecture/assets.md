# THE MOON RECORDS — ASSET CONVENTION
## Architecture Document — maintained by Void

---

## LOCATION

All release assets live **outside the OS repo** in a sibling directory:

```
~/Sites/The Moon/
├── the-moon-os/          ← the OS (this repo)
├── the-moon-web/         ← website (future)
└── the-moon-assets/      ← audio, artwork — never committed to git
    └── tmr-001-logic-moon-example/
        ├── audio/
        ├── artwork/
        └── release.json
```

`the-moon-assets/` is never a git repository. It lives locally and/or on cloud storage (iCloud, Dropbox). It is not public.

---

## FOLDER NAMING CONVENTION

```
[catalog-number]-[artist-slug]-[title-slug]/
```

Examples:
- `tmr-001-logic-moon-example/`
- `pg-002-cortexia-title-slug/`

Must match the corresponding folder in `releases/` in the OS repo exactly.

---

## FOLDER STRUCTURE

```
pg-xxx-artist-title/
├── audio/
│   ├── 01_track-title.wav
│   ├── 02_track-title.wav
│   └── ...
├── artwork/
│   ├── cover.png             ← primary cover — 3000×3000px minimum
│   └── cover_alt.png         ← optional variants
└── release.json              ← source of truth — see schema below
```

---

## AUDIO FILE RULES

| Rule | Requirement |
|---|---|
| Format | WAV only — 24bit / 44.1kHz minimum |
| Naming | `[track-number]_[track-title-slug].wav` — zero-padded, lowercase, underscores |
| No spaces | Never in filenames |
| Master only | No stems, no alternative mixes in this folder |

Examples:
- `01_vector_field.wav`
- `02_null_signal.wav`

---

## ARTWORK RULES

| Rule | Requirement |
|---|---|
| Resolution | 3000×3000px minimum |
| Format | PNG or TIFF |
| Color space | RGB |
| Filename | `cover.png` — always this name, no variation |

---

## RELEASE.JSON — SCHEMA

`release.json` is the source of truth for all release metadata. It is human-maintained — not auto-generated. The validation script reads from it.

```json
{
  "release_id": "PG-001",
  "artist": "Input Null",
  "members": ["Void", "Paradroid"],
  "title": "Vector Field Signals",
  "format": "EP",
  "label": "The Moon Records",
  "catalog": "PG-001",
  "release_date": "YYYY-MM-DD",
  "tracks": [
    {
      "number": 1,
      "title": "Track Title",
      "file": "01_track_title.wav",
      "duration": "6:42"
    }
  ],
  "artwork": {
    "cover": "cover.png",
    "min_resolution": "3000x3000",
    "format": "PNG or TIFF"
  },
  "platforms": {
    "bandcamp": null,
    "spotify": null,
    "soundcloud": null
  },
  "notes": ""
}
```

### Required fields

All fields except `notes` and `distribution` URLs are required before validation passes.
`null` values indicate pending — the validator will flag them.

---

## TOOLCHAIN

All toolchain commands run via the `pg` CLI bootstrapper. First-time setup:

```bash
bash tools/setup.sh
```

### `generate` — scan audio, write release.json

Scans `audio/` for WAV/AIFF files. Reads duration, sample rate, bit depth. Merges with any existing `release.json`, preserving manually entered fields.

```bash
tools/tmr generate ~/Sites/Phantom\ Grid/the-moon-assets/tmr-001-input-null-vector-field-signals/
```

Output: pre-filled `release.json` with all detectable data. Manual fields flagged.

### `validate` — check release folder against schema

Checks:
1. `release.json` exists and is valid JSON
2. All required fields are non-null
3. Every `file` listed in `tracks[]` exists in `audio/`
4. WAV files follow naming convention
5. `cover.png` exists in `artwork/`
6. Cover resolution meets 3000×3000px minimum

```bash
tools/tmr validate ~/Sites/Phantom\ Grid/the-moon-assets/tmr-001-input-null-vector-field-signals/
```

Add `--generate-md` to output a `release_draft.md` if all checks pass:

```bash
tools/tmr validate /path/to/release/ --generate-md
```
