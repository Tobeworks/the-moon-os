# RELEASE WORKFLOW
## The Moon Records — Schritt-für-Schritt

---

## PHASE 1 — FOLDER SETUP

```
releases/
└── pg-NNN-artist-title/
    ├── audio/          ← WAV/AIFF Master-Files hier rein
    ├── artwork/        ← Cover (min. 3000×3000px, PNG oder JPG)
    └── export/         ← wird automatisch erstellt
```

**1.1** Release-Ordner anlegen (Naming: `pg-NNN-artist-title`, alles lowercase)
**1.2** WAV/AIFF Masters in `audio/` legen
**1.3** Cover-Datei in `artwork/` legen

---

## PHASE 2 — GENERATE & VALIDATE

```bash
# release.json aus Audio-Files generieren
./pg generate releases/pg-NNN-artist-title

# Metadaten in release.json ausfüllen:
# artist, title, label, catalog, genre, tracklist

# Validieren + release.md Draft generieren
./pg validate releases/pg-NNN-artist-title --generate-md
```

**2.1** `release.json` öffnen und alle Felder ausfüllen
**2.2** Validate läuft grün durch → Status in `release.md` auf `IN_PRODUCTION` setzen
---

## PHASE 3 — CONVERT (MP3 für Distribution)

```bash
# WAV → MP3 320k + ID3 Tags + Cover einbetten
./pg convert releases/pg-NNN-artist-title
```

**Output:** `export/mp3/` — fertige MP3s mit Tags, bereit für Bandcamp/plaforms

---

## PHASE 4 — SOCIAL ASSETS

```bash
# social.json Skeleton erstellen und konfigurieren
./pg social releases/pg-NNN-artist-title --init

# social.json bearbeiten:
# start (Sekunde), duration, show_waveform, show_text per Track

# Square (Instagram Post) rendern
./pg social releases/pg-NNN-artist-title --format square

# Reel rendern
./pg social releases/pg-NNN-artist-title --format reel

# Beide auf einmal
./pg social releases/pg-NNN-artist-title --format all
```

**Output:** `export/social/square/` und `export/social/reel/`

---

## PHASE 5 — BANDCAMP

1. **bandcamp.com/track/new** oder **album/new** öffnen
2. MP3s aus `export/mp3/` hochladen
3. Cover aus `artwork/` hochladen
4. Metadaten aus `release.json` übertragen:
   - Title, Artist, Label, Catalog Number
   - Release-Datum setzen
5. Preis setzen (oder "Name your price")
6. Als Draft speichern → Review → Publish
7. Bandcamp-URL in `release.md` unter `DISTRIBUTION` eintragen

---

## PHASE 6 — DISTRIBUTION (Spotify/Apple Music etc.)

> Noch nicht vollständig definiert — Distributor steht aus.

1. MP3s aus `export/mp3/` + Cover bei Distributor einreichen
2. Metadaten aus `release.json` übertragen
3. Release-Datum ca. 3–4 Wochen im Voraus setzen (Spotify-Pitching)
4. ISRCs und UPC notieren → in `release.json` eintragen

---

## PHASE 7 — INSTAGRAM / SOCIAL

1. Assets aus `export/social/square/` → Instagram Post
2. Assets aus `export/social/reel/` → Instagram Reel
3. Caption aus `workflows/platform-texts.md` übernehmen

---

## PHASE 8 — ABSCHLUSS

**8.1** Status in `release.md` auf `RELEASED` setzen
**8.2** Release-Datum und alle URLs in `release.md` → `plaforms` eintragen
**8.3** Commit: `release(PG-NNN): released`

---

## QUICK REFERENCE

| Schritt | Befehl |
|---|---|
| Folder setup | manuell |
| Generate JSON | `./pg generate <path>` |
| Validate | `./pg validate <path> --generate-md` |
| Convert MP3 | `./pg convert <path>` |
| Social init | `./pg social <path> --init` |
| Social render | `./pg social <path> --format all` |

## DATEI-ÜBERSICHT NACH ABSCHLUSS

```
pg-NNN-artist-title/
├── audio/                  ← Masters (WAV/AIFF)
├── artwork/                ← Cover
├── export/
│   ├── mp3/                ← plaforms-ready MP3s
│   └── social/
│       ├── square/         ← Instagram Posts
│       └── reel/           ← Instagram Reels
├── release.json            ← Machine-readable Metadaten
├── release.md              ← Master Record
└── social.json             ← Social Asset Config
```
