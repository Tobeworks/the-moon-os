# AGENTS.md

Einstiegspunkt für Agents, die an The Moon Records arbeiten.

Diese Datei erklärt **wie die Teile zusammenhängen** und **was verbindlich gilt**.
Die Detailanleitungen stehen in den verlinkten Dateien und werden hier bewusst
nicht wiederholt — zwei Beschreibungen desselben Vorgangs driften auseinander.

---

## 1. Die vier Repos

| Repo | Rolle | Pfad |
|---|---|---|
| **the-moon-os** | Quelle der Wahrheit: `data/releases.json`, Toolchain, Brand-Doku | `/Users/tobe/Sites/The Moon/the-moon-os` |
| **the-moon-web** | Astro-Website. Enthält **the-moon-os als Submodule** und liest daraus die Releases | `/Users/tobe/Sites/The Moon/the-moon-web` |
| **the-moon-assets** | Rohmaterial pro Release: WAV-Masters, Cover, erzeugte Exporte | `/Users/tobe/Sites/The Moon/the-moon-assets` |
| **dsp_scraper** | Findet Releases auf Streaming-Plattformen (externes Repo) | `/Users/tobe/Sites/logic-moon.de/dsp_scraper` |

---

## 2. Datenfluss — vom Master zur Website

```
the-moon-assets/tmr-NNN-artist-title/     WAV + Cover
        │
        │  ./tools/tmr convert            → MP3 320k/128k, Artwork-Varianten
        ▼
        │  ./tools/tmr push               → Upload CDN, Eintrag in data/releases.json
        ▼
the-moon-os/data/releases.json            ← Quelle der Wahrheit
        │
        │  git commit + git push          im OS-Repo
        ▼
the-moon-web/the-moon-os (Submodule)      ← MUSS nachgezogen werden
        │
        │  git submodule update --remote
        │  git add the-moon-os && git commit && git push
        ▼
GitHub Action (build.yaml)                ← löst NUR bei Push auf the-moon-web aus
        ▼
ghcr.io/tobeworks/the-moon-web:latest     → Deployment
```

**Der häufigste Fehler:** Release gepusht, aber das Submodule in `the-moon-web`
nicht nachgezogen. Die Website zeigt dann weiter den alten Stand — ohne dass
irgendwo eine Warnung erscheint (siehe Fallstricke).

---

## 3. Toolchain

Kanonischer Einstieg ist `./tools/tmr` (aktiviert das venv selbst):

```bash
./tools/tmr generate <asset-ordner>     # release.json aus den Audio-Files
./tools/tmr validate <asset-ordner>     # gegen Schema prüfen
./tools/tmr convert  <asset-ordner>     # WAV → MP3 + ID3 + Artwork-Varianten
./tools/tmr push     <asset-ordner>     # CDN-Upload + data/releases.json
./tools/tmr social   <asset-ordner>     # Social-Assets
./tools/tmr sftp-test                   # CDN-Verbindung prüfen, ohne Upload
```

`--force` bei `convert` überschreibt bestehende Exporte. Nötig, wenn sich das
Cover geändert hat — sonst behalten die MP3s das alte Bild in den ID3-Tags.

Vollständige Toolchain-Doku: [tools/README.md](tools/README.md)
Release-Ablauf Schritt für Schritt: [workflows/release-workflow.md](workflows/release-workflow.md)

---

## 4. Ein Release anlegen — Kurzfassung

1. Ordner in `the-moon-assets/` als `tmr-NNN-artist-title` mit `audio/` und `artwork/`
2. `./tools/tmr generate <pfad>` — erzeugt `release.json` und `about.md`
3. `release.json` ausfüllen: `release_id`, `catalog`, `artist`, `title`, `release_date`
4. `about.md` schreiben — landet als Releasetext auf der Website und in Promo-Mails.
   Der Generator legt einen Platzhalter an; **niemals ungeprüft pushen**
5. `./tools/tmr validate <pfad>`
6. `./tools/tmr convert <pfad>`
7. `./tools/tmr push <pfad>` — **geht nach außen, vorher freigeben lassen**
8. `data/releases.json` committen und pushen
9. Submodule in `the-moon-web` nachziehen (siehe Datenfluss oben)

DSP-Links folgen erst Wochen später:
[Phase 9](workflows/release-workflow.md#phase-10--dsp-links-nachtragen)

---

## 5. Fallstricke

**Submodule meldet sich nicht von selbst.**
Git zeigt ein Submodule nur als geändert, wenn der ausgecheckte Commit vom
Pointer abweicht. Steht beides gleich, ist `git status` leer — auch wenn das
Remote längst weiter ist. Kein Fetch, kein Signal. Ein veralteter Pointer ist
für Git ein gültiger Zustand, kein Fehler.

**Der Website-Build hört nur auf `the-moon-web`.**
Ein Push in `the-moon-os` löst nichts aus. Und `submodules: recursive` im
Workflow holt exakt den Commit, den der Pointer nennt — nicht den neuesten.

**Leere Plattform-URLs blockieren.**
`{"name": "spotify", "url": ""}` zählt beim Zusammenführen als vorhanden und
verhindert den echten Link. Im Frontend wird der Eintrag ohnehin ausgefiltert.
Solche Platzhalter füllen, nicht überspringen.

**Zukünftiges Release-Datum blendet Links aus.**
Die Detailseite zeigt vor dem `release_date` keine Plattform-Links und
schreibt „Releasing" statt „Released". Das ist Absicht, kein Bug.

**Plattformnamen sind an Dateinamen gekoppelt.**
`spotify`, `apple_music`, `amazon_music`, `soundcloud`, `deezer`, `beatport`,
`bandcamp` — identisch mit den Logos in `the-moon-web/src/assets/dsp/`.
Weicht der Name ab, rendert die Komponente stillschweigend nur Text.

**Scraper-Treffer sind nicht automatisch richtig.**
Gematcht wird primär über den Titel. Immer gegen den Künstlernamen prüfen,
Confidence unter 85 und fehlenden `matched_artist` manuell nachsehen.

---

## 6. Arbeitsweise

- **Nichts committen oder pushen ohne ausdrückliche Anweisung.** Gilt für alle
  Repos, jedes Mal neu — eine frühere Freigabe gilt nicht fort.
- **CDN-Upload und Mailversand sind Aktionen nach außen** und brauchen dieselbe
  ausdrückliche Freigabe.
- Vor dem Ändern von `data/releases.json` ein Backup anlegen; die Datei ist die
  Quelle der Wahrheit für Website, Player und Promo-Mails.
- Website-Code: Tailwind, **kein `<style>`-Block** außer für Keyframes.
  Die Startseite ist die Referenz für Layout und Typografie.

---

## 7. Weiterführend

| Thema | Datei |
|---|---|
| Label-Kontext, Sitemap | [README.md](README.md) |
| Release-Ablauf im Detail | [workflows/release-workflow.md](workflows/release-workflow.md) |
| Toolchain | [tools/README.md](tools/README.md) |
| Asset-Konvention, release.json-Schema | [architecture/assets.md](architecture/assets.md) |
| Web-Architektur, OS→Web-Datenfluss | [architecture/web.md](architecture/web.md) |
| Brand, Farben, Typografie | [brand/the-moon-brand.md](brand/the-moon-brand.md) |
| Sound-Identität, A&R | [brand/sonic-brief.md](brand/sonic-brief.md) |
| Texte für Bandcamp, Instagram | [workflows/platform-texts.md](workflows/platform-texts.md) |
