# RELEASE WORKFLOW
## The Moon Records — Schritt-für-Schritt

---

## PHASE 1 — FOLDER SETUP

```
releases/
└── tmr-NNN-artist-title/
    ├── audio/          ← WAV/AIFF Master-Files hier rein
    ├── artwork/        ← Cover (min. 3000×3000px, PNG oder JPG)
    └── export/         ← wird automatisch erstellt
```

**1.1** Release-Ordner anlegen (Naming: `tmr-NNN-artist-title`, alles lowercase)
**1.2** WAV/AIFF Masters in `audio/` legen
**1.3** Cover-Datei in `artwork/` legen

---

## PHASE 2 — GENERATE & VALIDATE

```bash
# release.json aus Audio-Files generieren
./tools/tmr generate ../the-moon-assets/tmr-NNN-artist-title

# Metadaten in release.json ausfüllen:
# artist, title, label, catalog, genre, tracklist

# Validieren + release.md Draft generieren
./tools/tmr validate ../the-moon-assets/tmr-NNN-artist-title --generate-md
```

**2.1** `release.json` öffnen und alle Felder ausfüllen
**2.2** Validate läuft grün durch → Status in `release.md` auf `IN_PRODUCTION` setzen
---

## PHASE 3 — CONVERT (MP3 für Distribution)

```bash
# WAV → MP3 320k + ID3 Tags + Cover einbetten
./tools/tmr convert ../the-moon-assets/tmr-NNN-artist-title
```

**Output:** `export/mp3/` — fertige MP3s mit Tags, bereit für Bandcamp/plaforms

---

## PHASE 4 — PUSH (CDN + releases.json)

```bash
# Verbindung testen, ohne etwas hochzuladen
./tools/tmr sftp-test

# Audio + Artwork aufs CDN, Release in data/releases.json eintragen
./tools/tmr push ../the-moon-assets/tmr-NNN-artist-title
```

**Vorher prüfen:** `about.md` darf nicht mehr der Generator-Platzhalter sein —
der Text landet in `data/releases.json` und damit auf der Website und in
Promo-Mails.

**Der Schritt geht nach außen** (CDN-Upload) und wird vorher freigegeben.

**Danach — sonst erscheint nichts auf der Website:**

```bash
# 1. im OS-Repo
git add data/releases.json && git commit -m "feat: tmr-NNN" && git push

# 2. im Web-Repo das Submodule nachziehen
cd ../the-moon-web
git submodule update --remote the-moon-os
git add the-moon-os && git commit -m "Update the-moon-os: tmr-NNN" && git push
```

Erst der Push auf `the-moon-web` löst den Build aus. Ein Push in `the-moon-os`
allein bewirkt nichts — siehe [AGENTS.md](../AGENTS.md#5-fallstricke).

---

## PHASE 5 — SOCIAL ASSETS

```bash
# social.json Skeleton erstellen und konfigurieren
./tools/tmr social ../the-moon-assets/tmr-NNN-artist-title --init

# social.json bearbeiten:
# start (Sekunde), duration, show_waveform, show_text per Track

# Square (Instagram Post) rendern
./tools/tmr social ../the-moon-assets/tmr-NNN-artist-title --format square

# Reel rendern
./tools/tmr social ../the-moon-assets/tmr-NNN-artist-title --format reel

# Beide auf einmal
./tools/tmr social ../the-moon-assets/tmr-NNN-artist-title --format all
```

**Output:** `export/social/square/` und `export/social/reel/`

---

## PHASE 6 — BANDCAMP

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

## PHASE 7 — DISTRIBUTION (Spotify/Apple Music etc.)

> Noch nicht vollständig definiert — Distributor steht aus.

1. MP3s aus `export/mp3/` + Cover bei Distributor einreichen
2. Metadaten aus `release.json` übertragen
3. Release-Datum ca. 3–4 Wochen im Voraus setzen (Spotify-Pitching)
4. ISRCs und UPC notieren → in `release.json` eintragen

> Die DSP-Links selbst werden **nicht** hier eingetragen, sondern später mit
> dem Scraper eingesammelt, sobald das Release auf den Plattformen live ist
> → siehe [Phase 10](#phase-10--dsp-links-nachtragen).

---

## PHASE 8 — INSTAGRAM / SOCIAL

1. Assets aus `export/social/square/` → Instagram Post
2. Assets aus `export/social/reel/` → Instagram Reel
3. Caption aus `workflows/platform-texts.md` übernehmen

---

## PHASE 9 — ABSCHLUSS

**8.1** Status in `release.md` auf `RELEASED` setzen
**8.2** Release-Datum und alle URLs in `release.md` → `plaforms` eintragen
**8.3** Commit: `release(TMR-NNN): released`

---

## PHASE 10 — DSP-LINKS NACHTRAGEN

> Zeitpunkt: **einige Tage bis Wochen nach dem Release-Datum.** Frisch
> veröffentlichte Titel sind auf den Plattformen noch nicht auffindbar — der
> Scraper liefert dann korrekterweise keine Treffer.

Der Scraper nimmt die Bandcamp-URL eines Releases, sucht dasselbe Release auf
den DSPs und gibt die gefundenen Links als JSON aus.

**Repo:** `/Users/tobe/Sites/logic-moon.de/dsp_scraper`
(Python + Playwright, eigenes venv via `uv`)

```bash
cd /Users/tobe/Sites/logic-moon.de/dsp_scraper

# ein einzelnes Release
uv run cli.py https://logicmoon.bandcamp.com/album/the-north

# JSON auf stdout statt in results.json (zum Weiterverarbeiten)
uv run cli.py --stdout https://logicmoon.bandcamp.com/album/the-north
```

**Aktive Plattformen:** Spotify, Apple Music, Amazon Music, SoundCloud,
Deezer, Beatport. YouTube Music, Tidal, Napster und Pandora sind im Repo
vorhanden, aber deaktiviert (Consent-Wall bzw. kein nutzbares Such-DOM).

### Ergebnisse in `data/releases.json` übernehmen

Die Treffer landen unter `platforms` des jeweiligen Release:

```json
{ "name": "spotify", "url": "https://open.spotify.com/album/..." }
```

Namensschema wie beim Scraper: `spotify`, `apple_music`, `amazon_music`,
`soundcloud`, `deezer`, `beatport` — identisch mit den Dateinamen der Logos
in `the-moon-web/src/assets/dsp/`, sonst fehlt das Icon.

**Regeln beim Übernehmen:**

- **Nur ergänzen, nie überschreiben.** Handgepflegte Links (vor allem
  Bandcamp) haben Vorrang. Der Vorgang ist dadurch beliebig wiederholbar.
- **Leere Platzhalter füllen**, nicht überspringen — ein Eintrag wie
  `{"name": "spotify", "url": ""}` blockiert sonst den echten Treffer und wird
  im Frontend ohnehin ausgefiltert.
- **Jeden Treffer gegen den Künstlernamen prüfen.** Der Scraper matcht primär
  über den Titel; bei ähnlichen Titeln entstehen Fehltreffer. Beim Lauf im
  August 2026 waren 3 von 46 falsch, u. a. ein SoundCloud-Upload eines fremden
  Künstlers und ein Apple-Music-Album mit anderem Namen.
- **Confidence unter 85 immer manuell prüfen**, ebenso Treffer ohne
  `matched_artist`.

### Bekannte Eigenheiten

| Plattform | Hinweis |
|---|---|
| Deezer | niedrige Trefferquote — Ergebniszeilen sind clientseitig gerenderte `div`s ohne `href` |
| Beatport | Katalog ist DJ/Dance-lastig; Ambient-Releases fehlen dort meist zu Recht |
| Amazon Music | kein Logo in Simple Icons (von Amazon untersagt) — Icon stammt aus einem Altbestand |

---

## QUICK REFERENCE

| Schritt | Befehl |
|---|---|
| Folder setup | manuell |
| Generate JSON | `./tools/tmr generate <path>` |
| Validate | `./tools/tmr validate <path> --generate-md` |
| Convert MP3 | `./tools/tmr convert <path>` |
| Push CDN + releases.json | `./tools/tmr push <path>` |
| Social init | `./tools/tmr social <path> --init` |
| Social render | `./tools/tmr social <path> --format all` |
| DSP-Links (Wochen später) | `uv run cli.py <bandcamp-url>` im `dsp_scraper`-Repo |

## DATEI-ÜBERSICHT NACH ABSCHLUSS

```
tmr-NNN-artist-title/
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
